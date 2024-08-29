# The Operational Forecast System (OFS) from NOAA

As of September 16, 2024, the names of the model output files will officially be transitioned to the following format:
**Stations:** OFS.tCCz.YYYYMMDD.stations.[nowcast|forecast].nc
**2-D surface field output:** OFS.tCCz.YYYYMMDD.2ds.[n|f]HHH.nc
**3-D field output:** OFS.tCCz.YYYYMMDD.fields.[n|f]HHH.nc
**3-D field output on a regular grid:** OFS.tCCz.YYYYMMDD.regulargrid.[n|f]HHH.nc

Where:
**OFS** refers to the name of the model (e.g. cbofs, sfbofs, leofs)
**[nowcast/forecast]** or **[n/f]** denotes either the nowcast or forecast results
**YYYYMMDD** refers to the date of the model run
**CC** refers to the cycle of the day (e.g. 06, 12)
**HHH** is the nowcast or forecast hour (e.g. 001, 002)

An Operational Forecast System (OFS) consists of the automated integration of observing system data streams, hydrodynamic model predictions, product dissemination and continuous quality-control monitoring. State-of-the-art numerical hydrodynamic models driven by real-time data and meteorological, oceanographic, and/or river flow rate forecasts form the core of these end-to-end systems. The OFS perform nowcast and short-term (0 hr. - 120 hr.) forecast predictions of pertinent parameters (e.g., water levels, currents, salinity, temperature, waves) and disseminate them to users.


 Nowcasts and forecasts are scientific predictions about the present and future states of water levels (and possibly currents and other relevant oceanographic variables, such as salinity and temperature) in a coastal area. They rely on observed data and/or forecasts from a numerical model. A nowcast incorporates recent (and often near real-time) observed meteorological, oceanographic, and/or river flow rate data. A nowcast covers the period of time from the recent past (e.g., the past day) to the present, and it can make predictions for locations where observational data are not available. A forecast incorporates meteorological, oceanographic, and/or river flow rate forecasts and makes predictions for times where observational data will not be available. A forecast is usually initiated by the results of a nowcast.



## File Format



The files are in [NETCDF](https://www.esrl.noaa.gov/psd/data/gridded/whatsnetCDF.html) format. Both NOAA and NASA provide tools for accessing NETCDF files:



 - [Panoply](https://www.giss.nasa.gov/tools/panoply/) is an application from NASA that allows users to view and graph data that are contained in NETCDF files.

 - [ncdump](https://www.esrl.noaa.gov/psd/data/gridded/read-our-files.htmlhttp://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/) is a software library that can be used for extracting data from a NETCDF file. It is aimed at advanced users.

 - [Other tools](https://www.esrl.noaa.gov/psd/data/gridded/read-our-files.html) are also available here.





## Accessing Operational Forecast System Data on AWS

The OFS Forecast are stored in Amazon S3 buckets in us-east-1 AWS region.



 - The `noaa-ofs-pds` bucket contains NOMADS production OFS models for the last 30 days. 
 - The `noaa-nos-ofs-pds` bucket contains CO-OPS operational models and are retained historically. 



The CO-OPS operational bucket contains the ensemble forecast data organized by day.

You can use the AWS Command Line Interface to list a particular day in the bucket like this:

 - aws s3 ls --no-sign-request s3://noaa-nos-ofs-pds/OFS/netcdf/YYYYMM    
 - aws s3 ls --no-sign-request s3://noaa-ofs-pds/OFS.YYYYMMDD

 - Where `OFS` stands for Operational Forecast System, see table below

 - `YYYY` = year, `MM` = month, `DD` = day

<br />
<table>
<thead>
<tr>
<th>OFS Code</th>
<th>Operational Forecast System</th>
<th>Forecast Horizon</th>
<th>Resolution</th>
</tr>
</thead>
<tbody>
<tr>
<td>CBOFS</td>
<td>Chesapeake Bay</td>
<td>48 hrs</td>
<td>50m - 3km</td>
</tr>
<tr>
<td>CIOFS</td>
<td>Cook Inlet</td>
<td>48 hrs</td>
<td>10m – 3.5km</td>
</tr>
<tr>
<td>DBOFS</td>
<td>Delaware Bay</td>
<td>48 hrs</td>
<td>100m - 3km</td>
</tr>
<tr>
<td>GLOFS</td>
<td>Great Lakes</td>
<td>60 hrs</td>
<td>5 km</td>
</tr>
<tr>
<td>GoMOFS</td>
<td>Gulf of Maine</td>
<td>72 hrs</td>
<td>700 m</td>
</tr>
<tr>
<td>LEOFS</td>
<td>Lake Erie</td>
<td>120 hrs</td>
<td>400m - 4km</td>
</tr>
<tr>
<td>LMHOFS</td>
<td>Lake Michigan and Huron</td>
<td>120 hrs</td>
<td>50m – 2.5km</td>
</tr>
<tr>
<td>NGOFS2</td>
<td>Northern Gulf of Mexico</td>
<td>48 hrs</td>
<td>45m - 300m</td>
</tr>
<tr>
<td>SFBOFS</td>
<td>San Francisco Bay</td>
<td>48 hrs</td>
<td>100m - 4km</td>
</tr>
<tr>
<td>SSCOFS</td>
<td>Salish Sea and Columbia River</td>
<td>72 hrs</td>
<td>100m - 3km</td>
</tr>
<tr>
<td>TBOFS</td>
<td>Tampa Bay</td>
<td>48 hrs</td>
<td>100m - 1.2km</td>
</tr>
<tr>
<td>WCOFS</td>
<td>West Coast</td>
<td>72 hrs</td>
<td>4km</td>
</tr>
</tbody>
</table>
<br />


Construction of the path for particular OFS data is outlined below and will allow access to the forecast data. To construct the path and filename substitute the following variables into the path templates below:



 - `OFS` is the `OFS Code` from previous table

 - `CC` is the model run cycles, 00, 06, 12, 18 , or 03, 09, 15, 21 for nowcast and forecast runs

 - `YYYY` = year, `MM` = month, `DD` = day

 - `HHH` is forecast hour, i.e. 001 – 120, check the forecast horizon in previous table


As of September 16, 2024, the OFS will be using a new file naming convention.  See the [NWS Service Change Notice](https://www.weather.gov/media/notification/pdf_2023_24/scn24-77_nos_sscofs.pdf) for more details on the new file names. The file names shown below use the old naming convention and can be used to access files older than September 16, 2024.

<br />
<table>
<thead>
<tr>
<th>File Names</th>
<th>Format</th>
<th>File Type</th>
</tr>
</thead>
<tbody>
<tr>
<td>nos.OFS.stations.nowcast.YYYYMMDD.tCCz.nc </td>
<td>Station nowcast file (NetCDF)</td>
<td>output</td>
</tr>
<tr>
<td>nos.OFS.stations.forecast.YYYYMMDD.tCCz.nc</td>
<td>Station forecast file (netCDF)</td>
<td>output</td>
</tr>
<tr>
<td>nos.OFS.fields.nHHH.YYYYMMDD.tCCz.nc</td>
<td>Field nowcast file (NetCDF)</td>
<td>output</td>
</tr>
<tr>
<td>nos.OFS.fields.fHHH.YYYYMMDD.tCCz.nc</td>
<td>Field forecast file (NetCDF)</td>
<td>output</td>
</tr>
<tr>
<td>nos.OFS.2ds.nHHH.YYYYMMDD.tCCz.nc</td>
<td>Surface layer 2D Field nowcast file (NetCDF)</td>
<td>output</td>
</tr>
<tr>
<td>nos.OFS.2ds.fHHH.YYYYMMDD.tCCz.nc</td>
<td>Surface layer 2D Field forecast file (NetCDF)</td>
<td>output</td>
</tr>
<tr>
<td>nos.OFS.met.nowcast.YYYYMMDD.tCCz.nc</td>
<td>Surface forcing nowcast file (NetCDF)</td>
<td>input</td>
</tr>
<tr>
<td>nos.OFS.hflux.nowcast.YYYYMMDD.tCCz.nc</td>
<td>Surface flux forcing nowcast file (NetCDF)</td>
<td>input</td>
</tr>
<tr>
<td>nos.OFS.met.forecast.YYYYMMDD.tCCz.nc</td>
<td>Surface forcing forecast file (NetCDF)</td>
<td>input</td>
</tr>
<tr>
<td>nos.OFS.hflux.forecast.YYYYMMDD.tCCz.nc</td>
<td>Surface flux forcing forecast file (NetCDF)</td>
<td>input</td>
</tr>
<tr>
<td>nos.OFS.obc.YYYYMMDD.tCCz.nc</td>
<td>Open boundary forcing file (NetCDF)</td>
<td>input</td>
</tr>
<tr>
<td>nos.OFS.clim.YYYYMMDD.tCCz.nc</td>
<td>Climate nudging file from global RTOFS</td>
<td>input</td>
</tr>
<tr>
<td>nos.OFS.river.YYYYMMDD.tCCz.nc</td>
<td>River forcing file (NetCF)</td>
<td>input</td>
</tr>
<tr>
<td>nos.OFS.init.nowcast.YYYYMMDD.tCCz.nc</td>
<td>Initial condition file (NetCDF)</td>
<td>input</td>
</tr>
<tr>
<td>nos.OFS.roms.tides.YYYYMMDD.tCCz.nc</td>
<td>Tidal forcing file (NetCDF)</td>
<td>input</td>
</tr>
<tr>
<td>nos.OFS.nowcast.YYYYMMDD.tCCz.in</td>
<td>Runtime input file for nowcast (text)</td>
<td>input</td>
</tr>
<tr>
<td>nos.OFS.forecast.YYYYMMDD.tCCz.in</td>
<td>Runtime input file for forecast (text)</td>
<td>input</td>
</tr>
<tr>
<td>nos.OFS.fields.nowcast.YYYYMMDD.tCCz.nc</td>
<td>3D field nowcast file (NetCDF)</td>
<td>output</td>
</tr>
<tr>
<td>nos.OFS.fields.forecast.YYYYMMDD.tCCz.nc</td>
<td>3D field forecast file (NetCDF)</td>
<td>output</td>
</tr>
</tbody>
</table>
<br />



For `noaa-ofs-pds` if you append the model run name onto the path you will get:

 - `noaa-ofs-pds/OFS.YYYYMMDD/nos.OFS.fields.fHHH.YYYYMMDD.tCCz.nc`

 - `noaa-ofs-pds/OFS.YYYYMMDD/nos.OFS.fields.nHHH.YYYYMMDD.tCCz.nc`

 - `noaa-ofs-pds/OFS.YYYYMMDD/nos.OFS.forecast.YYYYMMDD.tCCz.nc`

 -  etc.

For `noaa-nos-ofs-pds` if you append the model run name onto the path you will get:


 - noaa-nos-ofs-pds/OFS/netcdf/YYYYMM/nos.OFS.fields.fHHH.YYYYMMDD.tCCz.nc

 - noaa-nos-ofs-pds/OFS/netcdf/YYYYMM/nos.OFS.fields.nHHH.YYYYMMDD.tCCz.nc

 - noaa-nos-ofs-pds/OFS/netcdf/YYYYMM/nos.OFS.forecast.YYYYMMDD.tCCz.nc

 - etc.



## Contents of the NETCDF files.



Learn more about the Operational Forecast System models via the following link:



[https://www.tidesandcurrents.noaa.gov/models.html](https://www.tidesandcurrents.noaa.gov/models.html)



