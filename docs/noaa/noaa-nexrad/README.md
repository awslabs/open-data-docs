# NOAA NEXRAD on AWS

The Next Generation Weather Radar (NEXRAD) is a network of 160 high-resolution Doppler radar sites that detects precipitation and atmospheric movement and disseminates data in approximately 5 minute intervals from each site. NEXRAD enables severe storm prediction and is used by researchers and commercial enterprises to study and address the impact of weather across multiple sectors.

The real-time feed and full historical archive of original resolution (Level II) NEXRAD data, from June 1991 to present, is now freely available on Amazon S3 for anyone to use. This is the first time the full NEXRAD Level II archive has been accessible to the public on demand. Now anyone can use the data on-demand in the cloud without worrying about storage costs and download time.

We are making NEXRAD data available as part of our research agreement with the US National Oceanic and Atmospheric Administration (NOAA) to enable new product development and analysis.

This page includes information on data structure and sample use cases to help you get started. You can find much more detailed information about NEXRAD Level II data from NOAA and other online sources.

## Accessing the Archive Data

The NEXRAD Level II archive data is hosted in the `noaa-nexrad-level2` Amazon S3 bucket in the us-east-1 AWS region. The address for the public bucket is: `https://noaa-nexrad-level2.s3.amazonaws.com`.

Each volume scan file of archival data is available as an object in Amazon S3. The basic data format is:

`/<Year>/<Month>/<Day>/<NEXRAD Station/>/<filename>`

Where:

- `<Year>` is the year the data was collected
- `<Month>` is the month of the year the data was collected
- `<Day>` is the day of the month the data was collected
- `<NEXRAD Station>` is the NEXRAD ground station (map of ground stations)
- `<filename>` is the name of the file containing the data. These are compressed files (compressed with gzip). The file name has more precise timestamp information.

All files in the archive use the same compressed format (.gz). The data file names are, for example, `KAKQ20010101_080138.gz`. The file naming convention is:

`GGGGYYYYMMDD_TTTTTT`

Where:

- `GGGG` = Ground station ID (map of ground stations)
- `YYYY` = year
- `MM` = month
- `DD` = day
- `TTTTTT` = time when data started to be collected (GMT)

Note that the 2015 files have an additional field on the file name. It adds “_V06” to the end of the file name. An example is `KABX20150303_001050_V06.gz`.

The full historical archive from NOAA from June 1991 to present is available.

### Accessing the Archive Data using AWS CLI

Using the AWS CLI is the most convenient way to get the data from S3. The CLI is a set of command line tools that enable functionality such as `ls`, `cp`, and `sync` for S3 buckets. To install the CLI read the [installation instructions](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-install.html) for your platform.

For example, to list all the data for May 6th, 2019 for the KTLX radar site do:

`aws s3 ls s3://noaa-nexrad-level2/2019/05/06/KTLX/ --no-sign-request`

The `--no-sign-request` flag enables you to run the command without providing credentials. This works because the bucket is publicly accessible.

To download all of the data for May 6th, 2019 for the KTLX radar site do:

`aws s3 cp s3://noaa-nexrad-level2/2019/05/06/KTLX/ . --recursive --no-sign-request`

Here the `--recursive` flag tells the CLI to grab all objects which begin with the keypath `s3://noaa-nexrad-level2/2019/05/06/KTLX/`.

## Accessing the Real-Time Data

The NEXRAD Level II real-time data is hosted in the `unidata-nexrad-level2-chunks` Amazon S3 bucket in the us-east-1 AWS Region. The address for the public bucket is: `https://unidata-nexrad-level2-chunks.s3.amazonaws.com/`

Each chunk of each volume scan file is its own object in Amazon S3. The basic data format is:

`/<Site>/<Volume_number>/<YYYYMMDD-HHMMSS-CHUNKNUM-CHUNKTYPE>`

Where:

- `<Site>` is the NEXRAD ground station (map of ground stations)
- `<Volume_Number>` is the volume id number (cycles from 0 to 999)
- `<YYYYMMDD>` is the date of the volume scan
- `<HHMMSS>` is the time of the volume scan
- `<CHUNKNUM>` is the chunk number
- `<CHUNKTYPE>` is the chunk type

All files in the real-time feed use bzip2 compression. Additional documentation on the chunks is available on the last page, B-1, of this Interface Control Document.

## Subscribing to NEXRAD Data Notifications

We have set up public Amazon SNS topics that create a notification for every new object added to the Amazon S3 chunks and archive buckets for NEXRAD on AWS. To start, you can subscribe to these notifications using Amazon SQS and AWS Lambda. This means you can automatically add new real-time and near-real-time NEXRAD data into a queue or trigger event-based processing if the data meets certain criteria such as geographic location.

The Amazon Resource Name (ARN) for the real-time data is:

`arn:aws:sns:us-east-1:684042711724:NewNEXRADLevel2ObjectFilterable`

Note that this topic providers [filterable fields](https://docs.aws.amazon.com/sns/latest/dg/sns-message-filtering.html) to allow you to specify the notifications you want to receive. The notification has the form of

```json
{
  Type: 'Notification',
  MessageId: '',
  TopicArn: 'arn:aws:sns:us-east-1:684042711724:NewNEXRADLevel2ObjectFilterable',
  Message: {
    S3Bucket: 'unidata-nexrad-level2-chunks',
    Key: 'KDFX/602/20190510-143508-028-I',
    SiteID: 'KDFX',
    DateTime: '2019-05-10T14:35:08',
    VolumeID: 602,
    ChunkID: 28,
    ChunkType: 'I',
    L2Version: 'V06
  },
  Timestamp: '2019-05-10T14:36:44.781Z',
  MessageAttributes: {
    SiteID: { Type: 'String', Value: 'KDFX' },
    VolumeID: { Type: 'String', Value: '602' },
    ChunkID: { Type: 'String', Value: '28' },
    ChunkType: { Type: 'String', Value: 'I' },
    L2Version: { Type: 'String', Value: 'V06' },
    DateTime: { Type: 'String', Value: '2019-05-10T14:35:08' }
  }
}
```

The ARN for the archive data is:

`arn:aws:sns:us-east-1:811054952067:NewNEXRADLevel2Archive`
