# 8.2 Measuring SNP-based heritability using LMMs

Recall from the GWAS chapter that we used linear mixed models to test for association of a SNP with a phenotype while controlling for genome-wide relatedness using a random effects term:

$$
Y = \beta_j X_j + C\gamma + u + \epsilon
$$

where:
* $Y=\{y_1, ..., y_n\}$ is a vector of phenotypes for each of our $n$ samples. (This is often scaled to have mean 0 and variance 1).
* $X_j=\{x_{j1}, ..., x_{jn}\}$ is a vector of genotypes for SNP $j$ (0s, 1s, and 2s).
* $\beta_j$ is the effect size for SNP $j$ (this is just a scalar value).
* $C$ is an $n\times C$ matrix of covariates (e.g. sex, age, top population PCs) and $\gamma$ is a $C \times 1$ vector of covariate coefficients.
* $\epsilon=\{\epsilon_{1}..., \epsilon_{j}\}$ is a vector of noise terms for each sample.
* $u \sim N(0, \sigma^2_g K)$
* $K$ is an $n \times n$ *genetic relatedness matrix* (GRM). The GRM can be thought of as giving the correlation of genotypes across all SNPs for each pair of samples, adjusted for minor allele frequencies. We will give more details on $K$ below.
* $\sigma^2_g$ is a scalar value quantifying how much phenotypic variance is driven by genetic relatedness.

Now that we are interested in heritability, we just want to know about the sources of variance in the phenotype $Y$. Let's ignore fixed effects (one option is to regress them out of the phenotype beforehand). Then we can write:

$$
Y = u + \epsilon
$$

That is, $Y$ has contributions from both genetic factors ($u$) and noise/environment ($\epsilon$). Taking the variance of each side and assuming no gene-environment interactions:

$$
Var(Y) = Var(u) + Var(e) = \sigma^2_gK+\sigma^2eI
$$

Note, $I$ is the $n \times n$ identity matrix and thus has 1's on the diagonal, and the mean diagonal value of $K$ is close to 1 assuming the SNPs were normalized first to have mean 0 variance 1.

Then:

$$
h^2_{SNP} = \frac{\sigma^2_g}{\sigma^2_g + \sigma^2_e}
$$

gives a measure of the fraction of phenotypic variance explained by genome-wide relatedness across all SNPs measured.

Note, the heritability measured in this way is often substantially less than that measured by pedigree-based methods. This is likely in part due to the fact that $K$ is constructed only from genotyped variants (typically common SNPs) and probably does not capture all truly causal variants, such as rare variants or non-SNP variants. Another possible explanation is that pedigree-based methods capture some non-additive heritability not captured by $h^2_{SNP}$.