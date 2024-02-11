# Chapter 8: Heritability

Heritability describes how much variation in a trait is *inherited* (i.e., due to genetics). It has been known for thousands of years that offspring tend to look like their parents, and that many traits tend to run in families. In this section, we will explore several ways to quantify this.

Generally, we can think of someone's phenotype ($P$) as being the sum of effects of both their genetics ($G$) and environment ($E$):

$$
P = G + E
$$

As geneticists, we are not as interested in the value of the phenotype itself so much as in the *variation* of the phenotype across a population. We can decompose the *variance* in the phenotype as follows:

$$
V_p = V_G + V_E + V_{GE}
$$

where $V_p$ is the variance in the phenotype, $V_G$ is the variance in genotypes, $V_E$ is the variance in non-genetic (environmental) factors (e.g. diet, etc.) and $V_{GE}$ is from gene-environment interactions (technically $2Cov(G,E)$). We'll ignore the gene-environment term and assume it is 0 (probably a false assumption in many cases). Then we can define the heritability of a trait as:

$$
h^2 = \frac{V_G}{V_E}
$$

Notes:
* For historical reasons, we use $h^2$, not $h$ to define heritability.
* Some work on heritability distinguishes between *broad sense* heritability (technically, what we defined above), vs *narrow sense heritability* (heritability due only to additive geneticfactors). We will not really distinguish between these in the next sections, and typically will be thinking about strictly additive models.

Heritabilities of some example traits are given below (source: [Visscher et al. 2012](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC3257326/)):

* Height: 80%
* Breast cancer: 30%
* Type 1 Diabetes: 90%
* Type 2 Diabetes: 30-60%
* Obesity: 40-60%
* Schizophrenia: 70-80%
* Platelet count: 80%
* HDL cholesterol: 50%

We'll look at several methods for measuring heritability in closely related individuals, then show two methods for measuring heritability in a cohort of unrelated individuals.

```{tableofcontents}
```