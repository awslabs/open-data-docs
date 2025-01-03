# NAIP on AWS

The National Agriculture Imagery Program (NAIP) acquires aerial imagery during the agricultural growing seasons in the continental United States. This &ldquo;leaf-on&rdquo; imagery typically ranges from 30 centimeters to 100 centimeters in resolution and is available from the **naip-analytic** Amazon S3 bucket as 4-band (RGB + NIR) imagery in <a href="https://github.com/nasa-gibs/mrf/blob/master/src/gdal_mrf/frmts/mrf/MUG.md">MRF</a> format, in **naip-source** Amazon S3 bucket as 4-band (RGB + NIR) in uncompressed Raw GeoTiff format and **naip-visualization** as 3-band (RGB) Cloud Optimized GeotTiff format. NAIP data is provided state by state at varying time intervals. Each year, a variable number of states are updated with an overall update cycle of every two to three years for each state.

NAIP is administered through the USDA's Farm Production and Conservation Business Center (FPAC-BC) Geospatial Enterprise Operations (GEO) Branch.

NAIP imagery on AWS is available ranging from 2010 to 2023. This <a href="http://www.arcgis.com/home/webmap/viewer.html?webmap=17944d45bbef42afb05a5652d7c28aa5">NAIP coverage map</a> can help to determine what data is available for each year.

## Accessing NAIP on AWS

NAIP imagery on AWS, is managed by Esri and includes various products and formats. All NAIP imagery is provided in a Requester Pays bucket, which means that users can access it freely within the us-west-2 region, but will incur charges when accessing/downloading outside the region. More information on <a href="https://docs.aws.amazon.com/AmazonS3/latest/dev/RequesterPaysBuckets.html">Requeter Pays</a>.

The files are provided as digital ortho quarter quad tiles (DOQQs). Each individual tile covers a 3.75 x 3.75 minute quarter quadrangle plus a 300 meter buffer on all four sides. The DOQQs are provided as GeoTIFF, and each tile area corresponds to a USGS topographic quadrangle. All individual DOQQs are rectified in the UTM coordinate system, NAD 83. Some states, due to their size and location, span more than one UTM zone. Users should note that data volumes for individual states can be large. For the raw and analytic products, a single state for a single year is commonly comprised of multiple terabytes of imagery.

Data is grouped by state, year, resolution, data type and quadrangle.

For example, <code>al/2017/100cm/rgbir/30085/m_3008501_ne_16_1_20171018.mrf</code> is a MRF file and <code>al/2017/100cm/rgbir_cog/30085/m_3008501_ne_16_1_20171018.tif</code> is a COG file for a quarter-quad in the state of Alabama, taken during the 2017 season, at 100cm resolution, and is the northeastern corner of the 30085 USGS quadrangle.

The corresponding FGDC metadata file for the above MRF and COG is located under the <code>/fgdc/</code> prefix, <code>al/2017/100cm/fgdc/30085/m_3008501_ne_16_1_20171018.txt</code>.

Data stored under the prefix <code>/index/</code> are shapefiles that contain the file extents of each MRF and COG.

## Directory Structure

<b><u>Imagery MRF : </u></b><code>Bucketname/state/YYYY/resolution*/bands/SE_Reference_point_of_1_DegreeBlock/.mrf (.idx, .lrc)</code> files. (for **naip-source** and **naip-visualization** the file extension is .tif

<b><u>Imagery COG : </u></b><code>Bucketname/state/YYYY/resolution*/bands_cog/SE_Reference_point_of_1_DegreeBlock/.tif </code> files.

<b><u>Metadata: </u></b><code>Bucketname/state/YYYY/resolution/fgdc/SE_Reference_point_of_1_DegreeBlock/.txt</code> files

<b><u>Metadata for Year 2020 : </u></b><code>Bucketname/state/YYYY/resolution/fgdc/SE_Reference_point_of_1_DegreeBlock/quad/.xml</code> files

<b><u>State/Year Index: </u></b><code>Bucketname/state/YYYY/resolution/index/.shp (.dbf, .shx, .prj, .sbn)</code> files

\* Resolution will be defined in cm, either 100cm or 60cm or 50cm or 30cm

### Image Info

**naip-analytic**: The 4-band imagery in this bucket is provided in both MRF and COG formats. 

MRF is a cloud-optimized open data format that provides fast access to data on S3. MRF can be accessed using GDAL and most applications build using GDAL, but also directly by web applications. Data is provided as 512x512 tiles, with reduced-resolution pyramids created using 2x sampling by averaging. Finally, data compression is efficient and lossless (meaning the data values here are the same as the source). These files were created using gdal_translate with the following command:

<code>gdal_translate -of MRF -co OPTIONS="LERC_PREC=0.5 V2=ON -co COMPRESS=LERC src_dataset dst_dataset</code>

<code>gdaladdo -r average src_dataset 2 4 8 16 32 64</code>

*Note: The data upto year 2015 was compressed using Lerc1 compression and for the year 2016, 2017, 2018, 2019, 2020, 2021, 2022 and 2023 Lerc2 compression was used. Click <a href="https://github.com/Esri/lerc">here</a> for more info on Lerc compression.*

The Cloud Optimized GeoTIFF's (COG's) have been compressed using Deflate compression and are provided as 512x512 tiles with pyramids created using 2x sampling by averaging. These files were created using gdal_translate with the following command:

<code>gdal_translate -of COG -co tiled=yes -co BLOCKXSIZE=512 -co BLOCKYSIZE=512 -co COMPRESS=DEFLATE -co PREDICTOR=2 src_dataset dst_dataset</code>

<code>gdaladdo -r average -ro src_dataset 2 4 8 16 32 64</code>

*We used Esri’s <a href="https://github.com/Esri/OptimizeRasters/blob/master/README.md">OptimizedRasters</a> to convert this data.*

**naip-source**: The 4-band GeoTIFF's in this bucket are the source files provided by USDA, with no post-processing and without pyramids. Instead of the raw data, we recommend using the optimized MRF or COG data from the naip-analytic bucket.

**naip-visualization**: The 3-band COG's in this bucket have been compressed using YCbCr JPEG with quality 85 and are provided as 512x512 tiles, with pyramids created using 2x sampling by averaging. They were created using gdal_translate with the following command:

<code>gdal_translate -b 1 -b 2 -b 3 -of COG -co tiled=yes -co BLOCKXSIZE=512 -co BLOCKYSIZE=512 -co COMPRESS=DEFLATE -co PREDICTOR=2 src_dataset dst_dataset</code>

<code>gdaladdo -r average -ro src_dataset 2 4 8 16 32 64</code>

### Access Manifest

To see the full list of available files, you can access the bucket manifest with the aws-cli (version 15 and above) command below:

<code>aws s3 cp s3://naip-analytic/manifest.txt manifest.txt --request-payer</code>

<code>aws s3 cp s3://naip-source/manifest.txt manifest.txt --request-payer</code>

<code>aws s3 cp s3://naip-visualization/manifest.txt manifest.txt --request-payer</code>

or a command link

<code>aws s3 ls s3://naip-analytic/ --request-payer</code>

<em>The S3 buckets are provided as a Requester Pays bucket, see <a href="https://docs.aws.amazon.com/AmazonS3/latest/dev/RequesterPaysBuckets.html">here</a> for more information on Requester Pays.</em>