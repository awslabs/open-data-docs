# 1000 Genomes Project and AWS

The 1000 Genomes Project is an international collaboration which has established the most detailed catalogue of human genetic variation, including SNPs, structural variants, and their haplotype context. The final phase of the project sequenced more than 2500 individuals from 26 different populations around the world and produced an integrated set of phased haplotypes with more than 80 million variants for these individuals.

The Amazon mirror contains the complete data set from the project and the data can be found in the  `s3://1000genomes` bucket in the us-east-1 AWS region.

For more information please look at http://www.1000genomes.org. If you have any questions about the data, please email info@1000genomes.org.

## Accessing 1000 Genomes Data

AWS is making the 1000 Genomes Project data publicly available to the community free of charge. Public Data Sets on AWS provide a centralized repository of public data hosted on Amazon Simple Storage Service (Amazon S3). The data can be seamlessly accessed from AWS services such Amazon Elastic Compute Cloud (Amazon EC2) and Amazon Elastic MapReduce (Amazon EMR), which provide organizations with the highly scalable compute resources needed to take advantage of these large data collections. AWS is storing the public data sets at no charge to the community. Researchers pay only for the additional AWS resources they need for further processing or analysis of the data. Learn more about [Public Data Sets on AWS](https://aws.amazon.com/publicdatasets/).

The latest 1000 Genomes Project data is publicly available in the [1000genomes Amazon S3 bucket](http://s3.amazonaws.com/1000genomes).

You can access the data via simple HTTP requests, or take advantage of the AWS SDKs in languages such as Ruby, Java, Python, .NET and PHP.

## Analyzing 1000 Genomes Data

Researchers can use the Amazon EC2 utility computing service to dive into this data without the usual capital investment required to work with data at this scale. AWS also provides a number of [orchestration](https://aws.amazon.com/swf/) and [automation](https://aws.amazon.com/cloudformation/) services to help teams make their research available to others to remix and reuse.

Making the data available via a bucket in Amazon S3 also means that customers can crunch the information using Hadoop via Amazon [Elastic MapReduce](https://aws.amazon.com/emr/), and take advantage of the growing collection of tools for running bioinformatics job flows, such as [CloudBurst](http://sourceforge.net/apps/mediawiki/cloudburst-bio/index.php?title=CloudBurst) and [Crossbow](http://bowtie-bio.sourceforge.net/crossbow/index.shtml).

## Other Sources

**The NIH National Center for Biotechnology Information (NCBI), a division of the National Library of Medicine at NIH:**

- ftp://ftp-trace.ncbi.nlm.nih.gov/1000genomes
- ftp6.ncbi.nlm.nih.gov (for IPv6 access)
- 1000 Genomes : NCBI/NLM/NIH (via Aspera)

**The European Bioinformatics Institute (EMBL-EBI), with support from the Wellcome Trust:**

- ftp://ftp.1000genomes.ebi.ac.uk/vol1/
- http://www.1000genomes.org/aspera (via Aspera)

## Education Grants Program

Educators, researchers and students can apply for free credits to take advantage of the utility computing platform offered by AWS, along with Public Datasets such as the 1000 Genomes Project data. If you're running a genomics workshop or have a research project which could take advantage of the hosted 1000 Genomes dataset, you can apply for an [AWS Grant](https://aws.amazon.com/education/).