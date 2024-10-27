# 4.2 Global ancestry analysis with ADMIXTURE

In the previous section we used principal components analysis (PCA) to analyze global ancestry based on SNP genotypes. Here we explore an approach as implemented in the ADMIXTURE program [1] to characterize global ancestry in a different way. 

The main idea of ADMIXTURE is to model each individual as coming from a mixture of different source populations. We can then quantify each person's ancestry in terms of the relative contribution of each of the sources.

## ADMIXTURE model overview

The major components of the ADMIXTURE model are:

* There are $K$ "source populations". We have to choose $K$ up front. But, we don't need to know what the $K$ populations actually correspond to. i.e. we don't need to say one population is European, one is African, etc. We will instead learn what those source populations are.

* $q_{ik}$ gives the fraction of individual $i$'s genome that is from population $k$. For example, we might have individual $i$ who is a mixture of 50% from population 1 and 50% from population 2. (**unknown**)

* $f_{kj}$ gives the minor allele frequency of SNP $j$ in population $k$. For example, we might have a SNP with MAF of 0% in population 1 and 10% in population 2. (**unknown**)

In running ADMIXTURE, we will set $K$ to a fixed value (e.g. 3). Then, we will attempt to infer $Q=\{q_{ik}\}$ and $F=\{f_{kj}\}$ from the data. Below we go through how to frame this as a maximum likelihood problem.

## Writing down the likelihood of $Q$ and $F$

Recall from our discussion of Hardy-Weinberg Equilibrium that if we know the MAF $p$, we can easily compute the frequency of the three possible genotypes at a bi-allelic locus:

* $P(00) = (1-p)^2$
* $P(01) = 2p(1-p)$
* $P(11) = p^2$

Here, we extend the same logic to compute the probability of each possible genotype for each SNP, but taking into account that each individual is a mixture of different source populations. Instead of using $p$, we will use $\sum_{k}q_{ik}f_{kj}$, which computes a weighted MAF, weighted by the ancestry components of a particular individual. So for a single sample we get:

* $P(00) = [\sum_{k}q_{ik}(1-f_{kj})]^2$
* $P(01) = 2[\sum_{k}q_{ik}f_{kj}][\sum_{k}q_{ik}(1-f_{kj})]$
* $P(11) = [\sum_{k}q_{ik}f_{kj}]^2$

 Let $g_{ij}$ be the genotype (number of copies of the minor allele at SNP $j$ in sample $i$). Then, we can write the probabilities above as:

$$ P(g_{ij}|Q=\{q_{ij}\}, F=\{f_{kj}\}) $$

We can compute the likelihood of the entire data by assuming each SNP and each sample are independent. The goal is to find $Q$ and $F$ that maximize the likelihood below:

$$L(Q, F) = \Pi_i \Pi_j P(g_{ij}|Q, F) $$

Note, when we perform likelihood maximization, it is usually best to work with the log of the likelihood function. This is convenient, since it helps with numerical precision issues by avoiding really small numbers. It also turns products into sums which are easier to work with. Below we derive an equation for the log likelihood:

$$\log L(Q, F) = \log \Pi_i \Pi_j P(g_{ij}|Q, F) $$

Using $\log AB = \log A + \log B$ we get:

$$\log L(Q, F) = \sum_i \log \Pi_j P(g_{ij}|Q, F) $$

and applying that rule again:

$$\log L(Q, F) = \sum_i \sum_j \log P(g_{ij}|Q, F) $$

Note the "2" in $P(01) = 2[\sum_{k}q_{ik}f_{kj}][\sum_{k}q_{ik}(1-f_{kj})]$ will become a constant term, $+log2$, in the log likelihood equation. An additive constant does not enter into the maximization problem. When finding the maximum of $\sum_i \sum_j \log P(g_{ij}|Q, F)$, we can simply drop the "2" in $P(01)$ case. Then, we can conveniently write this as a single equation:

$$ \log L(Q, F) = \sum_i \sum_j \log [\sum_{k}q_{ik}f_{kj}]^{g_{ij}}[\sum_{k}q_{ik}(1-f_{kj})]^{2-g_{ij}} $$

Using $\log A^k = k\log A$: we get:

$$ \log L(Q, F) = \sum_i \sum_j g_{ij} \log [\sum_{k}q_{ik}f_{kj}] + (2-g_{ij})\log [\sum_{k}q_{ik}(1-f_{kj})] $$

(which matches the likelihood equation given in Alexander, et al.)

We then must find values for $Q$ and $F$ to maximize this log likelihood. These are further subject to the constraints:

* $q_{ik} \geq 0$
* $\sum_k q_{ik} = 1$
* $0 \leq f_{kj} \leq 1$

Note, this is a very high dimensional problem! There are often hundreds of samples and tens of thousands of SNPs in a single ADMIXTURE run. We do not go through the procedure to optimize this function, but that is a major innovation of the ADMIXTURE program [1].

In the problem set, you'll explore using ADMIXTURE to characterize global ancestry in the 1000 Genomes Project samples. See [2] for an example.

## References

[1] Fast model-based estimation of ancestry in unrelated individuals. Alexander et al. Genome Research 2009.

[2] https://www.nature.com/articles/nature15393/figures/2