# Introduction to population genetics and the forces of evolution

In order to interpret one individual's genome, we will need to consider it in the context of patterns of genetic variation observed across en entire population of people, and the forces driving these patterns. 

We will focus here on four major forces driving patterns of genetic variation in populations

Note, population genetics itself is a whole field. We just touch on basics here that we will need for future applications in personal genomics. For more detailed reading on this topic, we recommend Hartl and Clark's textbook "Principles of Population Genetics".

## Mutations

Mutations are the fuel for evolution. They are also what makes studying our own person genomes interesting! 

Any time a cell divides, the DNA must be replicated, and errors, or mutations, may be introduced. If these errors occur in *germ cells* such as sperm or eggs, the error will be passed on to offspring. That offspring can then spread the mutation to their offspring, who can then spread it to their offspring, and so on. And so a genetic *polymorphism* in the population is born.

Note, mutations may also happen in non-germ cells in our bodies. New cells are being created all the time (e.g. skin cells, blood cells, and others). Mutations that accumulate in those non-germ cells are referred to as *somatic* mutations. We will primarily consider *germline* mutations in this course. (although, somatic mutations are also very interesting and an important contributor to a variety of diseases).

### Types of mutations

Mutations can take many forms. We will focus primarily on the simplest type of mutation, in which a single base is replaced with a different base. You can think of these mutations as "spelling errors". For example, in the following string of nucleotides the fourth "T" is mutated to a "C".

```
AAATGCCG -> AAACGCCG
```

We will refer to these spelling changes as *Single Nucleotide Polymorphisms* or SNPs. (Note, you might also see these referred to as *single nucleotide variants* or SNVs. I tend to just use "SNPs" for everything. But some scientists argue SNV should be used to refer to variants that are private to a single individual, or to somatic variants, whereas SNPs should be used for variants seen in more than one person. This has been the topic of intense [twitter debates](http://lh3.github.io/2021/03/15/snp-vs-snv)).

Note, there are many other interesting ways genomes can vary that we will only consider later on. These include:

* *Indels*, or insertions or deletions of 1 or more nucleotide.
* *Structural variants*, which can involve rearrangements or insertions or deletions of large (>1kb) chunks of the genome.
* *Tandem repeats*, which consist of repeated sequences in tandem. These regions often vary in copy number of the repeat across individuals
* *Copy number variants*, similar to tandem repeats but usually longer. These regions also frequently vary in copy number.

### Properties of mutations

On average, the human mutation rate is around 1.5x10e-8 per base per generation. That is, every base has a 1.5e-8 chance of being mutated in the germline of each new human. While we often think of mutations occurring at random and at a constant rate, mutation rates are not actually constant. They are affected by many factors, including genomic sequence context, age of the parents (especially the father), and more. 

Because mutation rates are relatively slow, most bases in our genome will have only been mutated at most once ever in the course of human history. This forms the bases of the *infinite sites model*, which assumes that every new mutation occurs at a new site in the genome that was not previously mutated.

Two nice properties follow from this:

* All copies of a particular allele in the present-day population likely descended from a single common ancestral chromosome. For example, consider a position in the genome for which the ancestral allele is "G". But some present-day individuals have an "A" at that site. We can usually assume all the copies of "A" allele descend from a single common ancestor.

* The vast majority of SNPs will be *bi-allelic*, meaning there are only two possible alleles. Note, SNPs with more than 2 alleles do exist, but are rare.

Because mutations accumulate at a constant(ish) rate, they can be used as a *molecular clock*. By comparing the genomes (or regions of the genome) of two individuals and counting the number of positions at which they differ, we can infer how long ago those individuals, or those genomic regions, shared a common ancestor.

Finally, we can learn something about the age of a mutation based on how frequent it is. For example, mutations that are very common in many different world populations likely arose long ago before the split of modern populations. On the other hand, mutations that are very rare or only seen in specific population groups likely arose more recently. These properties will help us later on when we use genetic variation to infer an individual's ancestry.

### Hardy-Weinberg Equilibrium and random mating

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

## Genetic drift

As mutations arise, they are then spread to offspring, and some may eventually spread throughout the population. This is a *stochastic process*: the process happens randomly, within a finite population size. As a result, there will be random fluctuations of allele frequencies over time. This phenomenon is known as *genetic drift*.

Consider the following simplified model of evolution of a single position of the genome: we have a starting population of N people (and so, 2N copies of the genome since we are diploid).
The position has two alleles, A and B, with frequencies p and 1-p in the population.
We will assume that the population size is constant at N people over time, and that generations are non-overlapping.
So, every generation, we have N new individuals. Their two alleles are each drawn at random from the 2N available alleles from the previous generation.
This process happens over and over again over many generations of evolution.

This simple process of genetic drift is formalized in the *Wright-Fisher model*.
If there are 2N total alleles, and the frequency of A is p, then the probability that there will be k copies of A in the next generation can be computed using the binomial distribution:

$$P(k copies of A) = (2N choose k)p^k(1-p)^{2NN-k}$$

This process gets repeated each generation. Note, each generation we must recompute p based on the observed frequency of A in the previous generation.

The Wright-Fisher model is essentially a simple random walk. Note that both p=0 and p=1 are *absorbing states*. In the absence of new mutations, once we reach an absorbing state, we are stuck there forever. i.e. once either A or B disappears, the allele frequencies will stay constant in all future generations. We call this *fixation* of one of the alleles (and *loss* of the other). If we run the evolution process for enough generations, eventually we will reach one of these absorbing states.

Some interesting properties about patterns of genetic variation in populations can be derived from this model:

* *Effects of population size*: The smaller the population size, the more dramatic effect genetic drift will have. If a population is very large, frequencies will remain pretty stable across generations due to the law of large numbers. In the extreme case of infinite population size, allele frequencies will stay constant. On the other hand, if a population is very small, due to sampling noise there may be large fluctations in allele frequencies each generation.

* *Effects of population bottlenecks*: A population bottleneck occurs when a small group of founder individuals from an original population forms a new population group, creaing a substantial reduction in population size. This can have dramatic effects on allele frequencies. For example, if an allele that was originally rare was present in one of the small number of founrders, that allele may suddenly become common in the new bottlenecked population. One consequence is that populations such as Ashkenazi Jews, early French Canadian settlers, and Finnish, which experienced recent bottlenecks, have a high rate of recessive disorders which are rare in other groups but common in the bottlenecked groups since they happened to be carried by some of the original founders.

* *Effects of allele frequencies*: An allele that is at 50% frequency as a 50% chance to eventually be lost or become fixed. mAn allele that is rare is more likely to eventually be lost. On the other hand, an allele that is very common is likely to eventually take over and become fixed. Formally, the probability that an allele with frequency p will eventually be fixed is equal to p. The time it takes for fixation to occur is proportional to the population size. Fixation will happen more quickly in small populations and take a long time in large populations.

## Selection

### Incorporating selection into our model for drift
The model of genetic drift above assumed that alleles in each generation are drawn at random from the alleles available from the previous generation. In that case, alleles were considered *neutral*. That is, whether you inherited A vs. B had no effect on your health and ability to produce offspring.

However, in some cases certain alleles can be advantageous, whereas others may be disadvantageous.
These in turn increase or decrease your *fitness*, defined in evolutionary terms as the number of offspring you can produce.
For example, an allele that causes a severe condition in which an individual does not to survive until adulthood has a severe negative fitness effect and is unlikely to be passed on to future generations.
On the other hand, an allele that causes an advantage, such as the ability to digest lactose, might result in an individual being more likely to survive longer and produce more offspring, and so has a positive fitness effect.

These effects can be formally modeled at each SNP in our model as a *selection coefficient* s, which quantifies the relative fitness of one allele compared to another.
In our toy SNP example with alleles A and B, let s be the selection coefficient for A and p be the frequency of A.
In our neutral model, each allele in the next generation had probability p to be an A. 
However, we can bias that probability based on the selection coefficient. i.e. the probability of sampling A will be p(1+s)/(p(1+s)+(1-p)).
Here if s=0, we are back at the neutral model. If s>0, A is beneficial and we'll be biased toward sampling it more often. If s<0, A is deleterious and we will be biased toward sampling it less often.
If s is very high or very low, fixation of the advantageous allele and loss of the disadvantageous allele will happen very quickly.

### Types and examples of selection in humans

The above model of selection is quite simple and does not consider that alleles may confer different selective advantages if they are in a homozygous vs. heterozygous state. We can consider three general categories of selection:

* **Directional selection**: In this case, AA has lowest fitness, AB often has intermediate fitness, and BB has the highest fitness. Examples of loci in the human genome under directional selection are the *LCT* gene that controls our ability to digest lactose and *SLC24A5* associated with skin pigmentation.

* **Stabilizing selection**:  In this case, heterozygotes (AB) have an advantage over homozygotes (AA or BB). An example of this is the gene *HBB* involved in sickle-cell anemia. Having two copies (BB) of mutant HBB results in sickle-cell anemia. However, heterozygotes (AB) have an advantage over homozygotes for AA, in that they are largely resistant to malaria. In cases of stabilizing selection, a deleterious allele is maintained in the population even though it can have a negative impact, since it has an advantage when found in a heterozygous state.

* **Disruptive selection**: In this case, heterozygotes have a disadvantage over either homozygous genotype. There are several examples of this type of selection in non-humans. Let us know if you think of any examples in humans!

While we will not cover them in detail in this course, a number of methods have been developed for identifying regions of the genome under selection. Many of these are reviewed in [Haasl and Payseur](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC4868130/).

## Gene flow

Finally, new genetic material may be introduced by admixture between population groups. This can take multiple forms. For example, we now know that our ancestors interbred with Neanderthal and other ancient human species. These ancient groups contributed to genetic material that is still in present-day populations. Gene flow can also occur when there is admixture between two or more groups that had been previously separated for some time. We will talk much more about admixture in coming weeks as we look at global vs. local ancestry and human history.

## Recommended reading

* [Principles of Population Genetics](https://www.amazon.com/Principles-Population-Genetics-Daniel-Hartl/dp/0878933085) by Hartl and Clark