# T2.2 Some tips and examples for using plink

[Plink](https://www.cog-genomics.org/plink/1.9/) contains implementation of many different methods and utilities that can be performed on genetic data. There are many different options, which can make it a bit confusing if you've never used it before. This brief tutorial goes over some plink basics, and through some example use cases.

**Note: when using plink, things will run much faster if you use the high memory instances on datahub.**

## The anatomy of a plink command

While there are many different ways to use plink, almost all commands will take the following form:

```
plink \
  <option to specify input genotypes> \
  --out outprefix \
  [command]
  [other options]
```

That is, you must specify:

* The path to the genotype information for your samples. This argument can take multiple possible forms (see below)
* A prefix to name output files. e.g. `--out prefix` will name files `prefix.log`, etc. The exact output files depend on the command used.
* A command to run. e.g. `--pca` or `--genome`. Every run has to have some kind of command, even if it is just a command to output a certain file format, like `--make-bed` or `--recode`.

Finally, you may wish to specify other options, e.g. to restrict analysis to a certain chromosome, filter SNPs by MAF, etc.

Below we talk more about these different arguments.

### Specifying input genotypes

For almost any use case, plink will need access to the actual genotypes (e.g. 0s, 1s, and 2s for each variant) for one or more samples. There are multiple ways to provide genotypes as input to plink. These include:

* `--vcf <file.vcf>`: Plink can directly read VCF files. They can be either bgzipped or not. This is convenient since many big datasets (e.g. 1000Genomes) are in VCF format already. But there are some caveats (see below).

* `--file <prefix>`: The original plink format has several files per dataset: `$prefix.ped` gives family/sample information and genotype calls, and `$prefix.map` specifies the locations, alleles, and IDs of the SNPs. `--file prefix` tells plink to look for `prefix.map` and `prefix.ped`.

* `--bfile <prefix>`: This is a binary (compressed) version of the original plink format. Plink will look for `prefix.bed` (compressed genotypes) and `prefix.bim` (SNP info in a simple text file)

Plink handles many other formats, but all of our data for the course will come in one of the formats above. Read more about plink file formats here: https://www.cog-genomics.org/plink/1.9/formats.


#### Converting between genotype formats

You can use plink to convert genotypes from one format to another. e.g.:

```
VCF=/datasets/cs284-sp22-a00-public/1000Genomes/ALL.chr21.phase3_shapeit2_mvncall_integrated_v5a.20130502.genotypes.vcf.gz

# Convert VCF to ped/map
# (using only couple of variants to make this smaller)
plink \
 --vcf $VCF \
 --recode \
 --out test \
 --from rs559462325 --to rs565663130

# Convert ped/map to bed/bim
plink \
 --file test \
 --make-bed \
 --out test

# Convert bed/bim to VCF
plink \
 --bfile test \
 --recode vcf bgz \
 --out test
 ```

#### Caveats when dealing with VCF files

Plink currently loses some information present in VCF files, which can be a common source of errors and confusion. You shouldn't need to deal with most of these things (except the `--double-id` issue) in your problem sets but they are good to be aware of:

* It doesn't properly deal with multi-allelic variants (https://www.cog-genomics.org/plink/1.9/data#merge3)
* Plink traditionally included both family and sample IDs for each sample. But VCFs only have sample IDs. To deal with this, you'll have to use the `--double-id` option if you want to extract sets of samples. See the 1000 Genomes tutorial from week 1.
* If you convert from VCF to another plink format, it order alleles based on minor vs. major allele. This can be problematic and confusing, since VCFs have a strict notion of reference allele (what is in the reference genome) and alternate allele. See option `--keep-allele-order`.
* Phase data is ignored (and orders might be switched in output files).

## Example plink use case - LD pruning

Some analyses, such as PCA or running ADMIXTURE, assume we have a set of SNPs in linkage *equilibrium*. In those cases, it is common to "prune" SNPs to remove highly correlated SNPs from analysis. See the example below for steps to do this.

```
VCF=/datasets/cs284-sp22-a00-public/1000Genomes/ALL.chr21.phase3_shapeit2_mvncall_integrated_v5a.20130502.genotypes.vcf.gz

# Get a list of independent SNPs
# --indep-pairwise <window size> <step size> <r2 threshold>
# Window size = 50 variants
# Step size = variant count to shift the window at the end of each step
# r2 threshold = 0.1. at each step, pairs of variants in the current window with 
# squared correlation greater than the threshold are noted, and variants are 
# greedily pruned from the window until no such pairs remain
# Outputs prune-example.prune.in (variants to keep) and prune-example.prune.out (variants to prune)
plink --vcf $VCF --indep-pairwise 50 10 0.1 --out prune-example

# Extract a new dataset with only those SNPs
plink --vcf $VCF --extract prune-example.in --out prune-example.pruned --make-bed
```
