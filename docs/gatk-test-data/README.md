# GATK Test Data

Overall, this dataset contains public data intended for testing [GATK](https://software.broadinstitute.org/gatk/) and other related tools. Some of the samples referenced here are selected from samples that were sequenced as a part of the [1000 Genomes](http://www.internationalgenome.org/) project. The most commonly referenced sample in this dataset is NA12878, which is genomic DNA derived from the Coriell cell line GM12878, characterized for high-confidence SNPs, indel, and homozygous reference regions. The NA12878 sample was generated as a part of the [Genome in a Bottle](https://www.nist.gov/programs-projects/genome-bottle) project, a public-private-academic consortium hosted by NIST, with the goal to develop reference data used as the gold standard for using analyzing sequence data for clinical purposes. The data from an NA12878 sample is also commonly used as a form of quality control for analytical experiments on sequencing data.

  

The contents of this dataset is multi-modal and includes various types of genomic data, such as CRAMs/BAMs, whole-genome sequencing (WGS) data, exome data, RNA data, etc. The diversity of this dataset is meant to fuel a variety of different types of test cases, such as short variant discovery, RNA seq analysis, Copy Number Variation (CNV) analysis and more. Example pipelines that can be run on this dataset are referenced in the [GATK workflows](https://github.com/gatk-workflows) repository, [Blue Collar Bioinformatics](https://github.com/bcbio/bcbio-nextgen) repository, and many more.

  

All types of data available:

-   Exome BAM
    
-   RNA BAM
    
-   WGS BAM
    
-   WGS CRAM
    
-   WGS Fastq
    
-   WGS gVCF
    
-   WGS uBAM
    
-   WGS VCF
    

  

Data curated for specific types of analysis/tools available:

-   Joint Genotyping
    
-   Copy Number Variation
    
-   RNA Seq Cell Ranger
    
-   Mutect2

## Accessing GATK test data on AWS

The dataset is organized by a directory structure such that each type of data or sample analysis is organized as a sub-directory directly under the bucket `s3://gatk-test-data`:

```
s3://gatk-test-data/README.md
s3://gatk-test-data/1kgp/
s3://gatk-test-data/cnv/
s3://gatk-test-data/exome_bam/
s3://gatk-test-data/intervals/
s3://gatk-test-data/joint_discovery/
s3://gatk-test-data/mutect2/
s3://gatk-test-data/paired-fastq-to-unmapped-bam/
s3://gatk-test-data/qc/
s3://gatk-test-data/refs/
s3://gatk-test-data/rna-seq-cellranger/
s3://gatk-test-data/rna_bam/
s3://gatk-test-data/wgs_bam/
s3://gatk-test-data/wgs_cram/
s3://gatk-test-data/wgs_fastq/
s3://gatk-test-data/wgs_gvcf/
s3://gatk-test-data/wgs_ubam/
s3://gatk-test-data/wgs_vcf/
```

For example, if you're looking to find test data for scientific tools used to analyze whole genome BAM data, an listing of the `wgs_bam` directory shows:
```
s3://gatk-test-data/wgs_bam/NA12878_20k_b37/
s3://gatk-test-data/wgs_bam/NA12878_20k_hg38/
s3://gatk-test-data/wgs_bam/NA12878_24RG_b37/
s3://gatk-test-data/wgs_bam/NA12878_24RG_hg38/
```

The options above are slight variations of the NA12878 data. The annotation in the middle of the directory name, for example `_24RG` is meant to indicate the original data has been downsampled to a size of 24 readgroups. The annotations towards the end of the directory name, such as `_b37` and `_hg38` are meant to indicate the type of  genome reference build that was used for alignment. 


## Contact

This dataset is maintained and updated by the Broad Institute. For any help, please submit question on the [GATK forums](#https://gatkforums.broadinstitute.org/gatk/post/question/gatk-support-forum).

## License

This data has no associated license and is freely open for public use.