# District of Columbia – Classified Point Cloud LiDAR

LiDAR point cloud data for Washington, DC is available for anyone to use on Amazon S3. This dataset, managed by the Office of the Chief Technology Officer (OCTO), through the direction of the District of Columbia GIS program, contains tiled point cloud data for the entire District along with associated metadata.

LiDAR is a remote sensing method that emits hundreds of thousands of near-infrared light pulses a second to measure distances to the Earth. These light pulses generate precise, 3D information about the shape of the Earth and its surface characteristics. LiDAR is popularly used to make high-resolution maps and digital elevation models, with applications in geodesy, archaeology, geography, geology, seismology, and forestry.

## Accessing DC LiDAR Data on AWS

Tiled, classified LAS files (meeting or exceeding QL1) are available to access individually on Amazon S3. The tile grid is defined by the [DC OCTO grid structure](https://dcgis.maps.arcgis.com/apps/View/index.html?appid=f0145164d65848dc978e887063a53f25). You can also access the DC OCTO grid structure on S3 at `s3://dc-lidar-2015/Classified_LAS/Shapefile/`.

In addition to the LAS file, each tile has a corresponding metadata XML. There is also [a top-level metadata file for the project available via HTTPS](https://dc-lidar-2015.s3.amazonaws.com/Classified_LAS/DC_Octo_Classified_LAS.xml) or via S3 at `s3://dc-lidar-2015/Classified_LAS/DC_Octo_Classified_LAS.xml`.

All files are web-accessible via HTTPS by going [here](https://dc-lidar-2015.s3.amazonaws.com/index.html).

Examples of how to access the files via the [AWS CLI](https://aws.amazon.com/cli/) can be seen below.

`aws s3 ls dc-lidar-2015 --recursive`

`aws s3 cp s3://dc-lidar-2015/Classified_LAS/3316.las 3316.las`

`wget https://s3.amazonaws.com/dc-lidar-2015/Classified_LAS/3316.las`

### Classified Point Cloud Classes

Each point within the point cloud has been classified according to the schema below.

- **Class 1**: Processed, but unclassified
- **Class 2**: Bare earth
- **Class 7**: Low noise
- **Class 9**: Water
- **Class 10**: Ignored ground
- **Class 11**: Withheld
- **Class 17**: Bridge decks
- **Class 18**: High noise


## Contact
If you have questions about the data, please contact the [DC Office of the Chief Technology Officer](https://octo.dc.gov/).

## License
See [Washington, DC Terms of Use](https://dc.gov/page/terms-and-conditions-use).