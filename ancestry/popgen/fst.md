# 3.7 Measuring population differentiation with Fst

We introduce a metric known as $F_{ST}$, or the *fixation index*, which is commonly used in population genetics to quantify the differentiation of two or more population groups. This metric can be computed with plink (`plink --fst`), and is also output by ADMIXTURE to measure the differentiation of the K source populations identified.

In addition to these notes, you might also find this video by Sarah Tishkoff helpful: https://www.youtube.com/watch?v=I8RCOI7n4XI.

First, we introduce one more term to quantify genetic variation at a single locus. We previously defined the *minor allele frequency* of a SNP to be the frequency of the minor allele. We can additionally quantify variation using *heterozygosity* ($H$):

$$ H = 1 - \sum_i p^2_i $$

where there are $i$ possible alleles (usually just 2), and $p_i$ gives the frequency of each allele. Intuitively, this number will be 0 when there is no variation (so a single allele as frequency 1), and close to 1 when there are many alleles. For a bi-alleic SNP with two alleles at 50% each, this comes out to 0.5.

The *fixation index* is used to quantify population structure. Consider two different populations, population 1 and population 2. We can compute $F_{ST}$ for a single SNP as:

$$ F_{ST} = \frac{H_{between}-H_{within}}{H_{between}} $$

where:

* $H_{between}$ gives the heterozygosity computed when considering data for all samples at once, regardless of population label.
* $H_{within}$ is the average heterozygosity within the two different populations

The idea is that if the heterozygosity is the same in all populations, this equation will come out to 0, meaning there is no population structure (i.e. population 1 and 2 are not substantially different). On the other hand, if there is far more diversity between populations than within, this number will be close to 1, meaning the populations are very diverged at that SNP.

Above we showed how to compute $F_{ST}$ for a single variant. We can also compute a global mean $F_{ST}$ by averaging across SNPs.

For reference, $F_{ST}$ values between pairs of human populations tend to be in the range of 15% or less. In the problem set, you will use plink to compute pairwise distances between 1000 Genomes populations.