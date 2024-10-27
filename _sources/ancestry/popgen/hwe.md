# 3.5 Hardy-Weinberg Equilibrium and random mating

Humans are *diploid*, meaning we all have two genome copies in most of our cells: one maternally inherited and one paternally inherited. 
Under the assumption of *random mating* (i.e. males ane females pair randomly with each other and then have offspring, and this happens again and again each generation), we can think of our two copies of each chromosome as being drawn at random from the pool of available chromosomes in the population. 

For a particular SNP, let there be two possible alleles, A and B, with allele frequencies p and 1-p. Then under the random mating assumption, we'd expect to see the following distributions of genotypes:

* Frequency(AA) = p^2
* Frequency(AB or BA) = 2p(1-p)
* Frequency(BB) = q^2

This relationship is known as *Hardy-Weinberg Equilibrium* (HWE).

Testing for (HWE) is a common quality control step used to assess whether observed genotypes in a population make sense. To test whether genotypes follow HWE, a common analysis is to use a Chi-squared test to determine whether the observed genotype counts of AA, AB, BB deviate from those expected based on the frequencies above.

There are multiple reasons why observed genotypes might not follow HWE. For example:
* *Non-random mating*: For example, if all the "AA"s like to mate with each other, we will see an excess of individuals homozygous for A.
* *Population structure*: This is related to non-random mating. Say our data consists of 50% data from European samples and 50% data from African samples. If we try to compute HWE on the combined genotypes from these datasets, there will be major departures from HWE.
* *Genotyping errors*: Departure from HWE can be an indication that something went wrong. Consider an example: you have data for a SNP with MAF=50%, and all observed genotypes are heterozygous for AB. This is highly unlikely, since we'd expect 25% AA, 50% AB, and 25% BB. In this case, it is highly likely the genotypes are unreliable, perhaps because the probes on the array hybridize equally well to each allele, or recognize non-unique regions of the genome.
* Other cases that will break HWE include selection and small population size.
