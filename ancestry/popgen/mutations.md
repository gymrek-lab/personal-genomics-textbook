# 3.3 Types and patterns of mutations

## Types of mutations

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

## Properties of mutations

On average, the human mutation rate is around 1.5x10e-8 per base per generation. That is, every base has a 1.5e-8 chance of being mutated in the germline of each new human. While we often think of mutations occurring at random and at a constant rate, mutation rates are not actually constant. They are affected by many factors, including genomic sequence context, age of the parents (especially the father), and more. 

Because mutations accumulate at a constant(ish) rate, they can be used as a *molecular clock*. By comparing the genomes (or regions of the genome) of two individuals and counting the number of positions at which they differ, we can infer how long ago those individuals, or those genomic regions, shared a common ancestor.

We can learn something about the age of a mutation based on how frequent it is. For example, mutations that are very common in many different world populations likely arose long ago before the split of modern populations. On the other hand, mutations that are very rare or only seen in specific population groups likely arose more recently. These properties will help us later on when we use genetic variation to infer an individual's ancestry.



## Somatic mutations

Note, mutations may also happen in non-germ cells in our bodies. New cells are being created all the time (e.g. skin cells, blood cells, and others). Mutations that accumulate in those non-germ cells are referred to as *somatic* mutations. We will primarily consider *germline* mutations in this course. (although, somatic mutations are also very interesting and an important contributor to a variety of diseases).


