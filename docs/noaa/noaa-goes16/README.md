# NOAA GOES on AWS

Data from NOAA's GOES-R series satellite is available on Amazon S3. The National Oceanic and Atmospheric Administration (NOAA) operates a constellation of Geostationary Operational Environmental Satellites (GOES) to provide continuous weather imagery and monitoring of meteorological and space environment data for the protection of life and property across the United States. GOES satellites provide critical atmospheric, oceanic, climatic and space weather products supporting weather forecasting and warnings, climatologic analysis and prediction, ecosystems management, safe and efficient public and private transportation, and other national priorities.

The satellites provide advanced imaging with increased spatial resolution, 16 spectral channels, and up to 1 minute scan frequency for more accurate forecasts and timely warnings.

The real-time feed and full historical archive of original resolution Advanced Baseline Imager (ABI) radiance data (Level 1b) and full resolution Cloud and Moisture Imager (CMI) products (Level 2) are freely available on Amazon S3 for anyone to use.

## Accessing GOES Data on AWS

The availability of GOES-R Series on AWS data is the result of the NOAA Big Data Project (BDP) to explore the potential benefits of storing copies of key observations and model outputs in the Cloud to allow computing directly on the data without requiring further distribution. Such an approach could help form new lines of business and economic growth while making NOAA's data more easily accessible to the American public.

This page includes information on data structure; you can find much [more detailed information about GOES-R Series data from NOAA](http://www.goes-r.gov/).

Examples of how to access the objects via the AWS CLI can be seen below.

`aws s3 ls noaa-goes16 --no-sign-request`

`aws s3 ls noaa-goes17 --no-sign-request`

`aws s3 cp s3://noaa-goes16/<Product>/<Year>/<Day of Year>/<Hour>/<Filename> . --no-sign-request`

*Data will be moved to different Amazon S3 storage classes on a schedule. It will start in Standard and move to Infrequent Access after 30 days.*

## About the Data
All data files from GOES-16 (formerly GOES-R) & GOES-17 are provided in netCDF4 format. The GOES-16 data is hosted in the `noaa-goes16` Amazon S3 bucket in the us-east-1 AWS region. The GOES-17 data is hosted in the `noaa-goes17` Amazon S3 bucket in the us-east-1 AWS region. Individual files are availabe in the netCDF format with the following schema:

`<Product>/<Year>/<Day of Year>/<Hour>/<Filename>`

where:

- `<Product>` is the product generated from one of the sensors aboard the satellite (e.g.)
  - ABI-L1b-RadF - Advanced Baseline Imager Level 1b Full Disk
  - ABI-L1b-RadC - Advanced Baseline Imager Level 1b CONUS
  - ABI-L1b-RadM - Advanced Baseline Imager Level 1b Mesoscale
  - ABI-L2-ACHAC - Advanced Baseline Imager Level 2 Cloud Top Height CONUS
  - ABI-L2-ACHAF - Advanced Baseline Imager Level 2 Cloud Top Height Full Disk
  - ABI-L2-ACHAM - Advanced Baseline Imager Level 2 Cloud Top Height Mesoscale
  - ABI-L2-ACHTF - Advanced Baseline Imager Level 2 Cloud Top Temperature Full Disk
  - ABI-L2-ACHTM - Advanced Baseline Imager Level 2 Cloud Top Temperature Mesoscale
  - ABI-L2-ACMC - Advanced Baseline Imager Level 2 Clear Sky Mask CONUS
  - ABI-L2-ACMF - Advanced Baseline Imager Level 2 Clear Sky Mask Full Disk
  - ABI-L2-ACMM - Advanced Baseline Imager Level 2 Clear Sky Mask Mesoscale
  - ABI-L2-ACTPC - Advanced Baseline Imager Level 2 Cloud Top Phase CONUS
  - ABI-L2-ACTPF - Advanced Baseline Imager Level 2 Cloud Top Phase Full Disk
  - ABI-L2-ACTPM - Advanced Baseline Imager Level 2 Cloud Top Phase Mesoscale
  - ABI-L2-ADPC - Advanced Baseline Imager Level 2 Aerosol Detection CONUS
  - ABI-L2-ADPF - Advanced Baseline Imager Level 2 Aerosol Detection Full Disk
  - ABI-L2-ADPM - Advanced Baseline Imager Level 2 Aerosol Detection Mesoscale
  - ABI-L2-AICEF - Ice Concentration and Extent
  - ABI-L2-AITAF - Ice Age and Thickness
  - ABI-L2-AODC - Advanced Baseline Imager Level 2 Aerosol Optical Depth CONUS
  - ABI-L2-AODF - Advanced Baseline Imager Level 2 Aerosol Optical Depth Full Disk
  - ABI-L2-BRFC - Land Surface Bidirectional Reflectance Factor (CONUS) 2 km resolution & DQFs
  - ABI-L2-BRFF - Land Surface Bidirectional Reflectance Factor (Full Disk) 2 km resolution & DQFs
  - ABI-L2-BRFM -  Land Surface Bidirectional Reflectance Factor (Mesoscale) 2 km resolution & DQFs
  - ABI-L2-CMIPC - Advanced Baseline Imager Level 2 Cloud and Moisture Imagery CONUS
  - ABI-L2-CMIPF - Advanced Baseline Imager Level 2 Cloud and Moisture Imagery Full Disk
  - ABI-L2-CMIPM - Advanced Baseline Imager Level 2 Cloud and Moisture Imagery Mesoscale
  - ABI-L2-CODC - Advanced Baseline Imager Level 2 Cloud Optical Depth CONUS
  - ABI-L2-CODF - Advanced Baseline Imager Level 2 Cloud Optical Depth Full Disk
  - ABI-L2-CPSC - Advanced Baseline Imager Level 2 Cloud Particle Size CONUS
  - ABI-L2-CPSF - Advanced Baseline Imager Level 2 Cloud Particle Size Full Disk
  - ABI-L2-CPSM - Advanced Baseline Imager Level 2 Cloud Particle Size Mesoscale
  - ABI-L2-CTPC - Advanced Baseline Imager Level 2 Cloud Top Pressure CONUS
  - ABI-L2-CTPF - Advanced Baseline Imager Level 2 Cloud Top Pressure Full Disk
  - ABI-L2-DMWC - Advanced Baseline Imager Level 2 Derived Motion Winds CONUS
  - ABI-L2-DMWF - Advanced Baseline Imager Level 2 Derived Motion Winds Full Disk
  - ABI-L2-DMWM - Advanced Baseline Imager Level 2 Derived Motion Winds Mesoscale
  - ABI-L2-DMWVC - L2+ Derived Motion Winds - Vapor CONUS
  - ABI-L2-DMWVF - L2+ Derived Motion Winds - Vapor Full Disk
  - ABI-L2-DMWVF - L2+ Derived Motion Winds - Vapor Mesoscale
  - ABI-L2-DSIC - Advanced Baseline Imager Level 2 Derived Stability Indices CONUS
  - ABI-L2-DSIF - Advanced Baseline Imager Level 2 Derived Stability Indices Full Disk
  - ABI-L2-DSIM - Advanced Baseline Imager Level 2 Derived Stability Indices Mesoscale
  - ABI-L2-DSRC - Advanced Baseline Imager Level 2 Downward Shortwave Radiation CONUS
  - ABI-L2-DSRF - Advanced Baseline Imager Level 2 Downward Shortwave Radiation Full Disk
  - ABI-L2-DSRM - Advanced Baseline Imager Level 2 Downward Shortwave Radiation Mesoscale
  - ABI-L2-FDCC - Advanced Baseline Imager Level 2 Fire (Hot Spot Characterization) CONUS
  - ABI-L2-FDCF - Advanced Baseline Imager Level 2 Fire (Hot Spot Characterization) Full Disk
  - ABI-L2-FDCM - Advanced Baseline Imager Level 2 Fire (Hot Spot Characterization) Mesoscale
  - ABI-L2-LSAC  - Land Surface Albedo (CONUS) 2km resolution & DQFs
  - ABI-L2-LSAF - Land Surface Albedo (Full Disk) 2km resolution & DQFs
  - ABI-L2-LSAM - Land Surface Albedo (Mesoscale) 2km resolution & DQFs
  - ABI-L2-LSTC - Advanced Baseline Imager Level 2 Land Surface Temperature CONUS
  - ABI-L2-LSTF - Advanced Baseline Imager Level 2 Land Surface Temperature Full Disk
  - ABI-L2-LSTM - Advanced Baseline Imager Level 2 Land Surface Temperature Mesoscale
  - ABI-L2-LVMPC - Advanced Baseline Imager Level 2 Legacy Vertical Moisture Profile CONUS
  - ABI-L2-LVMPF - Advanced Baseline Imager Level 2 Legacy Vertical Moisture Profile Full Disk
  - ABI-L2-LVMPM - Advanced Baseline Imager Level 2 Legacy Vertical Moisture Profile Mesoscale
  - ABI-L2-LVTPC - Advanced Baseline Imager Level 2 Legacy Vertical Temperature Profile CONUS
  - ABI-L2-LVTPF - Advanced Baseline Imager Level 2 Legacy Vertical Temperature Profile Full Disk 
  - ABI-L2-LVTPM - Advanced Baseline Imager Level 2 Legacy Vertical Temperature Profile Mesoscale
  - ABI-L2-MCMIPC - Advanced Baseline Imager Level 2 Cloud and Moisture Imagery CONUS
  - ABI-L2-MCMIPF - Advanced Baseline Imager Level 2 Cloud and Moisture Imagery Full Disk
  - ABI-L2-MCMIPM - Advanced Baseline Imager Level 2 Cloud and Moisture Imagery Mesoscale
  - ABI-L2-RRQPEF - Advanced Baseline Imager Level 2 Rainfall Rate (Quantitative Precipitation Estimate) Full Disk
  - ABI-L2-RSRC - Advanced Baseline Imager Level 2 Reflected Shortwave Radiation Top-Of-Atmosphere CONUS
  - ABI-L2-RSRF - Advanced Baseline Imager Level 2 Reflected Shortwave Radiation Top-Of-Atmosphere Full Disk
  - ABI-L2-SSTF - Advanced Baseline Imager Level 2 Sea Surface (Skin) Temperature Full Disk
  - ABI-L2-TPWC - Advanced Baseline Imager Level 2 Total Precipitable Water CONUS
  - ABI-L2-TPWF - Advanced Baseline Imager Level 2 Total Precipitable Water Full Disk
  - ABI-L2-TPWM - Advanced Baseline Imager Level 2 Total Precipitable Water Mesoscale
  - ABI-L2-VAAF - Advanced Baseline Imager Level 2 Volcanic Ash: Detection and Height Full Disk
  - EXIS-L1b-SFEU - EXIS-Solar Flux: EUV 
  - EXIS-L1b-SFXR - EXIS-Solar Flux: X-Ray 
  - GLM-L2-LCFA - Geostationary Lightning Mapper Level 2 Lightning Detection 
  - MAG-L1b-GEOF - MAG-Geomagnetic Field
  - SEIS-L1b-EHIS - SEISS-Energetic Heavy Ions 
  - SEIS-L1b-MPSH - SEISS-Mag. Electrons & Protons: Med & High Energy
  - SEIS-L1b-MPSL - SEISS-Mag. Electrons & Protons: Low Energy
  - SEIS-L1b-SGPS - SEISS-Solar & Galactic Protons
  - SUVI-L1b-Fe093 - Solar Ultraviolet Imager Level 1b Extreme Ultraviolet
  - SUVI-L1b-Fe131 - Solar Ultraviolet Imager Level 1b Extreme Ultraviolet
  - SUVI-L1b-Fe171 - Solar Ultraviolet Imager Level 1b Extreme Ultraviolet
  - SUVI-L1b-Fe195 - Solar Ultraviolet Imager Level 1b Extreme Ultraviolet
  - SUVI-L1b-Fe284 - Solar Ultraviolet Imager Level 1b Extreme Ultraviolet
  - SUVI-L1b-He303 - Solar Ultraviolet Imager Level 1b Extreme Ultraviolet
- `<Year>` is the year the netCDF4 file was created
- `<Day of Year>` is the numerical day of the year (1-365)
- `<Hour>` is the hour the data observation was made
- `<Filename>` is the name of the file containing the data. These are compressed and encapsulated using the netCDF4 standard.

A `<Filename>` is delineated by underscores '_' and looks like this:

`OR_ABI-L1b-RadF-M3C02_G16_s20171671145342_e20171671156109_c20171671156144.nc`

where:

- `OR`: Operational system real-time data
- `ABI-L1b-RadF-M3C02` is delineated by hyphen '-':
  - `ABI`: is ABI Sensor
  - `L1b`: is processing level, L1b data or L2
  - `Rad`: is radiances. Other products include CMIP (Cloud and Moisture Imagery products) and MCMIP (multichannel CMIP).
  - `F`: is full disk (normally every 15 minutes), C is continental U.S. (normally every 5 minutes), M1 and M2 is Mesoscale region 1 and region 2 (usually every minute each)
  - `M3`: is mode 3 (scan operation), M4 is mode 4 (only full disk scans every five minutes – no mesoscale or CONUS)
  - `C02`: is channel or band 02, There will be sixteen bands, 01-16
- `G16`: is satellite id for GOES-16 (future G17)
- `s20171671145342`: is start of scan time
  - 4 digit year
  - 3 digit day of year
  - 2 digit hour
  - 2 digit minute
  - 2 digit second
  - 1 digit tenth of second
- `e20171671156109`: is end of scan time
- `c20171671156144`: is netCDF4 file creation time
- `.nc` is netCDF file extension
