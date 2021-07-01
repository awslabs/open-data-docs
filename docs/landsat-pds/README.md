# Landsat on AWS

Landsat 8 data is available for anyone to use via Amazon S3. All Landsat 8 scenes are available from the start of imagery capture. All new Landsat 8 scenes are made available each day, often within hours of production.

The Landsat program is a joint effort of the <a href="https://usgs.gov">U.S. Geological Survey</a> and <a href="https://nasa.gov">NASA</a>. First launched in 1972, the Landsat series of satellites has produced the longest, continuous record of Earth’s land surface as seen from space. NASA is in charge of developing remote-sensing instruments and spacecraft, launching the satellites, and validating their performance. USGS develops the associated ground systems, then takes ownership and operates the satellites, as well as managing data reception, archiving, and distribution. Since late 2008, Landsat data have been made available to all users free of charge. Carefully calibrated Landsat imagery provides the U.S. and the world with a long-term, consistent inventory of vitally important global resources.

AWS has made Landsat 8 data freely available on Amazon S3 so that anyone can use our on-demand computing resources to perform analysis and create new products without needing to worry about the cost of storing Landsat data or the time required to download it.

Learn more about how Landsat data is used on <a href="http://landsat.gsfc.nasa.gov/">NASA's Landsat Science site</a>.

## Accessing Landsat on AWS
The data are organized using a directory structure based on each scene’s path and row. For instance, the files for Landsat scene LC08_L1TP_139045_20170304_20170316_01_T1 are available in the following location: s3://landsat-pds/c1/L8/139/045/LC08_L1TP_139045_20170304_20170316_01_T1/

The “c1” refers to Collection 1, the “L8” refers to Landsat 8, “139” refers to the scene’s path, “045” refers to the scene’s row, and the final directory matches the product’s identifier, which uses the following naming convention: `LXSS_LLLL_PPPRRR_YYYYMMDD_yyymmdd_CC_TX`, in which:

- `L` = Landsat
- `X` = Sensor
- `SS` = Satellite
- `PPP` = WRS path
- `RRR` = WRS row
- `YYYYMMDD` = Acquisition date
- `yyyymmdd` = Processing date
- `CC` = Collection number
- `TX` = Collection category

In this case, the scene corresponds to WRS path 139, WRS row 045, and was taken on March 4th, 2017.

Each scene’s directory includes:

- a .TIF GeoTIFF for each of the scene’s up to 12 bands (note that the GeoTIFFs include 512x512 internal tiling)
- .TIF.ovr overview file for each .TIF (useful in GDAL based applications)
- a _MTL.txt metadata file
- a small rgb preview jpeg, 3 percent of the original size
- a larger rgb preview jpeg, 15 percent of the original size
- an index.html file that can be viewed in a browser to see the RGB preview and links to the GeoTIFFs and metadata files

For instance, the files associated with scene `LC08_L1TP_139045_20170304_20170316_01_T1` are available at:

`s3://landsat-pds/c1/L8/139/045/LC08_L1TP_139045_20170304_20170316_01_T1/`

or

`https://landsat-pds.s3.amazonaws.com/c1/L8/139/045/LC08_L1TP_139045_20170304_20170316_01_T1/index.html`

A gzipped csv describing all available scenes is available at

`s3://landsat-pds/scene_list.gz`

or

`https://landsat-pds.s3.amazonaws.com/c1/L8/scene_list.gz`

If you use the AWS Command Line Interface, you can access the bucket with this simple command:

`aws s3 ls landsat-pds/c1/`

<em>Note that Collection 1 Real-Time data will be stored on a rolling, 1 month basis to be backfilled as Tier 1 data becomes available.</em>

## Receive Data Notifications with Amazon SNS
The following ARN is for an <a href="https://aws.amazon.com/sns/">Amazon SNS</a> topic that provides notifications whenever a new Landsat scene has been added to Landsat on AWS::

`arn:aws:sns:us-west-2:274514004127:NewSceneHTML`

This topic publishes <a href="http://docs.aws.amazon.com/AmazonS3/latest/dev/notification-content-structure.html">an Amazon S3 event message</a> whenever a scene-level index.html file has been created, which is the last step in the process to make scene data available on Amazon S3. It will only accept subscriptions via <a href="https://aws.amazon.com/sqs/">Amazon SQS</a> or <a href="https://aws.amazon.com/lambda/">AWS Lambda</a>.

<a href="http://docs.aws.amazon.com/sns/latest/dg/SubscribeTopic.html">Learn more about subscribing to SNS topics</a>.

