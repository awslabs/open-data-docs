# ASTER-L1T on AWS

ASTER L1T data available for anyone to use via Amazon S3. All new ASTER L1T scenes are made available each day.


The Advanced Spaceborne Thermal Emission and Reflection Radiometer (ASTER) Level 1 Precision Terrain Corrected Registered At-Sensor Radiance (AST_L1T) data contains calibrated at-sensor radiance, which corresponds with the ASTER Level 1B (AST_L1B), that has been geometrically corrected, and rotated to a north-up UTM projection. The AST_L1T is created from a single resampling of the corresponding ASTER L1A (AST_L1A) product.

The precision terrain correction process incorporates GLS2000 digital elevation data with derived ground control points (GCPs) to achieve topographic accuracy for all daytime scenes where correlation statistics reach a minimum threshold. Alternate levels of correction are possible (systematic terrain, systematic, or precision) for scenes acquired at night or that otherwise represent a reduced quality ground image (e.g., cloud cover).

ASTER-L1T v003 data has been organized as Cloud-Optimized GeoTIFFs (COGs) by sensor and underlying projection system to facilitate ease of analysis.  AWS has made ASTER L1T data freely available on Amazon S3 so that anyone can use our on-demand computing resources to perform analysis and create new products without needing to worry about the cost of storing ASTER L1T data or the time required to download it.

Learn more about ASTER L1T data on <a href="https://lpdaac.usgs.gov/products/ast_l1tv003/">NASA's LPDAAC at USGS EROS Center</a> website. ASTER SWIR detectors are not functioning normally and SWIR data acquired since Apr 2008 are not useable. Please consult the <a href="https://lpdaac.usgs.gov/documents/217/ASTER_SWIR_User_Advisory_July_18_08.pdf">ASTER SWIR User Advisory</a> for more details.

## Accessing ASTER-L1T data on AWS
The data are organized using a directory structure based on each scene’s projection, path and row. The location of the imagery object associated with a collection from a single sensor follows the following template.

`s3://aster-l1t/{sensor}/{epsg}/{path:03d}/{row:03d}/{yyyymmdd}/{granule}_{sensor}.tif`

The associated metadata in STAC format can be found adjacent to the imagery

`s3://aster-l1t/{sensor}/{epsg}/{path:03d}/{row:03d}/{yyyymmdd}/{granule}_{sensor}_stac.json`


The images are organized by sensors as not all ASTER granules contain imagery from all the sensors. VNIR includes Bands 1 to 3, SWIR Bands 4 to 9 and TIR Bands 10 to 14.


The various fields that contribute to the object path are tabulated below:

| Field       |        Description                           | Example      															   |
|:------------|:---------------------------------------------|:----------------------------------------------|
| sensor      | One of VNIR, TIR or SWI                      |  SWIR        	 															 |
| epsg        | EPSG code of the source ASTER L1T granule    |  32611        																 |
| path        | Scene Path Number in the WRS system          |  137          																 |
| row         | Scene row number in the WRS system           |  568          																 |
| yyyymmdd    | Scene acquisition date                       | 20080101      																 |
| granule     | NASA CMR source granule name                 | AST_L1T_00301012008054749_20170730074450_36729|


For the example above, the two associated objects are:

`s3://aster-l1t/SWIR/32611/137/568/20080101/AST_L1T_00301012008054749_20170730074450_36729_SWIR.tif`

`s3://aster-l1t/SWIR/32611/137/568/20080101/AST_L1T1_00301012008054749_20170730074450_36729_SWIR_stac.json`


In this case, the scene corresponds to imagery acquired by SWIR sensor over WRS path 137, WRS row 158 on January 1st, 2008.

Each scene’s assets includes:

- a multi-band `.tif` Cloud Optimized GeoTIFF file - 3 bands for VNIR, 6 bands for SWIR and 5 bands for TIR.
- a `_stac.json` metadata file


## Receive Data Notifications with Amazon SNS
The following ARN is for an <a href="https://aws.amazon.com/sns/">Amazon SNS</a> topic that provides notifications whenever a new ASTER L1T scene has been added to ASTER-L1T on AWS::

`arn:aws:sns:us-west-2:526859492376:aster-l1t-object_created`

This topic publishes <a href="http://docs.aws.amazon.com/AmazonS3/latest/dev/notification-content-structure.html">an Amazon S3 event message</a> whenever a new `.tif` file has been created. It will only accept subscriptions via <a href="https://aws.amazon.com/sqs/">Amazon SQS</a> or <a href="https://aws.amazon.com/lambda/">AWS Lambda</a>.

<a href="http://docs.aws.amazon.com/sns/latest/dg/SubscribeTopic.html">Learn more about subscribing to SNS topics</a>.


## Detailed metadata associated with the scenes

We include a `_stac.json` metadata file with every `.tif` file. As this is an evolving metadata format, this may not sufficiently capture all the information needed by users for scientific analysis. Hence, we also also included the following metadata items that can be accessed via a TIFF reader or via GDAL-based applications.

1. `GRANULE_METADATA`
   - JSON string that contains the serialized metadata of the source ASTER L1T granule
2. `PATH`, `ROW`, `VIEW`, `CYCLE`, `REVOLUTION`, `ORBIT`, `CLOUD_PERCENT`
   - Scalars set at the image level that are useful for manipulating collections.
3. `SATURATION_DN`, `RADIANCE_GAIN` and `RADIANCE_BIAS`
   - These are scalars set at the band level for each band for all the sensors. The gain and bias factors allow users to translate the imagery in DN to top-of-atmosphere radiance.
4. `REFLECTANCE_GAIN` and `REFLECTANCE_BIAS`
   - These are scalars set at the band level for each band of the VNIR and SWIR imagery. The gain and bias factors allow users to translate the imagery in DN to top-of-atmosphere reflectance.
5. `BRIGHTNESS_TEMPERATURE_k1` and `BRIGHTNESS_TEMPERATURE_k2`
   - These are scalars set at the band level for each band of TIR imagery. These factors allow conversion of top-of-atmosphere radiances to brightness temperatures.

