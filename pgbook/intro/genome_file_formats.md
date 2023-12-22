# Tutorial: file formats for describing genetic variation

Throughout the course, we will work with data files containing information about genetic variation across one more more samples. Here we will briefly summarize the main file formats genotypes are stored in, some tools for wrangling them, and some additional genotype file formats we'll occasionally be dealing with.

## VCF files

Data you will download from 23andme is in VCF format. VCF files generally have:

* Header lines beginning with `##` which describe the data in each field. They also contain helpful things like the commands uesd to generate the file, information about the reference genome used, etc.
* A final header line beginning with `#CHROM` which gives the column headers of the data in the following rows.
* One line per variant describinging:
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

The most important FORMAT field is "GT", which gives the genotype of each sample. 0=reference allele, 1=alternate allele 1, 2=alternate allele 2, etc. For example GT=0/0 means a sample is homozygous for the reference allele, 1/1 means heterozygous.

For unphased genotypes, alleles are separated by a "/", meaning they are unordered.
For phased genotypes, alleles are separated by a "|", meaning they are ordered.

The full spec is described here: https://samtools.github.io/hts-specs/VCFv4.2.pdf

You can look at these example VCF files on datahub:

* `/datasets/cs284-sp21-A00-public/ps1/pset1_1000Genomes_chr16.vcf` (phased, multi-sample)
* `/datasets/cs185-sp21-A00-public/datasets/week3/friend_genome.vcf.gz` (unphased, single-sample)

Note the second is zipped. You can zip and index VCF files using `bgzip` and `tabix`:

```shell
cp /datasets/cs284-sp21-A00-public/ps1/pset1_1000Genomes_chr16.vcf . # copy to current dir
bgzip pset1_1000Genomes_chr16.vcf 
tabix -p vcf pset1_1000Genomes_chr16.vcf.gz
```

Indexing allows us to easily extract specific genomic locations, e.g.:
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

## Plink files

You will also come across genotypes in the format used by Plink (https://www.cog-genomics.org/plink/1.9/formats), which can perform many different functions, including filter, association testing, IBD calculation, and more.

The text version of plink files includes:

* FAM (sample info): A text file with no header line, and one line per sample with the following six fields:
  - Family ID ('FID')
  - Within-family ID ('IID'; cannot be '0')
  - Within-family ID of father ('0' if father isn't in dataset)
  - Within-family ID of mother ('0' if mother isn't in dataset)
  - Sex code ('1' = male, '2' = female, '0' = unknown)
  - Phenotype value ('1' = control, '2' = case, '-9'/'0'/non-numeric = missing data if case/control)
* PED (genotypes): Contains no header line, and one line per sample with 2V+6 fields where V is the number of variants. The first six fields are the same as those in a .fam file. 
* MAP (variants info) : A text file with no header file, and one line per variant with the following 3-4 fields:
  - Chromosome code. PLINK 1.9 also permits contig names here, but most older programs do not.
  - Variant identifier
  - Position in morgans or centimorgans (optional; also safe to use dummy value of '0')
  - Base-pair coordinate

The binary versions of these files are bed/bim files.

You can find example plink data in: `/datasets/cs284-sp22-a00-public/ps2/`:

When running plink, you will almost always use one of these options:
* `--bfile <prefix>`: uses `<prefix>.bed` and `<prefix>.bim` as input
* `--file <prefix>`: uses `<prefix>.ped`, `<prefix>.map`, and `<prefix>.fam` as input

You will probably eventually encounter the need to convert things between VCF/plink formats. Plink can do that:

```shell
# VCF->plink
echo rs112607901 > exclude.txt # this ID was duplicated
plink \
  --vcf pset1_1000Genomes_chr16.vcf.gz \
  --recode  \
  --exclude exclude.txt \
  --out pset1_1000Genomes_chr16

# Plink->VCF
zcat pset1_1000Genomes_chr16.vcf.gz | grep -v "^#" | cut -f 1-5 > gtdata_alleles.tab
plink \
  --file pset1_1000Genomes_chr16 \
  --recode vcf bgz \
  --a2-allele gtdata_alleles.tab 4 3 '#' \
  --real-ref-alleles \
  --exclude exclude.txt \
  --out pset1_1000Genomes_chr16_converted
```

