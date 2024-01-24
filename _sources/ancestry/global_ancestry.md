# Chapter 4: Global ancestry

A major reason many people participate in "direct-to-consumer" genetics is to learn about their ancestry. We will think about two different ways to describe someone's ancestry:

* **Global ancestry**: provides a high-level description of someone's overall ancestry. For example, we might describe someone's ancestry as "European", or "African", or "10% European and 90% East Asian". Depending on our dataset and application, we may be able to be more precise. e.g. "50% Chinese and 50% Japanese."

* **Local ancestry**: describes the ancestral origin of individual fragments of the genome. For example, we might say that the first half of one of your copies of chromosome 1 is of African origin. You can think of *global ancestry* as a genome-wide average of someone's *local ancestry*.

This chapter deals with global ancestry. We will focus on two methods for computing global ancestry: PCA and ADMIXTURE. We will look later in the course at local ancestry.

Note, the topic of inferring ancestry from genomes can be a sensitive topic. Here we will focus on genetically inferred ancestry. [Borrell, et al 2021](https://www.nejm.org/doi/full/10.1056/NEJMms2029562) define:

* **Race** and **ethnicity** are self-ascribed or socially ascribed (e.g. by police or medical staff) identities. 
* **Genetic ancestry** describes the genetic origin of one's population. (This often correlates with the geographic origin of someone's ancestors)

We will focus on *genetic ancestry* here. We will use genetic information to quantitatively measure ancestry. The result will be a continuous definition of ancestry (rather than discrete labels such as "White" or "Black") that is inferred in an unbiasd way from the data.

Importantly, while the terms "race", "ethnicity", and "genetic ancestry" have different meanings, they are often used somewhat interchangeably in genomics and medical literature. In some cases, this has been the cause of great harm especially to under-represented minorities, for example when "race-specific" reference values for certain medical tests are used. We will explore these issues more in coming lectures. 

```{tableofcontents}
```