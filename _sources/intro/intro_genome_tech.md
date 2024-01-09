# Chapter 2: Overview of technologies for genome analysis

As human geneticists, we are primarily interested in studing *genetic variation*, or differences in genome sequences across individuals. Multiple technologies now enable profiling genetic variation at massive scale. While the majority of the course will focus on what we can do with all of that data, it is important to have an understanding of the basic sources of data we'll be working with.

Genomics technologies have evolved rapidly over the last 10-20 years. However we will focus on a few main technologies that are most widely used, each with different pros and cons:

* **Genotyping arrays**: Genotyping arrays assay a predefined set of positions in the genome already known to be polymorphic (variable in the population). They typically analyze a million or so positions that are known to be variable across individuals. By focusing on a small subset (<1%) of the genome, they are both very cheap (around $100 or less per person) and result in data that is a pretty manageable size. 

* **Next-generation sequencing (NGS)**: NGS (most notably, Illumina technology) enables sequencing of DNA fragments. However, we cannot read entire chromosomes of DNA at once. Rather, the genome is fragmented into small sequences. NGS generates billions of short *reads* of DNA, usually around 100-150bp each. These short sequences of DNA can then be aligned back to a reference genome. Mutations are identified by comparing observed bases in the reads to those in the reference genome to identify differences. Modern NGS data is highly accurate, with per-base errors less than 1%. 

* **Long-read sequencing**: In the last 5 years or so, technologies enabling much longer reads (10s to hundreds of kilobases long in one go) have become available. The two major technologies for long reads are currently Oxford Nanopore (ONT) and Pacific Biosciences (Pacbio). Until recently, long reads have had very high per-base error rates (around 10-15%). In the last several years, "hifi" technology from Pacbio has brought the error rate down to close to that of Illumina reads.

In the following sections we describe more details about each of these technologies, and some of their pros and cons. Note, there is a lot to know about sequencing technologies and analysis of sequencing data. We will not cover these topics exhaustively in this course and instead will focus mostly on downstream applications once we have a list of genetic variants.

```{tableofcontents}
```