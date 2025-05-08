# OpenStreetMap on AWS

[OpenStreetMap (OSM)](https://www.openstreetmap.org/) is a free, editable map of the world, created and maintained by volunteers and available for use under an [open license](https://opendatacommons.org/licenses/odbl/1.0/). Anyone can use OSM to provide maps, directions, and geographic context to users around the world. Since 2004, editors have created and modified several billion features (representing physical things on the ground like roads or buildings). As new users join the open mapping community, more and more valuable data is being added to OpenStreetMap, requiring increasingly powerful tools, interfaces, and approaches to explore its vastness.

Regular OSM data archives are made available in Amazon S3 in a few different formats. This includes both snapshots of the current state of data in OSM as well as historical archives and changesets. There are two main sources:

1. **OSMF-managed buckets** - Containing authoritative OSM data in PBF format, including planet and history PBFs, changeset and discussion XML, and OsmChange replication files
2. **Cloud-native formats** - Containing OSM data in cloud-optimized formats like ORC

## Accessing OpenStreetMap on AWS

### OSMF-managed Authoritative Data (PBF format)

The OpenStreetMap Foundation (OSMF) manages authoritative OSM data in PBF format in two AWS regions:

- Primary bucket (EU Central 1): `osm-planet-eu-central-1` A web interface is available at [planet.openstreetmap.org](https://planet.openstreetmap.org).
- Replicated bucket (US West 2): `osm-planet-us-west-2`

These buckets contain:

- Planet PBF files (weekly snapshots)
- History PBF files
- Changeset and discussion XML files
- OsmChange replication files (minutely/hourly/daily updates)

To see all the files available in the primary bucket:

```
aws s3 --no-sign-request ls s3://osm-planet-eu-central-1
```

### Cloud-Native Formats

OSM data in cloud-optimized formats are housed in the US East (N. Virginia) Region and can be accessed by any tool that supports S3, including the AWS [Command Line Interface](https://aws.amazon.com/cli/).

To see all the files available:

```shell
aws s3 --no-sign-request ls s3://osm-pds
```

To download a planet file in ORC format:

```shell
aws s3 cp s3://osm-pds/planet/planet-latest.orc .
```

To create tables from the cloud-native files for use with Amazon Athena or other similar packages:

**Planet Table (current state):**

```sql
CREATE EXTERNAL TABLE planet (
  id BIGINT,
  type STRING,
  tags MAP<STRING,STRING>,
  lat DECIMAL(9,7),
  lon DECIMAL(10,7),
  nds ARRAY<STRUCT<REF:BIGINT>>,
  members ARRAY<STRUCT<TYPE:STRING,REF:BIGINT,ROLE:STRING>>,
  changeset BIGINT,
  timestamp TIMESTAMP,
  uid BIGINT,
  user STRING,
  version BIGINT,
  visible BOOLEAN
)
STORED AS ORCFILE
LOCATION 's3://osm-pds/planet/';
```

**Planet History Table (all historical feature versions):**

```sql
CREATE EXTERNAL TABLE planet_history (
    id BIGINT,
    type STRING,
    tags MAP<STRING,STRING>,
    lat DECIMAL(9,7),
    lon DECIMAL(10,7),
    nds ARRAY<STRUCT<ref: BIGINT>>,
    members ARRAY<STRUCT<type: STRING, ref: BIGINT, role: STRING>>,
    changeset BIGINT,
    timestamp TIMESTAMP,
    uid BIGINT,
    user STRING,
    version BIGINT,
    visible BOOLEAN
)
STORED AS ORCFILE
LOCATION 's3://osm-pds/planet-history/';
```

**Changesets Table:**

```sql
CREATE EXTERNAL TABLE changesets (
    id BIGINT,
    tags MAP<STRING,STRING>,
    created_at TIMESTAMP,
    open BOOLEAN,
    closed_at TIMESTAMP,
    comments_count BIGINT,
    min_lat DECIMAL(9,7),
    max_lat DECIMAL(9,7),
    min_lon DECIMAL(10,7),
    max_lon DECIMAL(10,7),
    num_changes BIGINT,
    uid BIGINT,
    user STRING
)
STORED AS ORCFILE
LOCATION 's3://osm-pds/changesets/';
```

## Receive Data Notifications with Amazon SNS

### OSMF-managed Data Notifications

The following SNS topics provide notifications whenever new data has been added to the OSMF-managed S3 buckets:

- `eu-central-1` (primary): `arn:aws:sns:eu-central-1:630658470130:osm-planet-eu-central-1-notifications`
- `us-west-2` (replicated): `arn:aws:sns:us-west-2:630658470130:osm-planet-us-west-2-notifications`

These topics publish Amazon [S3 event messages](http://docs.aws.amazon.com/AmazonS3/latest/dev/notification-content-structure.html) whenever new files have been created on Amazon S3.

### Cloud-Native Format Notifications

The following ARN is for an Amazon [SNS](https://aws.amazon.com/sns/) topic that provides notifications whenever new cloud-native format data has been added:

- `us-east-1`: `arn:aws:sns:us-east-1:800218804198:New_File`

If you are using tables that reference the cloud-native data directly (as above), they will automatically reflect changes when updates are made.

#### Subscribing to OSMF-managed SNS Topics

You can subscribe to these topics using either Amazon SQS or AWS Lambda. Here's how to set up a subscription using the AWS CLI:

1. **Create an SQS queue** in the same region as the SNS topic:

```shell
# For the EU Central 1 topic
aws sqs create-queue --queue-name osm-planet-notifications --region eu-central-1
```

This will return a queue URL like `https://sqs.eu-central-1.amazonaws.com/123456789012/osm-planet-notifications`

2. **Get the queue ARN** which you'll need for the policy:

```shell
aws sqs get-queue-attributes \
  --queue-url <YOUR_QUEUE_URL> \
  --attribute-names QueueArn \
  --region <REGION>
```

This will return the queue ARN in the format `arn:aws:sqs:<REGION>:<ACCOUNT_ID>:osm-planet-notifications`

3. **Set the queue policy** to allow the SNS topic to send messages to it:

```shell
aws sqs set-queue-attributes \
  --queue-url <YOUR_QUEUE_URL> \
  --attributes '{"Policy": "{\"Version\":\"2012-10-17\",\"Statement\":[{\"Effect\":\"Allow\",\"Principal\":{\"Service\":\"sns.amazonaws.com\"},\"Action\":\"sqs:SendMessage\",\"Resource\":\"<YOUR_QUEUE_ARN>\",\"Condition\":{\"ArnEquals\":{\"aws:SourceArn\":\"<SNS_TOPIC_ARN>\"}}}]}"}' \
  --region <REGION>
```

4. **Subscribe your SQS queue to the SNS topic**:

```shell
# For the EU Central 1 topic
aws sns subscribe \
  --topic-arn arn:aws:sns:eu-central-1:630658470130:osm-planet-eu-central-1-notifications \
  --protocol sqs \
  --notification-endpoint <YOUR_QUEUE_ARN> \
  --region eu-central-1
```

5. **Poll your SQS queue for messages**:

```shell
aws sqs receive-message --queue-url <YOUR_QUEUE_URL> --region <REGION> | \
  jq ".Messages[] | .Body | fromjson | .Message | fromjson"
```

These notifications will inform you when new OSM data is available, allowing you to automate downstream processing.


