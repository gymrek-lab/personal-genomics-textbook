# T1.2 Plink files

You will also come across genotypes in the format used by Plink (https://www.cog-genomics.org/plink/1.9/formats), which can perform many different functions, including filter, association testing, IBD calculation, and more.

## Plink text files (fam/ped/map)
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

## Plink binary files (bed/bim/fam)

Plink files may be compressed into binary formats. The binary versions of these files are bed/bim files. Bed files are not human readable but can be converted back to ped/map files.

You can find example plink data in: `~/public/ps2/`:

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
# Note, plink may change the allele order if the major allele
# is not the reference. We use the --a2-allele and 
# --real-ref-alleles options below to force it to correctly
# set ref/alt in the output VCF file
zcat pset1_1000Genomes_chr16.vcf.gz | grep -v "^#" | cut -f 1-5 > gtdata_alleles.tab
plink \
  --file pset1_1000Genomes_chr16 \
  --recode vcf bgz \
  --a2-allele gtdata_alleles.tab 4 3 '#' \
  --real-ref-alleles \
  --exclude exclude.txt \
  --out pset1_1000Genomes_chr16_converted
```

## Plink pgen format (pgen/pvar/psam)

Plink2 has introduced a new binary format, [pgen](https://github.com/chrchang/plink-ng/blob/master/pgen_spec/pgen_spec.pdf), which shows better compute performance especially on recent massive biobank datasets. It also has better support for phased, multi-allelic, and dosage data.

To specify `pgen` input, instead of using `--bfile` or `--file`, you can use `--pfile <PREFIX>`, which looks for files `<PREFIX>.pgen`, `<PREFIX>.pvar`, and `<PREFIX>.psam`. 