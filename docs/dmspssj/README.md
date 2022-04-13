# Defense Meterology Satellite Program (DMSP) Precipitating Particle Flux Dataset

The United States Air Force (USAF) Defense Meteorological Satellite Program (DMSP) SSJ precipitating particle instrument measures in-situ total flux and energy distribution of electrons and ions at low earth orbit. These precipitating particles are of interest for space weather operations and research, in part because they produce aurora during normal and very strong geomagnetic storms. This dataset contains both sensor-level raw data (as detailed in Redmon et al. 2017) and a high-level machine-learning-ready data product.

## Sensor-Level (Raw) Data

Raw data is available at `/data/raw/<format>/<year>`

Available formats are:
* `cdf` for [NASA CDF Format](https://cdf.gsfc.nasa.gov/)
* `netcdf` for [Unidata NetCDF](https://www.unidata.ucar.edu/software/netcdf/)

This is the in-situ measurements of particle flux at the location of the DMSP spacecraft. Spacecraft position is provided in geographic (earth-centered-earth-fixed spherical) and geomagnetic coordinates. More information on this dataset is available in [Redmon et al. 2017](https://doi.org/10.1002/2016JA023339)

## ML-Ready data 

ML-Ready data is avaiable in [Apache Parquet](https://parquet.apache.org/) format at `/data/ml-ready/parquet/year=<year>`

It is reduced from raw data as follows:

1. The 19 SSJ energy channels are grouped in 'bands': "hard" (particle kinetic energy >1keV) and "soft" (particle kinetic energy <= 1keV) 
2. Uncertain (low detector count) measurements are discarded using flexible threshold (settable in configuration file)
3. The remaining measurements from all channels in a given band are summed 
4. Data is grouped by orbit (in the ML-ready dataset these would be the 'samples')
5. Data from each orbit is divided into latitude bins and the average flux in each bin is calculated (each bin is a 'feature' in the ML-ready dataset)

## References

* Redmon, R. J., Denig, W. F., Kilcommons, L. M., and Knipp, D. J. (2017), New DMSP database of precipitating auroral electrons and ions, J. Geophys. Res. Space Physics, 122, 9056– 9067, doi:[10.1002/2016JA023339](https://doi.org/10.1002/2016JA023339).
