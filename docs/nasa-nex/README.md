# NASA NEX

[NASA NEX](https://nex.nasa.gov/nex/) is a collaboration and analytical platform that combines state-of-the-art supercomputing, Earth system modeling, workflow management and NASA remote-sensing data. Through NEX, users can explore and analyze large Earth science datasets, run and share modeling algorithms, collaborate on new or existing projects and exchange workflows and results within and among other science communities.

Three NASA NEX datasets are now available to all via Amazon S3. One dataset, the NEX downscaled climate simulations, provides high-resolution climate change projections for the 48 contiguous U.S. states. The second dataset, provided by the Moderate Resolution Imaging Spectroradiometer (MODIS) instrument on NASA's Terra and Aqua satellites, offers a global view of Earth's surface every 1 to 2 days. Finally, the Landsat data record from the U.S. Geological Survey provides the longest existing continuous space-based record of Earth's land.

## Accessing NASA NEX Data

AWS is making the NASA NEX data available to the community free of charge. There are a variety of ways to access the data:

- Simple HTTP requests
- AWS Command Line Tools and SDKs (Ruby, Java, Python, .NET, PHP, etc.)
- [Amazon EC2](https://aws.amazon.com/ec2/)
- [Amazon Elastic MapReduce (Amazon EMR)](https://aws.amazon.com/emr/) (Managed Hadoop service)
- The data is hosted for free by AWS as part of the AWS Public Datasets program.
- Available NASA NEX Datasets

### Downscaled Climate Projections (NEX-DCP30)

The NASA Earth Exchange (NEX) Downscaled Climate Projections (NEX-DCP30) dataset is comprised of downscaled climate scenarios for the conterminous United States that are derived from the General Circulation Model (GCM) runs conducted under the Coupled Model Intercomparison Project Phase 5 (CMIP5) [Taylor et al. 2012] and across the four greenhouse gas emissions scenarios known as Representative Concentration Pathways (RCPs) [Meinshausen et al. 2011] developed for the Fifth Assessment Report of the Intergovernmental Panel on Climate Change (IPCC AR5). The dataset includes downscaled projections from 33 models, as well as ensemble statistics calculated for each RCP from all model runs available. The purpose of these datasets is to provide a set of high resolution, bias-corrected climate change projections that can be used to evaluate climate change impacts on processes that are sensitive to finer-scale climate gradients and the effects of local topography on climate conditions.

Each of the climate projections includes monthly averaged maximum temperature, minimum temperature, and precipitation for the periods from 1950 through 2005 (Retrospective Run) and from 2006 to 2099 (Prospective Run).

Available at `s3://nasanex/NEX-DCP30`

### Global Daily Downscaled Projections (NEX-GDDP)

The NASA Earth Exchange (NEX) Global Daily Downscaled Projections (NEX-GDDP) dataset is comprised of downscaled climate scenarios that are derived from the General Circulation Model (GCM) runs conducted under the Coupled Model Intercomparison Project Phase 5 (CMIP5) [Taylor et al. 2012] and across the two of the four greenhouse gas emissions scenarios known as Representative Concentration Pathways (RCPs) [Meinshausen et al. 2011] developed for the Fifth Assessment Report of the Intergovernmental Panel on Climate Change (IPCC AR5). The dataset is an ensemble of projections from 21 different models and two RCPs (RCP 4.5 and RCP 8.5), and provides daily estimates of maximum and minimum temperatures and precipitation using a daily Bias-Correction - Spatial Disaggregation (BCSD) method (Thrasher, et al., 2012). The data spans the entire globe with a 0.25 degree (~25-kilometer) spatial resolution for the periods from 1950 through 2005 (Historical) and from 2006 to 2100 (Climate Projections).

Available at `s3://nasanex/NEX-GDDP`

### Localized Constructed Analogs (LOCA)

The LOCA (Localized Constructed Analogs) dataset includes downscaled projections from 32 global climate models calculated for two Representative Concentration Pathways (RCP 4.5 and RCP 8.5). Each of the climate projections includes daily maximum temperature, minimum temperature, and precipitation for every 6x6km for the conterminous US from 1950 to 2100. LOCA attempts to better preserve extreme hot days and heavy rain events, regional patterns of precipitation. The total dataset size is approximately 10 TB.

For further information on the methods and contents of the data refer to the source page here.

Available at `s3://nasanex/LOCA`

### MOD13Q1 (Vegetation Indices 16-Day L3 Global 250m)

Global MODIS vegetation indices are designed to provide consistent spatial and temporal comparisons of vegetation conditions. Blue, red, and near-infrared reflectances, centered at 469-nanometers, 645-nanometers, and 858-nanometers, respectively, are used to determine the MODIS daily vegetation indices.

The MODIS Normalized Difference Vegetation Index (NDVI) complements NOAA's Advanced Very High Resolution Radiometer (AVHRR) NDVI products and provides continuity for time series historical applications. MODIS also includes a new Enhanced Vegetation Index (EVI) that minimizes canopy background variations and maintains sensitivity over dense vegetation conditions. The EVI also uses the blue band to remove residual atmosphere contamination caused by smoke and sub-pixel thin cloud clouds. The MODIS NDVI and EVI products are computed from atmospherically corrected bi-directional surface reflectances that have been masked for water, clouds, heavy aerosols, and cloud shadows.

Global MOD13Q1 data are provided every 16 days at 250-meter spatial resolution as a gridded level-3 product in the Sinusoidal projection. Lacking a 250m blue band, the EVI algorithm uses the 500m blue band to correct for residual atmospheric effects, with negligible spatial artifacts.

Vegetation indices are used for global monitoring of vegetation conditions and are used in products displaying land cover and land cover changes. These data may be used as input for modeling global biogeochemical and hydrologic processes and global and regional climate. These data also may be used for characterizing land surface biophysical properties and processes, including primary production and land cover conversion.

Available at `s3://nasanex/MODIS`

### Landsat GLS (Global Land Survey)

In the past, the U.S. Geological Survey (USGS) and NASA collaborated on the creation of four global land datasets from Landsat images: one from the 1970s, and one each from circa 1990, 2000, and 2005. Each of these global datasets was created from the primary Landsat sensor in use at the time: the Multispectral Scanner (MSS) in the 1970s, the Thematic Mapper (TM) in 1990, Enhanced Thematic Mapper Plus (ETM+) in 2000, and a combination of TM and ETM+ in 2005.

Available at `s3://nasanex/Landsat`