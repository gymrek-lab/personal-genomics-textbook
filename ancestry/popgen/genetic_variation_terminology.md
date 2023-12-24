# 3.1 Genetic variation terminology

## Terminology for describing genetic variation

For most analyses in this course, we will focus on analysis of single nucleotide polymorphisms (SNPs). We introduce here some basic terminology used to describe a particular SNP.

* We can refer to a SNP in at least two ways:
  - *Genomic coordinates*: We can use chr:pos (e.g. chr1:12345) to refer to the position of a SNP relative to the reference genome. Note, it is always important to include the build of the reference genome when using genomic coordinates. The most recent build of the reference is GRCh38 (or hg38). The previous build was GRCh37 (or hg19). Coordinates change slightly between builds. The [liftover tool](https://genome.ucsc.edu/cgi-bin/hgLiftOver) can be used to convert coordinates from one build to another. Note, some reference builds also differ slightly in how they refer to chromosomes. For example, hg19 uses "chr1", "chr2", etc., whereas GRCh37 uses the same coordinate system but uses "1", "2", etc.
  - *rsid*: Many SNPs also have identifiers based on the [dbSNP](https://www.ncbi.nlm.nih.gov/snp/) database. For example, rs12913832 describes the SNP at position chr15:28120472 in the GRCh38 reference build.

* An *allele* refers to different versions of a region of the genome. Most SNPs are *biallelic*, meaning there are only two possible bases. For example, a SNP might have "A" and "C" as possible alleles. Some SNPs or other variant types can be *multi-allelic*, meaning there are more than two possible alleles.

* The *reference allele* refers to the allele present in the reference genome build. An *alternate allele* refers to an allele that does not match the reference. 

* The *major allele* refers to the allele that is most common in the population. The *minor allele* is the rarer allele. Often but not always, the major allele matches the reference genome. Be careful to not mix these up.

* A *genotype* refers to the combination of alleles a person has at a specific variant region. For a bi-allelic SNP with "A" and "C" as possible alleles, the possible diploid genotypes are "AA", "AC", or "CC".

* A genotype is referred to as *homozygous* if both alleles are the same and *heterozygous* if the two alleles are different.

## Describing patterns of SNP variation

We can summarize variability of a particular SNP in the population based on the following metrics:

* *Minor allele frequency* (MAF) gives the frequency of the minor allele out of all observed alleles in a set of samples. Note, since humans are diploid, each sample has two alleles (except when considering sex chromosomes). So if we observe 6 samples, with genotypes AA, AA, AA, AC, CC, AA, the minor allele is "C", and the minor allele frequency is 3/12 = 0.25.

* *Minor allele count* (MAC) gives the number of times the minor allele was observed in a set of samples.

* *Alternate allele frequency* gives the frequency of the alternate (non-reference) allele.

SNPs are often categorized based on their MAF/MAC. For example:

* A SNP is typically referred to *common* if it has MAF >= 1%. (Note: you might see different thresholds for, but this is most widely used)
* SNPs with MAF < 1% are often referred to as *rare*. (Note: you might see different thresholds for, but this is most widely used)
* A *singleton* is a SNP with MAC of 1. That is, if we are analyzing $N$ individuals, a singleton would have MAF of $1/(2N)$. Singletons are the rarest possible SNP one can observe in a dataset of a particular size.