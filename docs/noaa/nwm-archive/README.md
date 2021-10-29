# NOAA National Water Model Reanalysis Model Data

In the field of Earth System Modeling, a reanalysis is a model
simulation that joins modern modeling technologies and newer, more
complete, datasets to obtain a better understanding of past
environments. For example, in weather forecasting, it can be thought of
as running a weather forecast model in the past. In such a scenario it
could mean the running of a weather model for a period when no data
existed in order to get a better understanding of the state of the
weather from that period. This same type of historical run can be
conducted for other types of models as well, including water models.

The NOAA National Water Model Reanalysis dataset contains input and output from multi-decade retrospective simulations. These simulations used observed rainfall as input and ingested other required meteorological input fields from a weather reanalysis dataset. The output frequency and fields available in this historical NWM dataset differ from those contained in the real-time forecast model. One application of this dataset is to provide historical context to current near real-time streamflow, soil moisture and snowpack conditions. The reanalysis data can be used to infer flow frequencies and perform temporal analyses with hourly streamflow output and 3-hourly land surface output. This  dataset can also be used in the development of end user applications which require a long baseline of data for system training or verification purposes. This dataset contains output from two retrospective simulations. Currently there are three versions of the dataset. Version 1.2 output files are available in netcdf (all variables). Version 2.0 output files are available in both netcdf (all variables) and zarr (channel routing variables) formats. While, the third version 2.1 input and output files are available in both netcdf and zarr for the following data types:

channel routing (chrtout)

subsurface (gwout)

lake (lakeout)

land surface (ldasout)

precipitation (precip) and 

terrain routing (rtout)

For the current three versions of the dataset we have: A 25-year (January 1993 through December 2017) retrospective simulation using version 1.2 of the National Water Model, a 26-year (January 1993 through December 2018) retrospective simulation using version 2.0 of the National Water Model and a 41-year (February 1979 through December 2020) retrospective simulation using version 2.1 of the National Water Model.


Learn more about the National Water Model: [http://water.noaa.gov/](http://water.noaa.gov/)

## Accessing NWM Reanalysis on AWS


The NWM Reanalysis version 1.2 is stored in the **nwm-archive** Amazon S3
bucket while the version 2.0 is stored int he **noaa-nwm-retro-v2.0-pds** bucket both in the **us-east-1** AWS region.

This bucket contains the reanalysis archive organized by year starting
in 1993. The files are an internally compressed NetCDF format. They do
not need to be decompressed. Each file contains detailed metadata
describing the data stored within it.

You can use the AWS Command Line Interface to list a particular year
(2003 in this example) in the bucket like this:

`aws s3 ls s3://noaa-nwm-retro-v2.0-pds/full_physics/2003/ --no-sign-request`

`aws s3 ls nwm-archive/2003/ --no-sign-request`


or via a url constructed like this:
`https://noaa-nwm-retro-v2.0-pds.s3.amazonaws.com/?prefix=full_physics/2003`

`https://nwm-archive.s3.amazonaws.com/?prefix=2003`

Each year contains files for each hour of the year iterated over
different product types, not all products are published at hourly
intervals. The products types are listed in the table below.

  Product   Description                                                  Cycle            Example File Name
  --------- ------------------------------------------------------------ ---------------- ------------------------------------
  CHRTOUT   Streamflow values at points associated with flow lines       Every Hour       201701010000.CHRTOUT\_DOMAIN1.comp
  LAKEOUT   Output values at points associated with reservoirs (lakes)   Every Hour       201701010000.LAKEOUT\_DOMAIN1.comp
  LDASOUT   Land surface model output                                    Every 3rd Hour   201701010000.LDASOUT\_DOMAIN1.comp
  RTOUT     Ponded water and depth to soil saturation                    Every 3rd Hour   201701010000.RTOUT\_DOMAIN1.comp

Each product (data file) uses the following naming convention:
`yyyy/yyyymmddhhhh.product_DOMAIN1.comp` where:

-   `yyyy` year from 1993
-   `mm` month
-   `dd` day
-   `hhhh` hour of day
-   `product` (CHRTOUT, LAKEOUT, LDASOUT or RTOUT)
-   `DOMAIN1.comp` this is the same for all files

For example, to access the stream flow data for 12pm on May 1st 2003,
the key would be:

`2003/200305011200.CHRTOUT_DOMAIN1.comp`

To access the data, combine the key with the bucket name like this:

`s3://nwm-archive/2003/200305011200.CHRTOUT_DOMAIN1.comp`

or via HTTPS like this:

`https://nwm-archive.s3.amazonaws.com/2003/200305011200.CHRTOUT_DOMAIN1.com`

## Configuration of the Data

The dataset contains four products for each day. The four products are
listed below along with some of the parameters and properties of the
respective files. More details are available in the metadata contained
within each file:

**RTOUT: Geospatial, 250m Gridded NetCDF**

-   sfcheadsubrt: Ponded water depth (mm)
-   zwattablrt: Water table depth (m)

**LAKEOUT: Point Type (including Lake ID)**

-   feature\_id: Lake Common ID
-   elevation: Water surface elevation (m)
-   inflow: Reservoir inflow (m<sup>3</sup> s<sup>-1</sup>)
-   outflow: Reservoir outflow (m<sup>3</sup> s<sup>-1</sup>)

**CHRTOUT: Point Type (including Reach ID)**

-   feature\_id: Reach ID
-   streamflow: River Flow (m<sup>3</sup> s<sup>-1</sup>)
-   q\_lateral: Runoff into channel reach (m<sup>3</sup> s<sup>-1</sup>)
-   velocity: River Velocity (m s<sup>-1</sup>)
-   qSfcLatRunoff: Runoff from terrain routing (m<sup>3</sup> s<sup>-1</sup>)
-   qBucket: Flux from ground water bucket (m<sup>3</sup> s<sup>-1</sup>)
-   qBtmVertRunoff: Runoff from bottom of soil to ground water bucket
    (m<sup>3</sup>)

**LDASOUT: Geospatial, 1Km Gridded NetCDF**

-   ACCET: Accumulated evapotranspiration (mm)
-   FIRA: Total net long wave (LW) radiation to atmosphere (W m<sup>-2</sup>)
-   FSA: Total absorbed Short Wave (SW) radiation (W m<sup>-2</sup>)
-   FSNO: Snow cover fraction on the ground (fraction)
-   HFX: Total sensible heat to the atmosphere (W m<sup>-2</sup>)
-   LH: Latent heat to the atmosphere (W m<sup>-2</sup>)
-   SNEQV: Snow water equivalent (kg m<sup>-2</sup>)
-   SNOWH: Snow depth (m)
-   SOIL\_M: Volumetric soil moisture (m<sup>3</sup> m<sup>-3</sup>)
-   SOIL\_W: Liquid volumetric soil moisture (m<sup>3</sup> m<sup>-3</sup>)
-   TRAD: Surface radiative temperature (K)
-   UGDRNOFF: Accumulated underground runoff (mm)

Note: For the two accumulation variables, UGDRNOFF and ACCET, the
accumulation takes place between pairs of dates: 1. 00Z January 1 -- 21Z
March 31 2. 00Z April 1 -- 21Z June 30 3. 00Z July 1 -- 21Z September 30
4. 00Z October 1 -- 21Z December 31

## Document Updates

27th July 2018. Corrected the units of the following paramters: - ACCET
- from m to mm - qBucket - from m\^3\^ to m<sup>3</sup> s<sup>-1</sup> - qSfcLatRunoff -
from m\^3\^ to m<sup>3</sup> s<sup>-1</sup>

Please note that the metadata in the corresponding netCDF files may have
incorrect units for: - qBucket and qSfcLatRunoff.

The units listed above are the correct units.
