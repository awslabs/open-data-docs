# MODIS on AWS

Moderate Resolution Imaging Spectroradiometer (MODIS) data is available for anyone to use via Amazon S3. Select products are made available each day, often within hours of production. MODIS is a joint effort of the U.S. Geological Survey and NASA.

The MODIS instrument is operating on both the Terra and Aqua spacecraft, in orbit since 1999 and 2002, respectively. It has a viewing swath width of 2,330 km and views the entire surface of the Earth every one to two days. Its detectors measure 36 spectral bands and it acquires data at three spatial resolutions: 250-m, 500-m, and 1,000-m. The MCD43A4.006, MOD09GA.006, MYD09GA.006, MOD09GQ.006 and MYD09GQ.006 products are available for use on Amazon S3, more details on these below.

Learn more about MODIS data on [NASA's MODIS site](https://modis.gsfc.nasa.gov/).

## Accessing MODIS on AWS

Each file is its own object in the `modis-pds` Amazon S3 bucket. An object key will look like the following:

`/product/horizontal_grid/vertical_grid/date/DATA`

For example, the files for an individual scene are available in the following location: `modis-pds/MCD43A4.006/21/07/2011360/`

where:

- `product` = MCD43A4.006 - MODIS product ID
- `horizontal_grid` = 21 - horizontal grid in the MODIS Sinusoidal Tiling System
- `vertical_grid` = 07 - vertical grid in the MODIS Sinusoidal Tiling System
- `date` = 2011360 - year and three-digit day of year representation
- `DATA` - individual object (e.g., granule, metadata or thumbnail)

If you use the AWS Command Line Interface, you can list all available files, download an individual granule, or read the metadata file in the commands below.

`aws s3 ls modis-pds`

`aws s3 cp s3://modis-pds/MCD43A4.006/21/11/2017006/MCD43A4.A2017006.h21v11.006.2017018074804_B01.TIF MCD43A4.A2017006.h21v11.006.2017018074804_B01.TIF`

`aws s3 cp  s3://modis-pds/MCD43A4.006/21/11/2017006/MCD43A4.A2017006.h21v11.006.2017018074804_meta.json -`

## About the Data

### MCD43A4, version 6

The MODIS MCD43A4 version 6 Nadir Bidirectional reflectance distribution function Adjusted Reflectance (NBAR) data set is a daily 16-day product. The Julian date in the granule ID of each specific file represents the 9th day of the 16 day retrieval period, and consequently the observations are weighted to estimate the Albedo for that day. The MCD43A4 algorithm, as is with all combined products, has the luxury of choosing the best representative pixel from a pool that includes all the acquisitions from both the Terra and Aqua sensors from the retrieval period.

The MCD43A4 provides the 500 meter reflectance data of the MODIS "land" bands 1-7 adjusted using the bidirectional reflectance distribution function to model the values as if they were collected from a nadir view.

`aws s3 ls modis-pds/MCD43A4.006/`

### MOD09GA & MYD09GA, version 6

The MOD09GA &  MYD09GA version 6 products provide an estimate of the surface spectral reflectance of Terra and Aqua MODIS Bands 1-7 corrected for atmospheric conditions such as gasses, aerosols, and Rayleigh scattering. Provided along with the 500 m reflectance and four observation bands are a set of nine 1 km observation bands. The reflectance layers form the product are used as the source data for many of the MODIS land products.

*Please note that these products are only stored on Amazon S3 for a period of 30 days after which they will be removed.*

`aws s3 ls modis-pds/MOD09GA.006/`
`aws s3 ls modis-pds/MYD09GA.006/`

### MOD09GQ & MYD09GQ, version 6

The MOD09GQ & MYD09GQ version 6 products provide an estimate of the surface spectral reflectance of Terra and Aqua MODIS 250 m bands 1-2 corrected for atmospheric conditions such as gasses, aerosols, and Rayleigh scattering. Along with the 250 m bands are the QC 250 m layer and five observation layers. This product is intended to be used in conjunction with the quality and viewing geometry information of the 500 m product (MOD09GA & MYD09GA).

*Please note that these products are only stored on Amazon S3 for a period of 30 days after which they will be removed.*

`aws s3 ls modis-pds/MOD09GQ.006/`
`aws s3 ls modis-pds/MYD09GQ.006/`

## Receive Data Notifications with Amazon SNS

The following ARN is for an [Amazon SNS](https://aws.amazon.com/sns/) topic that provides notifications whenever a new MODIS scene has been added to the bucket on AWS:

`arn:aws:sns:us-west-2:552188055668:New_MODIS_Scene`

This topic publishes [an Amazon S3 event message](http://docs.aws.amazon.com/AmazonS3/latest/dev/notification-content-structure.html) whenever a scene-level index.html file has been created, which is the last step in the process to make scene data available on Amazon S3. It will only accept subscriptions via [Amazon SQS](https://aws.amazon.com/sqs/) or [AWS Lambda](https://aws.amazon.com/lambda/).

[Learn more about subscribing to SNS topics](http://docs.aws.amazon.com/sns/latest/dg/SubscribeTopic.html).

## Contact

If you have questions about the data, you can create an Issue at the project repo on [GitHub](https://github.com/AstroDigital/modis-ingestor/issues).

## License

There are no restrictions on the use of data, unless expressly identified prior to or at the time of receipt. More information on licensing and data citation is available from [USGS](http://eros.usgs.gov/about-us/data-citation).