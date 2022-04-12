# The Global Ensemble Forecast System (GEFS) from NOAA

The Global Ensemble Forecast System (GEFS), previously known as the GFS Global ENSemble (GENS), is a weather forecast model made up of 21 separate forecasts, or ensemble members. The National Centers for Environmental Prediction (NCEP) started the GEFS to address the nature of uncertainty in weather observations, which are used to initialize weather forecast models. For more information, see [here](https://www.ncei.noaa.gov/products/weather-climate-models/global-ensemble-forecast).

The files are in [GRIB2](http://www.nco.ncep.noaa.gov/pmb/docs/grib2/grib2_doc/) format. Both NOAA and NASA provide tools for accessing GRIB2 files:

 - [Panoply](https://www.giss.nasa.gov/tools/panoply/) is a NASA application that allows users to view and graph data that are contained in GRIB2 files.
 - [wgrib2](http://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/) is a software library from NOAA that can be used for extracting data from a GRIB2 file. It is aimed at advanced users.
 - GRIB2 files can also be read by [GDAL](https://gdal.org/drivers/raster/grib.html), which can be accessed through many interfaces such as R, Python.


## Accessing Global Ensemble Forecast System (GEFS) Model Data on AWS
The GEFS Forecast data is stored in an Amazon S3 bucket in us-east-1 AWS region.

 - `noaa-gefs-pds`

This bucket contains the ensemble forecast data organized by day.
You can use the AWS Command Line Interface to list a particular day in the bucket like this:
 - aws s3 ls `noaa-gefs-pds/gefs.YYYYMMDD/XX/pgrb2a/`
or
 - aws s3 ls `noaa-gefs-pds/gefs.YYYYMMDD/XX/pgrb2b/`

Where `XX`  will be (00, 06, 12, 18) corresponding to the four forecasts published each day, every six hours and `YYYY` = year ,`MM` = month, `DD` = day

The organisation of the contents of `/pgrb2a/` and `/pgrb2b/` are the same and the contents are structured as follows (substitute pgrb2b for pgrb2a when appropriate):

The control run of the 21-member ensemble has output filenames that start  `gec00`:
 - `gec00.tXXz.pgrb2aanl`
 - `gec00.tXXz.pgrb2afVV`

Where `VV` will be in (00,06, .. , 384). `XX` will, as above,   be whatever you put in the path after `gefs.YYYYMMDD/` , (00, 06, 12, 18).

If you append the control run name onto the path you will get:
 - `noaa-gefs-pds/gefs.YYYYMMDD/XX/pgrb2b/gec00.tXXz.pgrb2aanl`
 - `noaa-gefs-pds/gefs.YYYYMMDD/XX/pgrb2b/gec00.tXXz. pgrb2afVV`

The forecast data from the 20 other ensemble perturbations have filenames that start  with `gep`:
 - `gepWW.tXXz.pgrb2aanl`
 - `gepWW.tXXz.pgrb2afVV`

Where `WW` will be in (01,02,03, .. 20) and VV will be in (00,06,12,18, .. ,384). `XX` will  be whatever you put in the path after `gefs.YYYYMMDD/` , (00, 06, 12, 18).

So if you append the perturbation run name for the data file onto the path you will get:
 - `noaa-gefs-pds/gefs.YYYYMMDD/XX/pgrb2b/gepWW.tXXz.pgrb2aanl`
 - `noaa-gefs-pds/gefs.YYYYMMDD/XX/pgrb2b/gepWW.tXXz.pgrb2afVV`

## Contents of the GRIB2 files.

Links to descriptions of the parameters that are contained in the GRIB2 files (pgrb2a & pgrb2b) follows:

[http://www.nco.ncep.noaa.gov/pmb/products/gens/](http://www.nco.ncep.noaa.gov/pmb/products/gens/)

The “pgrb2a” files contain ~83 of the “most commonly-used parameters”as described here:
[http://www.nco.ncep.noaa.gov/pmb/products/gens/gec00.t12z.pgrb2af06.shtml](http://www.nco.ncep.noaa.gov/pmb/products/gens/gec00.t12z.pgrb2af06.shtml)

The “pgrb2b” files contain ~425 of the “least commonly-used parameters”as described here:
[http://www.nco.ncep.noaa.gov/pmb/products/gens/gec00.t12z.pgrb2bf06.shtml](http://www.nco.ncep.noaa.gov/pmb/products/gens/gec00.t12z.pgrb2bf06.shtml)


