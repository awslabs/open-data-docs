# 3000 Rice Genome on AWS


The 3000 Rice Genome Project is an international effort to sequence the genomes of 3,024 rice varieties from 89 countries. The collaborating organizations are comprised of the [Chinese Academy of Agricultural Sciences](http://www.caas.cn/en/), [BGI Shenzhen](http://www.genomics.cn/index), and the [International Rice Research Institute (IRRI)](http://irri.org/). Rice is the leading food source across the globe, and is a vital crop to study to address food security and other global issues. Through analysis of these genomes, researchers can potentially identify genes for important agronomic traits such as better nutrition, climate change tolerance, and disease resistance.

AWS has made the 3000 Rice Genome data freely available on Amazon S3 so that anyone can use our on-demand computing resources to perform analysis and create new products without needing to worry about the cost of storing the data or the time required to download it.

For more information about the 3000 Rice Genomes Project, please visit [http://iric.irri.org/resources/3000-genomes-project](http://iric.irri.org/resources/3000-genomes-project).  

## Accessing 3000 Rice Genome Data on AWS

The whole genome sequence data was analyzed on [the DNAnexus platform](https://wiki.dnanexus.com/Featured-Projects/3000-rice-genomes), comparing each of the 3,024 varieties against five different reference genomes. Over 100TB of results consist of:

Alignment of pair-end reads from whole-genome resequencing of 3,024 rice accessions to 5 published rice reference genomes ([BWA-MEM](http://bio-bwa.sourceforge.net) version 0.7.10)
Discovery of Single Nucleotide Polymorphisms and small indels ([GATK](https://software.broadinstitute.org/gatk/) version 3.2.2)
A description of the analysis steps is available at: s3://3kricegenome/README-snp_pipeline.txt or [http://s3.amazonaws.com/3kricegenome/README-snp_pipeline.txt](http://s3.amazonaws.com/3kricegenome/README-snp_pipeline.txt).

The 3,000 Rice Genome on AWS data set makes available the reference alignments and variant calls available in sorted and indexed BAM files and indexed VCF files, respectively.

The data are organized using a simple directory structure based on the reference genome and source sample.For example, given the source sample IRIS_313–15896 analyzed against the 93–11 reference genome, you would find these associated BAM and VCF files in the following locations:

`s3://3kricegenome/9311/IRIS_313–15896.realigned.bam`

`s3://3kricegenome/9311/IRIS_313–15896.snp.vcf.gz`

Or:

[http://s3.amazonaws.com/3kricegenome/9311/IRIS_313–15896.realigned.bam](http://s3.amazonaws.com/3kricegenome/9311/IRIS_313–15896.realigned.bam)

[http://s3.amazonaws.com/3kricegenome/9311/IRIS_313–15896.snp.vcf.gz](http://s3.amazonaws.com/3kricegenome/9311/IRIS_313–15896.snp.vcf.gz)

The index of BAM and VCF files are co-located for fast random access of files. As an example, here we query for alignments on chromosome 1 from position 1000 to 1100 using samtools:

```
# Query for the chromosome 1 from base position 1000 to 1100

samtools view http://s3.amazonaws.com/3kricegenome/9311/IRIS_313-15896.realigned.bam 9311_chr01:1000-1100
```

Experimental metadata for the study are available via the original publication (`doi:10.1186/2047-217X-3-7`). Summarized experimental metadata is available in [ISATAB format](http://www.isa-tools.org/format/specification/) at:

`s3://3kricegenome/ERP005654.zip`

Or:

[http://s3.amazonaws.com/3kricegenome/ERP005654.zip](http://s3.amazonaws.com/3kricegenome/ERP005654.zip)

A manifest of all files in the bucket is also available at:

`s3://3kricegenome/MANIFEST`

Or:

[http://s3.amazonaws.com/3kricegenome/MANIFEST](http://s3.amazonaws.com/3kricegenome/MANIFEST)

Source sequence data, as well as more details on the experimental data, are available from the Sequence Read Archives (SRA) at [NCBI (USA)](http://www.ncbi.nlm.nih.gov/sra/?term=PRJEB6180), [EBI (Europe)](http://www.ebi.ac.uk/ena/data/view/PRJEB6180), and [DDBJ (Asia)](http://trace.ddbj.nig.ac.jp/DRASearch/study?acc=ERP005654).

The five reference genomes are not part of this Public Data Set, but are available from the following sources:

- Nipponbare (IRGSP-1.0_genome.fasta.gz) [http://rapdb.dna.affrc.go.jp/](http://rapdb.dna.affrc.go.jp/)
- 9311 (9311.fa.gz) ftp://public.genomics.org.cn/BGI/rice_seq/93-11/
- IR64 (os.ir64.cshl.draft.1.0.scaffold.fa.gz) [http://schatzlab.cshl.edu/data/rice/](http://schatzlab.cshl.edu/data/rice/)
- Kasalath (kasalath_genome.tar.gz) [http://rice50ks.dna.affrc.go.jp/](http://rice50ks.dna.affrc.go.jp/)
- DJ123 (os.dj123.cshl.draft.1.0.scaffold.fa.gz) [http://schatzlab.cshl.edu/data/rice/](http://schatzlab.cshl.edu/data/rice/)
