# T1.2 VCF files

Data you will download from 23andme is in VCF format. 
See the [VCF spec](https://samtools.github.io/hts-specs/VCFv4.2.pdf) for a full description.
VCF files generally have:

* Header lines beginning with `##` which describe the data in each field. They also contain helpful things like the commands uesd to generate the file, information about the reference genome used, etc.
* A final header line beginning with `#CHROM` which gives the column headers of the data in the following rows.
* One line per variant describing:
  - CHROM: the chromosome of the variant
  - POS: the position on the chromosome of the variant
  - ID: the identifier of the variant, for example a dbSNP rsid.
  - REF: the allele in the reference genome
  - ALT: a comma-separated list of alternate alleles (usually there is just 1)
  - QUAL: Describes confidence this is a true variant site.
  - FILTER: contains locus-level filters, e.g. PASS if it passes, or "." if not set, or something else if it fails.
  - INFO: ";"-separated list of locus-specific metrics, such as allele frequencies, imputation quality, etc.
  - FORMAT: ":"-separated list of data fields reported for each sample
  - The remaining part of the line has one column per sample. Each sample has a ":"-separated list of data values according to the order specified in FORMAT> 

The most important FORMAT field is "GT", which gives the genotype of each sample. 0=reference allele, 1=alternate allele 1, 2=alternate allele 2, etc. For example GT=0/0 means a sample is homozygous for the reference allele, 0/1 means heterozygous. Some VCFs will have additional FORMAT and INFO fields with extra metadata. 

Below is an example VCF file:
```
##fileformat=VCFv4.1
##FILTER=<ID=PASS,Description="All filters passed">
##fileDate=20150218
##source=1000GenomesPhase3Pipeline
##INFO=<ID=VT,Number=.,Type=String,Description="indicates what type of variant the line represents">
#CHROM  POS     ID      REF     ALT     QUAL    FILTER  INFO    FORMAT  sample1
16  31002227  rs569316018 C A 100 PASS  VT=SNP GT  0|0
16  31006003  rs143693530 C A 100 PASS  VT=SNP GT  0|1
```

You can see sample1 is homozygous for the reference allele at SNP rs569316018 and heterozygous at SNP rs143693530.

For unphased genotypes, alleles are separated by a "/", meaning they are unordered.
For phased genotypes, alleles are separated by a "|", meaning they are ordered.

You can look at these example VCF files on datahub:

* `~/public/ps1/pset1_1000Genomes_chr16.vcf`
* `~/public/1000Genomes/ALL.chr22.phase3_shapeit2_mvncall_integrated_v5a.20130502.genotypes.vcf.gz`

Note the second is zipped. You can zip and index VCF files using `bgzip` and `tabix`:

```shell
cp ~/public/ps1/pset1_1000Genomes_chr16.vcf . # copy to current dir
bgzip pset1_1000Genomes_chr16.vcf 
tabix -p vcf pset1_1000Genomes_chr16.vcf.gz
```

Indexing allows us to easily extract specific genomic locations with tabix, e.g.:
```shell
tabix pset1_1000Genomes_chr16.vcf.gz 16:31011634-31011635
```

You can use `bcftools` to do many manipulations to VCF files, e.g.:

```shell
bcftools query -l pset1_1000Genomes_chr16.vcf.gz # list the samples in the VCF file
bcftools index -n pset1_1000Genomes_chr16.vcf.gz # list the number of records (variants) in the VCF file
bcftools query -e'AF<0.01' -e'AF>0.99' -f "[%GT\t]\n" pset1_1000Genomes_chr16.vcf.gz # extract tab-separated genotypes
```

If you want to parse a VCF file in Python, rather than writing your own tools to do this, you're better off using existing libraries:
* PyVCF: https://pyvcf.readthedocs.io/
* cyvcf2: https://github.com/brentp/cyvcf2 (recommended. updated and faster than PyVCF)

