# NOAA National Water Model Forecast Model Data

The National Water Model (NWM) is a water resources model that simulates
and forecasts water budget variables, including snowpack,
evapotranspiration, soil moisture and streamflow, over the entire
continental United States (CONUS). The model, launched in August 2016,
is designed to improve the ability of NOAA to meet the needs of its
stakeholders (forecasters, emergency managers, reservoir operators,
first responders, recreationists, farmers, barge operators, and
ecosystem and floodplain managers) by providing expanded accuracy,
detail, and frequency of water information. It is operated by NOAA's
Office of Water Prediction.

Learn more about the National Water Model: [http://water.noaa.gov/](http://water.noaa.gov/)

## Accessing NWM Short Range Forecast on AWS


The NWM Short Range Forecast is stored in the **noaa-nwm-pds** Amazon S3
bucket in the **us-east-1** AWS region.

This bucket contains a four-week roll over of the Short Range Forecast
model output and the corresponding forcing data for version 1.2 of the
NWM model. The model is forced with meteorological data from the High
Resolution Rapid Refresh (HRRR) and the Rapid Refresh (RAP) models. The
Short Range Forecast configuration cycles hourly and produces hourly
deterministic forecasts of streamflow and hydrologic states out to 18
hours.

All data is in Network Common Data Form format (NetCDF) and correspond
to these general configurations:

-   1km gridded NetCDF (land surface variables and forcing)
-   250m gridded NetCDF (ponded water depth and depth to soil
    saturation)
-   Point-type NetCDF (stream routing and reservoir variables)
-   Point-type reservoir NetCDF (water surface elevation, inflow,
    outflow)

The data is organized by daily forecast, each forecast begins with the
prefix `nwm.yyyymmdd`, where: - `yyyy`: year - `mm` : month - `dd` : day
of the month

For example, all forecast data for 3 May 2018 have the prefix
`nwm.20180503`, you can use the AWS Command Line Interface to list this
forecast data. For example:

`aws s3 ls noaa-nwm-pds/nwm.20180503/ --no-sign-request`

For each daily forecast, the forcing data are separated from the short
range forecast data using the prefixes
`nwm.yyyymmdd/forcing_short_range/` and `wm.yyyymmdd/short_range/`.

Each data file uses the following naming convention:

`nwm.tHHz.shortrange.FILENAME.f0CYCLE.conus.nc`

Where: - `nwm` = national water model (this is the same for all files) -
`tHHZ` = hour when the forecast was made, for example t00z for midnight
- `shortrange` = shortrange forecast (fixed) - `FILENAME` = see list and
table below - `f0CYCLE` = the cycle of the forecast up to 18 hours, for
example 9th hour would be f009 - `conus` = continental United States
(this is the same for all files) - `nc` = NetCDF file format (this is
the same for all files)

For example, to retrieve the forecast made at midnight on a particular
date for reservoir water surface elevation at the 8th hour from
midnight, you would construct the following file name:

`nwm.t00z.short_range.reservoir.f008.conus.nc`

Which you could then attach to one of the following paths:

`s3://noaa-nwm-pds/nwm.yyyymmdd/short_range/` (replace yyyymmdd with the
date of interest)

or to a https url constructed like this:

`https://noaa-nwm-pds.s3.amazonaws.com/nwm.yyyymmdd/short_range/`
(replace yyyymmdd with the date of interest)

## Configuration of the Data

Each day is made up of hourly forecasts and each forecast has a time
horizon out to 18 hours. The data is made up of four forecast files and
a forcing file iterated over the forecast and horizon times. These files
are in NetCDF format and contain within them metadata describing their
content.

Listed here are some of the parameters and properties of the respective
files (by filename element). More details are available in the metadata
contained within each file:

**forcing: Geospatial, 1Km Gridded NetCDF**

-   LWDOWN: Surface downward long-wave radiation flux (W m<sup>-2</sup>)
-   PSFC: Surface pressure (Pa)
-   Q2D: 2m Specific Humidity (kg kg<sup>-1</sup>)
-   RAINRATE: Surface precipitation rate (mm sec<sup>-1</sup>)
-   SWDOWN: Surface downward short-wave radiation flux (W m<sup>-2</sup>)
-   T2D: 2m Air Temperature (K)
-   U2D: 10m U component of wind (m s<sup>-1</sup>)
-   V2D: 10m V component of wind (m s<sup>-1</sup>)

**land: Geospatial, 1Km Gridded NetCDF**

-   ACCET: Accumulated evapotranspiration (mm)
-   FSNO: Snow cover fraction on the ground (fraction)
-   SNEQV: Snow water equivalent (kg m<sup>-2</sup>)
-   SNOWH: Snow depth (m)
-   SNOWT\_AVG: Average snow temperature (degrees K)
-   SOILSAT\_TOP: Near surface soil moisture saturation (for top 40cm of
    soil column)

**terrain\_rt: Geospatial, 250m Gridded NetCDF**

-   sfcheadsubrt: Ponded water depth (mm)
-   zwattablrt: Water table depth (m)

**channel\_rt: Point type**

-   streamflow: Stream flow (m<sup>3</sup> s<sup>-1</sup>)
-   nudge: Stream flow data assimilation increment (m<sup>3</sup> s<sup>-1</sup>)
-   velocity: Stream velocity (m s<sup>-1</sup>)
-   qSfcLatRunof: Runoff from terrain routing into channel (m<sup>3</sup> s<sup>-1</sup>)
-   qBtmVertRunoff: Runoff from bottom of soil column to groundwater
    bucket (m<sup>-3</sup>)
-   qBucket: Flux from groundwater bucket into channel (m<sup>3</sup> s<sup>-1</sup>)

**reservoir: Point Type**

-   elevation: Water surface elevation (m)
-   inflow: Reservoir inflow (m<sup>3</sup> s<sup>-1</sup>)
-   outflow: Reservoir outflow (m<sup>3</sup> s<sup>-1</sup>)

Contained in the table below is a summary of the files and examples of
path and filenames.

  File Name     Description                                          Cycle   Example File Name
  ------------- ---------------------------------------------------- ------- ---------------------------------------------------------------
  forcing       Forcing paramters for the NWM model                  1 Hr    /short\_range/nwm.t00z.short\_range.forcing.f001.conus.nc
  land          Snow cover, Near Surface Soil Moisture               1 Hr    /short\_range/nwm.t00z.short\_range.land.f001.conus.nc
  terrain\_rt   Ponded Water and Water Table Depth                   1 Hr    /short\_range/nwm.t00z.short\_range.terrain\_rt.f001.conus.nc
  channel\_rt   Stream Flow and Stream Velocity                      1 Hr    /short\_range/nwm.t00z.short\_range.channel\_rt.f001.conus.nc
  reservoir     Water Surface Elevation, Reservoir in and out flow   1 Hr    /short\_range/nwm.t00z.short\_range.reservoir.f001.conus.nc

## Document Updates

27th July 2018. Corrected the units of the following paramters: - ACCET
- from m to mm - qBucket - from m\^3\^ to m<sup>3</sup> s<sup>-1</sup> - qSfcLatRunoff -
from m\^3\^ to m<sup>3</sup> s<sup>-1</sup>

Please note that the metadata in the corresponding netCDF files may have
incorrect units for: - qBucket and qSfcLatRunoff.

The units listed above are the correct units.
