# NOAA High-Resolution Rapid Refresh (HRRR)

An archive of HRRR data is available in
`s3://noaa-hrrr-bdp-pds`.

### File Structure

The key layout of data in both buckets is:

`Parameter/Vertical Level/YYYYMMDD/HH00/FFF`

`FFF` represents the forecast hour.

### File Format

The files are in GRIB format. Within each GRIB file are Climate and
Forecast v1.6 metedata that describes the data contained.

### SNS Topic

This Amazon SNS topic publishes a notification for every new object
added to the Amazon S3 GFS chunks on AWS. You can subscribe to this
Amazon SNS topic using [Amazon SQS](https://aws.amazon.com/sqs/) and
[AWS Lambda](https://aws.amazon.com/lambda/).

`arn:aws:sns:us-east-1:123901341784:NewHRRRObject`

### Further Documentation

There is a general introdcution to HRRR at
<https://rapidrefresh.noaa.gov/hrrr/>
