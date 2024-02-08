# 7.4 GWAS for case-control traits

In previous sections we focused on GWAS for *quantitative traits*. In some cases, we will instead have *case-control* traits, in which our phenotype is a binary outcome (e.g. 0=control, 1=case) instead of a quantitative value. In this case, the linear regression technique we used previously is not the most appropriate test to use. We'll discuss two alternatives.

## 7.4.1 Chi-square test for case-control traits

Let's consider a single SNP, with alleles T and G. We'd like to test for association between the genotypes at this SNP and a case-control phenotype. We could start by making a table of how many times we saw each *genotype* (TT, TG, or GG) in either cases or controls:

|          | TT       | TG       | GG       | total   |
|----------|----------|----------|----------|---------|
| controls | $r_0=64$ | $r_1=32$ | $r_2=4$  | $R=100$ |
| cases    | $s_0=25$ | $s_1=50$ | $s_2=25  | $S=100$ |
| total    | $n_0=89$ | $n_1=82$ | $n_2=29$ | $N=200$ |

Now, we can convert that to a table of how many times we saw each *allele*:
