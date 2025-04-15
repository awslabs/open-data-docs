# KyFromAbove on AWS

KyFromAbove acquires aerial imagery and LiDAR during leaf-off conditions in the Commonwealth. The imagery typically ranges from 6-inches to 3-inches in resolution and is available from the kyfromabove Amazon S3 bucket in a [Cloud Optimized GeoTiff](https://www.cogeo.org) format. All Phase 2 LiDAR data acquired by the program is [USGS Quality Level](https://www.usgs.gov/3d-elevation-program/topographic-data-quality-levels-qls) 2 (QL2), whereas approximately 40% of Phase 1 LiDAR data is considered QL3. Digital Elevation Models (DEMs) at a 5-foot (Phase 1) or 2-foot (Phase 2) resolution, point cloud data in an LAS format, spot elevations in a geopackage format, and contours in a geopackage format are also available from the kyfromabove Amazon S3 bucket. KyFromAbove LiDAR and imagery data resources are managed in a Kentucky-specific 5000x5000 foot grid (FIPS:1600) (EPSG:3089). Phase 1, Phase2, and Phase 3 imagery acquisitions have been completed. Phase 1 LiDAR acquisitions have been completed and Phase 2 was completed in February of 2024. Phase 3 LiDAR acquistions have been initiated. More details about the program can be found at <https://kyfromabove.ky.gov/>

The KyFromAbove program is administered by the Commonwealth Office of Technology, Office of Architecture and Governance, [Division of Geographic Information](https://technology.ky.gov/services-and-support/Pages/Enterprise-GIS.aspx) (DGI).

KyFromAbove imagery and LiDAR on AWS is available ranging from 2012 to 2024. The two webmaps below include tile-by-tile information regarding the year of acquisition, tilename, and extent. <br>
- <a href="https://kygeonet.maps.arcgis.com/home/webmap/viewer.html?webmap=ba05e691cf3a4acd9583b12ccf09856e">KyFromAbove Imagery coverage map</a><br>
- <a href="https://kygeonet.maps.arcgis.com/home/webmap/viewer.html?webmap=785e6040154e4050bda80049fc12d4a6">KyFromAbove LiDAR coverage map</a>

## Accessing KyFromAbove on AWS

KyFromAbove imagery and LiDAR resources on AWS are managed by the Kentucky Division of Geographic Information. All KyFromAbove resources are available via a publicly accessible bucket.

The files are organized in a Kentucky-specific 5000x5000 foot grid (FIPS:1600) (EPSG:3089). Elevation data is available in Cloud Optimized GeoTIFF (COG) and Geopackage formats depending on the data type, raster or vector. Imagery data is available in Cloud Optimized GeoTIFF (COG) format.

[ArcGIS Pro](https://www.esri.com/en-us/arcgis/products/arcgis-pro/overview) and [QGIS](https://qgis.org/en/site/) are both capable of accessing the data in the buckets on a file-by-file basis. Efforts to build a STAC and other geospatial catalogs are underway.

*A [Cloud Optimized GeoTIFF](https://www.cogeo.org/) (COG) is a GeoTIFF file optimized for hosting on a HTTP file server. COG has an internal organization that enables more efficient workflows on the cloud by supporting HTTP GET range requests, where just parts of a file are requested and returned.*

## Imagery Folder Structure

The KyFromAbove imagery data resources are organized in folders based on data type > project phase > year acquired > resolution, and by season beginning in Phase 3. Natural color orthoimagery (nadir) files are located in the <code>orthos/</code> subfolder and oblique imagery is located in the <code>obliques/</code> subfolder. Folders below that level are organized by project phase, and then by year and resolution. 

### Data Type Folders

<b><u>Orthos: </u></b> <code>orthos/</code> - KyFromAbove ortho imagery for the Commonwealth of Kentucky organized in a 5000x5000 foot grid. Each image tile has been converted to a Cloud Optimized GeoTiff format using a 256x256 tile setting. Phase 1 and 2 data is organized by acquisition year and is currently available for use. Phase 3 will be provided upon completion of the project and will be organized by year and season, imagery is being acquired during the fall and spring leaf-off seasons as sun angle permits.

<b><u>Obliques: </u></b> <code>obliques/</code> - KyFromAbove oblique imagery can be found in this folder. The four oblique views, associated with each nadir image, are provided in a 3-band (RGB) Cloud Optimized GeoTiff format using the default 512x512 tile setting. Phase 3 data is being provided upon completion of each acquisition area of interest. It is organized by year and season (where Season1 = Spring and Season2 = Fall) as imagery is being acquired during the fall and spring leaf-off seasons as sun angle and weather conditions permit. *Note: There are no oblique images available for Phase 1 and 2.*


### Project Phase Folder Naming Conventions

<b><u>Project Phase: </u></b> Folders are used to organize the data by project phase.  
> Example: <code>Phase1/</code>

### Acquisition Year and Resolution Folder Naming Conventions

<b><u>Ortho Imagery: </u></b> <code>KY_KYAPED_Year_Resolution</code>, where KY = Kentucky, KYAPED = "Kentucky Aerial Photography and Elevation Data", Year = calendar Year, and Resolution = Value+Unit.  
> Example: <code>KY_KYAPED_2014_6IN.</code>

<b><u>Oblique Imagery: </u></b> <code>KY_KYAPED_Year_Season_Resolution</code>, where KY = Kentucky, KYAPED = "Kentucky Aerial Photography and Elevation Data", Year = Year Acquired, Season = Season1 for Spring or Season2 for Fall, and Resolution = Value+Unit.  
> Example: <code>KY_KYAPED_2023_Season1_3IN.</code>

### File Naming Conventions

<b><u>Ortho Imagery (Phase1 and Phase2): </u></b> <code>tilename_year_resolution_cog.tif</code>, where tilename = 5K grid tilename, year = Year Acquired, and resolution = 1FT, 6IN, or 3IN.
> Example: <code>N013E284_2012_1FT_cog.tif</code> - *The ortho image for the extent of tile N013E284, acquired in 2012 at a 1-foot resolution.*

<b><u>Ortho Imagery (Phase3): </u></b> <code>tilename_year_season_resolution_cog.tif</code>, where tilename = 5K grid tilename, year = Year Acquired, season = Season1 for Spring or Season2 for Fall, and resolution = 3IN.
> Example: <code>N013E284_2023_Season1_3IN_cog.tif</code> - *The ortho image for the extent of tile N013E284, acquired in the Spring Season of 2023 at a 3-inch resolution.*

<b><u>Oblique Imagery: </u></b> <code>direction_flightline_eventID.tif</code>, where direction = sensor direction, flightline = flightline ID, and eventID = sensor capture instanceID.
> Example: <code>Bwd_2033_8829.tif</code> - *The forward facing oblique image, acquired in flight line 2033, with an eventID (capture instance) of 8829. Bwd, Fwd, Left, Right, and Color (nadir) are all valid oblique imagery file name prefixes for each capture event.*

### Metadata and Tile Grid Folders

Metadata and tile grid folders can be found at the root level within each project phase folder. 

<b><u>Metadata: </u></b> <code>Metadata/</code> - Project-level metadata files in XML format for each year during a given project phase.

<b><u>TileGrid: </u></b> <code>TileGrid/</code> - Geopackages that contain the name, file extents, phase and other information for each COG.

## Elevation Folder Structure

The KyFromAbove elevation data resources are organized in folders based on data type > project phase. The year each tile was acquired can be found in the tile grid for that phase. 

### Data Type Folders

<b><u>Contours: </u></b> <code>Contours/</code> - Topographic contours created from the KyFromAbove Phase 1 LiDAR-derived digital elevation model (DEM) in a geopackage and Esri file geodatabase format. There are four data resources in this folder - 1) KyTopo contours at a 10-foot interval primarily for Western and Central Kentucky, 2) KyTopo contours at a 20-foot interval primarily for Central and Eastern Kentucky, 3) KyTopo contours at a 40-foot interval for Eastern Kentucky, and 4) KyTopo contours at a 5-foot interval for the entire Commonwealth.

<b><u>DEM: </u></b> <code>DEM/</code> - LiDAR-derived digital elevation models (DEM) for the Commonwealth of Kentucky organized in a 5000x5000 foot grid. There are currently three data resources in this folder - 1) Phase 1 LiDAR-derived DEMs at a 5 foot resolution, 2) Phase 2 LiDAR-derived DEMs at a 2 foot resolution, and 3) Phase 3 LiDAR-derived DEMs at a 2 foot resolution. All data has been converted to a Cloud Optimized GeoTIFF (COG) format. Phase 2 and Phase 3 efforts have not achieved Statewide coverage to date.

<b><u>KyTopoMapSeries: </u></b> <code>KyTopoMapSeries/</code> - There are three data resources in this folder - 1) KyTopo Map Series quadrangles in a Cloud Optimized GeoTIFF (COG) format, 2) KyTopo Map Series quadrangles with all collar information in a non-georeferenced PNG format for printing on a standard ARCH-D sized sheet, and 3) the KyTopo Map Series quadrangles tile grid in a geopackage format. The COGs were created using GDAL with JPEG compression at a 90% quality setting and the default 512x512 tile setting.

<b><u>Spot Elevations: </u></b> <code>Spot Elevations/</code> - The data in this bucket includes spot elevations for the entire Commonwealth of Kentucky generated from the KyFromAbove Phase 1 LiDAR-derived digital elevation model (DEM) in a geopackage format. ArcGIS was used to create this dataset. Spot elevations for Phase 2 and Phase 3 will be generated upon completion of each Phase.

### Project Phase Folder Naming Conventions

<b><u>Project Phase: </u></b> Folders are used to organize the data by project phase.
> Example: <code>Phase1/</code>

### File Naming Conventions

<b><u>Digital Elevation Models (DEMs): </u></b> <code>tilename_year_DEM_phase_cog.tif</code>, where tilename = 5K grid tilename, year = Year Acquired, DEM = Digital Elevation Model, phase = Project Phase, and cog = Cloud-Optimized GeoTIFF format.
> Example: <code>N017E286_2011_DEM_Phase1_cog.tif</code> - *The elevation data for the extent of tile N017E286, acquired in 2011, during Phase 1, and in a COG format.*

### Metadata and Tile Grid Folders

Metadata and tile grid folders can be found at the root level within each project phase folder.

<b><u>Metadata: </u></b> <code>Metadata/</code> - Project-level metadata files in XML format for each year during a given project phase.

<b><u>TileGrid: </u></b> <code>TileGrid/</code> -  Geopackages that contain the name, file extents, phase and other information for each COG.


## Imagery Processing Information

Phase 1 and Phase 2 orthoimagery GeoTIFFs were converted to COGs using the Geospatial Data Abstract Library (GDAL) v3.5.3. Below is a sample single-line command used by DGI for converting ortho imagery.

> <code>gdal_translate "SourcePath/filename" "DestinationPath/filename" -of COG -co COMPRESS=JPEG -co QUALITY=95 -co NUM_THREADS=32 -co OVERVIEWS=IGNORE_EXISTING -co BLOCKSIZE=256</code>

NV5/Sanborn converted the Phase 3, three-band, oblique and associated nadir images, to COGs using the sample single-line command shown below.

> <code>gdal_translate.exe -b 1 -b 2 -b 3 "SourcePath/filename" "DestinationPath/filename" -of COG -co COMPRESS=JPEG -co BLOCKSIZE=512 -co QUALITY=95 -co NUM_THREADS=16 -co OVERVIEWS=IGNORE_EXISTING</code>

NV5 converted the Phase 3, four-band, ortho images, to COGs using the sample single-line command shown below.

> <code>gdal_translate.exe -b 1 -b 2 -b 3 -b 4 "SourcePath/filename" "DestinationPath/filename" -of COG -co COMPRESS=JPEG -co BLOCKSIZE=512 -co QUALITY=95 -co NUM_THREADS=16 -co OVERVIEWS=IGNORE_EXISTING</code>

## Elevation Processing Information

Phase 1 LiDAR-derived digital elevation models (DEMs) were converted to COGs using the Geospatial Data Abstract Library (GDAL) v3.5.3. Below is a sample single-line command used by DGI for converting the DEMs.

> <code>gdal_translate "SourcePath/filename" "DestinationPath/filename" -of COG -co COMPRESS=LZW -co NUM_THREADS=10 -co OVERVIEWS=IGNORE_EXISTING</code>

**It is important that a lossless compression option is used when creating COGs of digital elevation models (DEMs).*

## Vector Data Processing and Map Production Information

DGI uses Esri's stack of tools for the creation and maintenence of all enterprise-based geospatial data layers. QGIS was used to convert contours, spot elevations, and tile grids to a geopackage format for distribution on AWS. More information regarding the KyTopo Map series production process can be found [here](https://www.esri.com/about/newsroom/arcuser/kytopo-kentuckys-new-topographic-map-series/).

## More Information

More information can be found at [kyfromabove.ky.gov](https://kyfromabove.ky.gov).

<em>The KyFromAbove S3 buckets can be accessed by anyone on the internet (Publicly Accessible).</em>
