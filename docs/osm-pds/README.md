# OpenStreetMap on AWS

OpenStreetMap (OSM) is a free, editable map of the world, created and maintained by volunteers and available for use under an [open license](https://opendatacommons.org/licenses/odbl/1.0/). Anyone can use OSM to provide maps, directions, and geographic context to users around the world. In the 12 years of OSM’s existence, editors have created and modified several billion features (physical things on the ground like roads or buildings). As new users join the open mapping community, more and more valuable data is being added to OpenStreetMap, requiring increasingly powerful tools, interfaces, and approaches to explore its vastness.

Regular OSM data archives are made available in Amazon S3 in a few different formats (ORC, PBF). This includes both snapshots of the current state of data in OSM as well as historical archives and changesets.

## Accessing OpenStreetMap on AWS
All data are housed in the Amazon S3 US-East region and can be accessed by any tool that supports S3, including the AWS [Command Line Interface](https://aws.amazon.com/cli/).

To see all the files available:

`aws s3 ls osm-pds`


To download a planet file in PBF format:

`aws s3 cp s3://osm-pds/2017/planet-170501.osm.pbf .`

To create a table from the planet file for use with Amazon Athena or other similar packages:

```
CREATE EXTERNAL TABLE planet (
  id BIGINT,
  type STRING,
  tags MAP,
  lat DECIMAL(9,7),
  lon DECIMAL(10,7),
  nds ARRAY<STRUCT>,
  members ARRAY<STRUCT>,
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

## Receive Data Notifications with Amazon SNS

The following ARN is for an Amazon [SNS](https://aws.amazon.com/sns/) topic that provides notifications whenever new data has been added to OSM on AWS:

`arn:aws:sns:us-east-1:800218804198:New_File`

This topic publishes an Amazon [S3 event message](http://docs.aws.amazon.com/AmazonS3/latest/dev/notification-content-structure.html) whenever a new file has been created on Amazon S3. It will only accept subscriptions via Amazon [SQS](https://aws.amazon.com/sqs/) or AWS [Lambda](https://aws.amazon.com/lambda/).