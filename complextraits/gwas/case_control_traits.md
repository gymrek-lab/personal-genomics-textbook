# 7.4 GWAS for case-control traits

In previous sections we focused on GWAS for *quantitative traits*. In some cases, we will instead have *case-control* traits, in which our phenotype is a binary outcome (e.g. 0=control, 1=case) instead of a quantitative value. In this case, the linear regression technique we used previously is not the most appropriate test to use. We'll discuss two alternatives.

## 7.4.1 Chi-square test for case-control traits

Let's consider a single SNP, with alleles T and G. We'd like to test for association between the genotypes at this SNP and a case-control phenotype. We could start by making a table of how many times we saw each *genotype* (TT, TG, or GG) in either cases or controls:

|          | TT       | TG       | GG       | total   |
|----------|----------|----------|----------|---------|
| controls | $r_0=25$ | $r_1=50$ | $r_2=25  | $R=100$ |
| cases    | $s_0=64$ | $s_1=32$ | $s_2=4$  | $S=100$ |
| total    | $n_0=89$ | $n_1=82$ | $n_2=29$ | $N=200$ |

Now, we can convert that to a table of how many times we saw each *allele*:

|          | T                 | G                 | total    |
|----------|-------------------|-------------------|----------|
| controls | A=$2*r_0+r_1=100$ | C=$2*r_2+r_2=100$ | $2R=200$ |
| cases    | B=$2*s_0+s_1=160$ | D=$2*s_2+s_1=40$  | $2S=200$ |
| total    | $2n_0+n_1=260$    | $2n_2+n_1=140$    | $2N=400$ |

Now we'd like to ask: is one allele over-represented in cases vs. controls?

We can compute an *odds ratio* for a particular allele. Let's define:

* The odds that T occurs in a case: $B/A$
* The odds that G occurs in a control: $D/C$

Then the *odds ratio* for allele T is defined as:

$$
OR(T) = \frac{B/A}{D/C} = \frac{BC}{AD}
$$

In this case, we get the OR=4. That is, the odds that a T occurs in a case is 4 times higher than the odds that a G occurs in a case.

We'd also like to get a p-value to assess whether this is statistically significant. In this case, the *null hypothesis* is that the OR=1. An OR$>$1 indicates that allele T increases risk, whereas an OR$<$1 indicates that it decreases risk. We can use a Chi-square test to do this. A Chi-square test compares the observed vs. expected counts. We can compute the expected counts in each cell in our table, assuming the alleles were equally distributed in the cases and controls:

|          | T                     | G                    | total    |
|----------|-----------------------|----------------------|----------|
| controls | $(2n_0+n_1)(R/N)=130$ | $(2n_2+n_1)(R/N)=70$ | $2R=200$ |
| cases    | $(2n_0+n_1)(S/N)=130$ | $(2n_2+n_1)(S/N)=70$ | $2S=200$ |
| total    | $2n_0+n_1=260$        | $2n_2+n_1=140$       | $2N=400$ |

For a Chi-quare test, we just take the sum: $\sum_i \frac{(obs_i-exp_i)^2}{exp_i}$ which should follow a $\chi^2(1)$ distribution. We can use the resulting statistic compute a p-value. For our example:

$$
\sum_i \frac{(obs_i-exp_i)^2}{exp_i} = (100-130)^2/130 + (160-130)^2/130 + (100-70)^2/70 + (40-70)^2/70 = 39.6
$$

We can use the CDF of a Chi-square distribution to convert this to get a p-value:

```
from scipy import stats
1 - stats.chi2.cdf(39.6, 1) # result: 3.18e-10
```

We get a highly significant p-value, meaning we reject the null that the OR=1.

Let's check our answer with Python's chi-square test in scipy:

```
stats.chi2_contingency([[100,160],[100,40]], correction=False)
(39.56043956043956, 3.180615853514648e-10, 1, array([[130., 130.],
       [ 70.,  70.]]))
```

Our answer aggrees with scipy!