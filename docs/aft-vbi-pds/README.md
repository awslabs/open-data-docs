# Amazon Bin Image Dataset


Amazon Fulfillment Centers are bustling hubs of innovation that allow Amazon to deliver millions of products to over 100 countries worldwide with the help of robotic and computer vision technologies. The Amazon Bin Image Dataset contains images and metadata from bins of a pod in an operating Amazon Fulfillment Center. The bin images in this dataset are captured as robot units carry pods as part of normal Amazon Fulfillment Center operations.

Amazon Fulfillment Technologies has made the bin images available free of charge on Amazon S3 to encourage recognition research in a variety of areas, including counting generic items and learning from weakly-tagged data.

## Accessing the Amazon Bin Image Dataset

Over 500,000 bin JPEG images and corresponding JSON metadata files describing items in the bin are available in the `aft-vbi-pds` S3 bucket in the us-east-1 AWS Region. 

Images are located in the bin-images directory, and metadata for each image is located in the metadata directory. Images and their associated metadata share simple numerical unique identifiers. For example, the metadata for the image at [https://aft-vbi-pds.s3.amazonaws.com/bin-images/523.jpg](https://aft-vbi-pds.s3.amazonaws.com/bin-images/523.jpg) is found at [https://aft-vbi-pds.s3.amazonaws.com/metadata/523.json](https://aft-vbi-pds.s3.amazonaws.com/metadata/523.json).

If you use the AWS Command Line Interface, you can list images in the bucket with the "ls" command:

`aws s3 ls s3://aft-vbi-pds/bin-images/`


To download data using the AWS Command Line Interface, you can use the "cp" command. For instance, the following command will copy the image named 523.jpg to your local directory:

`aws s3 cp s3://aft-vbi-pds/bin-images/523.jpg 523.jpg`

## About the Data

Amazon uses a random storage scheme where items are placed into accessible bins with available space, so the contents of each bin are random, rather than organized by specific product types. Thus, each bin image may show only one type of product or a diverse range of products. Occasionally, items are misplaced while being handled, so the contents of some bin images may not match the recorded inventory of that bin.

If you have questions about the data, please email us at amazon-bin-images@amazon.com.

