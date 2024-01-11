# 3.4 Selection

## Incorporating selection into our model for drift
The model of genetic drift above assumed that alleles in each generation are drawn at random from the alleles available from the previous generation. In that case, alleles were considered *neutral*. That is, whether you inherited A vs. B had no effect on your health and ability to produce offspring.

These effects can be formally modeled at each SNP in our model as a *selection coefficient* s, which quantifies the relative fitness of one allele compared to another.
In our toy SNP example with alleles A and B, let s be the selection coefficient for A and p be the frequency of A.
In our neutral model, each allele in the next generation had probability p to be an A. 
However, we can bias that probability based on the selection coefficient. i.e. the probability of sampling A will be p(1+s)/(p(1+s)+(1-p)).
Here if s=0, we are back at the neutral model. If s>0, A is beneficial and we'll be biased toward sampling it more often. If s<0, A is deleterious and we will be biased toward sampling it less often.
If s is very high or very low, fixation of the advantageous allele and loss of the disadvantageous allele will happen very quickly.

## Types and examples of selection in humans

The above model of selection is quite simple and does not consider that alleles may confer different selective advantages if they are in a homozygous vs. heterozygous state. We can consider three general categories of selection:

* **Directional selection**: In this case, AA has lowest fitness, AB often has intermediate fitness, and BB has the highest fitness. Examples of loci in the human genome under directional selection are the *LCT* gene that controls our ability to digest lactose and *SLC24A5* associated with skin pigmentation.

* **Stabilizing selection**:  In this case, heterozygotes (AB) have an advantage over homozygotes (AA or BB). An example of this is the gene *HBB* involved in sickle-cell anemia. Having two copies (BB) of mutant HBB results in sickle-cell anemia. However, heterozygotes (AB) have an advantage over homozygotes for AA, in that they are largely resistant to malaria. In cases of stabilizing selection, a deleterious allele is maintained in the population even though it can have a negative impact, since it has an advantage when found in a heterozygous state.

* **Disruptive selection**: In this case, heterozygotes have a disadvantage over either homozygous genotype. There are several examples of this type of selection in non-humans. Let us know if you think of any examples in humans!

While we will not cover them in detail in this course, a number of methods have been developed for identifying regions of the genome under selection. Many of these are reviewed in [Haasl and Payseur](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC4868130/).
