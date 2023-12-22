# Chapter 2: Overview of technologies for genome analysis

As human geneticists, we are primarily interested in studing *genetic variation*, or differences in genome sequences across individuals. Multiple technologies now enable profiling genetic variation at massive scale. While the majority of the course will focus on what we can do with all of that data, it is important to have an understanding of the basic sources of data we'll be working with.

Genomics technologies have evolved rapidly over the last 10-20 years. However we will focus on a few main technologies that are most widely used, each with different pros and cons:

* **Genotyping arrays**: SNP genotyping arrays assay a predefined set of positions in the genome already known to be polymorphic. They typically analyze a million or so SNPs. By focusing on a small subset (<1%) of the genome, they are both very cheap (around $100 or less per person) and result in data that is a pretty manageable size. 

* **Next-generation sequencing (NGS)**: NGS (most notably, Illumina technology) enables sequencing of DNA fragments. However, we cannot read entire chromosomes of DNA at once. Rather, the genome is fragmented into small sequences. NGS generates billions of short *reads* of DNA, usually around 100-150bp each. These short sequences of DNA can then be aligned back to a reference genome. Mutations are identified by comparing observed bases in the reads to those in the reference genome to identify differences. Modern NGS data is highly accurate, with per-base errors less than 1%. 

* **Long-read sequencing**: In the last 5 years or so, technologies enabling much longer reads (10s to hundreds of kilobases long in one go) have become available. The two major technologies for long reads are currently Oxford Nanopore (ONT) and Pacific Biosciences (Pacbio). Until recently, long reads have had very high per-base error rates (around 10-15%). In the last several years, "hifi" technology from Pacbio has brought the error rate down to close to that of Illumina reads.

Below we describe more details about each of these technologies, and some of their pros and cons. Note, there is a lot to know about sequencing technologies and analysis of sequencing data. We will not cover these topics exhaustively in this course and instead will focus mostly on downstream applications once we have a list of genetic variants.

## Genotyping arrays

The majority of existing datasets in commerical direct to consumer companies such as 23andme, as well as datasets for large genome-wide association studies, are derived from genotyping arrays. (However, this is changing. within the next year there will be millions of NGS datasaets available from multiple projects including the UK Biobank, All of Us, Million Veterans Program, and others.) Note, you might hear this technology also referred to as SNP chips or SNP microarrays.

While each copy of our genome is around 3 billion base pairs, the majority of that sequence is identical between individuals. In fact, two copies of the genome will tend to differ only at 1 in 1,000 bases. We will refer to these single base pair differences as SNPs, or Single Nucleotide Polymorphisms. Some SNPs are very common - i.e. the same variant may be found in a large percentage of people. Other SNPs are rare, meaning they are not found in many people. We will talk much more about nomenclature and properties of SNPs in the next module.

The main idea of genotyping arrays is to save costs by only analyzing a small subset of the genome. Work from the early days following the human genome project (see the [Hapmap Project](https://www.genome.gov/10001688/international-hapmap-project)) found that the majority of *common* genetic variation in humans could be captured by around 500,000 SNPs. Note, many more common SNPs exist, but many SNPs are highly correlated with each other and thus provide redundant information. Thus, rather than sequencing the entire genome of many people, we can do targeted analysis of these known "tag" SNPs.

To perform SNP genotyping using microarray technology, DNA is fragmented and then hybridized to a large array containing hundreds of thousands of these different probes (short sequences). The majority of SNPs have only two possible bases (*alleles*) that are common in the population. For each SNP, the array has two probes designed to capture each these two possible bases at a given site. By the intensities of the two probesd (let's call them "A" and "B"), we can determine whether a person is *homozygous* for A (AA), homozygous for B (BB), or heterozygous (AB).

Most genotyping arrays target between 500,000 to 1.5 million SNPs. Commonly used arrays from Affymetrix and Illumina, as well as 23andme's custom chip, have around 600,000. Larger arrays exist. For example, the Illumina Human Omni2.5S-8 chip captures around 2.5 million SNPs. There are also custom arrays designed to get higher resolution at certain regions of the genome of interest. For example, a "Human Origins" chip available from ThermoFisher is specifically designed for study of human history. The "immunochip" from Illumina has dense coverage of SNPs near loci associated with major autoimmune and inflammatory diseases.


## Next-generation sequencing

SNP arrays are convenient to perform relatively cheap genotyping of a large number of samples. However, they are limited in that they can only assess a set of pre-determined polymorphic sites. Further, due to historical biases these chips tend to contain SNPs that are most polymorphic in European samples but might be less relevant to individuals with other ancestries.

An alternative is to actually *sequence* someone's genome. That is, we can read the code of the entire genome, and not just a subset. While multiple sequencing technologies exist, the major current technology are from Illumina's next-generation sequencing (NGS) machines.

Sequencing technologies are limited in our ability to read very long stretches of DNA at once. We cannot just take an entire chromosome and read it start to finish. Instead, we sequence short fragments, which we refer to as *reads*. NGS technology typically outputs billions of short reads of 150bp or so. Newer long-read technologies (see below) are capable of generating much longer sequences, but with some tradeoffs in error rates.

We just briefly outline the NGS workflow here. See CSE185 or past offerings of this course for more details.

1. Genomic DNA is isolated from a sample, e.g. from a blood sample or from human cell lines. Note, this results in DNA from many (millions) cells, so we are starting with many many copies of a person's genome.
2. The DNA is fragmented into small fragments of several hundred base pairs. This is because NGS technology cannot sequence very large pieces of DNA in one go.
3. Special adapter sequences are added to these fragments to enable them to be read by the sequencer.
4. Sequences are loaded onto the sequencing machine. They attach to a *flowcell*, where each original fragment is copied many times to increase the amount of DNA available for sequencing. 
5. The sequencer then uses a process of *sequencing by synthesis* to determine the sequence of each DNA fragment. Briefly, the sequencer is loaded with many nucleotides. Each base (A, C, G, or T), is fluorescently labeled with a different color. One base is added to each fragment at a time. After each base is added, the sequencer takes a picture of the flowcell. The color of the fluorescent label indicates which base was added.

For a more detailed view of Illumina NGS technology, take a look at this video: https://www.youtube.com/watch?v=fCd6B5HRaZ8.

This process generates billions of short reads. The reads are highly accurate, with error rates typically less than 1%. These reads can then be used to perform genotyping of SNPs and other complex variants. A typical workflow includes: (1) aligning the short reads to a reference genome sequence and (2) analysis of the bases aligned to each position to identify variable sites.

While we will mostly focus on SNPS in this course, NGS data can be used to genotyping much more complex types of genetic variation, including repeats, copy number variants, and large insertions and deletions.

## Long-read technologies

The vast majority of available whole genome sequencing (WGS) datasets currently are from Illumina technology. While NGS can genotype many types of genetic variation, there are ultimately regions of the genome that cannot be fully captured using short sequence of 150bp or so. This mostly includes long, complex repetitive regions such as telomeres or centromeres, but also large regions of the genome that are duplicated in multiple places.

Long-read technologies now generate reads that are many kilobases long. These have been used to fill remaining gaps in the human reference genome, and gain a much deeper knowledge of structural variation among human genomes. There is a tradeoff though: the longest reads still have far higher per-base error rates (10-15%). Thus, while they are useful for learning big picture genome organization, accurate identification of individual SNPs is challenging. However, long-read technology is rapidly improving, and error rates are expected to continue to fall.

There are currently two main technologies for long-read sequencing:

* **Oxford Nanopore Technologies (ONT)**: ONT works by taking long pieces of DNA (*high molecular weight* DNA) and threading them through a small hole called a nanopore. Each base generates a characteristic electrical signal, which can be used to infer the sequence of bases being threaded through. ONT can generate impressively long read lengths, but with generally higher error rates than Pacbio or Illumina.

* **Pacific Biosciences (PacBio)**: Pacbio has small wells with a polymerase attached to the bottom. High molecular weight DNA is passed through this polymerase, and a laser generates a signal that can be converted to base calls as the sequence passes through the polymerase and bases are added. PacBio can be run in two modes. In both modes, DNA fragments are first converted to circular molecules. (1) *Continuous long read sequencing (CLR)*: this mode generates very long reads (20kb-175kb+) with error rates around 10%. Each DNA fragment is read only once. (2) *Circular consensus sequence (CCS)* (also known as "hifi"). In this mode, somewhat shorter sequences (<20kb) are sequenced. But, the fragment is read multiple times by sequencing around the circular fragment. By reading the same fragment multiple times, errors can be corrected, resulting in low error rates of around 1%.

## Summary of genomics technologies

The different technologies described above have different pros and cons. Generally:

* SNP arrays are cheapest and best for assaying common SNPs in a large number of samples
* NGS gives a more comprehensive view of genetic variation, including rare variants and many more complex variants such as indels and some types of structural variants
* Long-reads can sequence the majority of human genome, include repeats and complex regions. However, it has higher error rates (this is changing rapidly), is more expensive, and is not yet as widely available.

While we will always need to be aware of where the data we are using came from, for most of our analyses we will focus on downstream applications of the resulting SNP genotypes, rather than of analysis of raw genotyping or sequencing data ourselves.

