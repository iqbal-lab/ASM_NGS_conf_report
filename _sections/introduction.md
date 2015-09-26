---
title: "Introduction"
order: 2
---
This report, submitted as part of our competition entry in the 2015 ASM Rapid NGS pipelines for genomic epidemiology conference in Washington, documents our goals and results in applying our reference-free methods to putative outbreaks. We have demonstrated previously that reference-free methods (Cortex) that preserve polymorphism and focus on sample/strain comparison rather than full-chromosomal reconstructon, result in extremely high accuracy variant discovery and genotyping [Iqbal 2012, Iqbal 2013]. These methods not only generate high confidence SNP, indel and structural variant (SV) calls, but also naturally highlight putative recombination events where SNPs/indels are clustered, and allow multi-sample graph queries to determine gene presence. These methods have now been applied in over ? bacterial publications, including on salmonella [Henk] and listeria [], and are now extremely stable and robust. However the flexibility of the system provides a challenge for the user: should one compare samples one at a time against a reference (and if so which one), should one compare all-against-all (and if so how does that scale as datasets expand), and so on. We have therefore started to build a framework, called Outbryk, specifically for the application of Cortex to outbreaks. We use two "workflows" in Cortex - the "independent" workflow compares samples independently against the reference, collects all the SNp/indel/SV calls, and goes back to genotype all samples at all sites. This workflow has recently been re-engineered to use radically less memory, and now scales to millions of bacteria (the bottleneck is combining VCFs at the end). The second workflow ("joint") compares all-agaonst-all samples in a cluster  in a single "multicolour" de Bruijn graph, and only detects events segregating between these samples. The same re-engineering has been done on this workflow, but is poorly tested so for now this step uses a lot of memory - in principle 500 listeria samples would take 15Gb of RAM for example.

We have two goals:

1. To demonstrate the speed, accuracy and resolution of genome analysis with Cortex. 
2. To trial our new Outbryk pipeline.


The approach we take in Outbryk is straightforward, designed to allow scalability to hundreds of thousands of samples, while providing high resolution access to variation within putative outbreaks. Firstly, multiple jobs are run in parallel, one per sample, discovering variation in each sample by comparison with a single fixed reference genome. A VCF is generated per sample, each job taking between 2 and 8 minutes roughly. A single process then combines all these VCFs to generate a single non-redundant site-list containing all SNPs, indels and SVs called in any sample.  All samples are then genotyped in parallel, at all of these sites, resulting in a final VCF file.

We then use FastTree to build a phylogenetic tree of all samples, based on isolated SNPs only, and use this purely for detecting clusters of samples of interest. Finally, within each of these clusters, we rapidly determine a more appropriate reference genome *for that cluster*, using the MASH tool from Phillippy et al, and jointly discover variation amongst this cluster. The de Bruijn graph of all cluster samples is constructed, and variation that is segregating within these samples is discovered. The reference is only used after this stage, to provide coordinates.
We also use this multi-sample graph to detect presence of genes and phages of interest.
In the case of this conference challenge, it has recently been shown that since the (SNP) mutation rate of Listeria is relatively low, indels and phages can provide extra resolution on clusters that otherwise seem egenetically homogeneous, and we wanted to see if our methods allowed us to see this in these data.

[I'd like a workflow diagram here]


The underlying variant discovery/genotyping methods are mature and robust, but the genomic epidemiology pipeline Outbryk is in rapid development and frankly, not super robust right now. It can be found here: http://github.com/iqbal-lab/outbryk . Our analysis of the conference challenge data is here http://github.com/iqbal-lab/ASM_rapid_ngs_challenge_2015. The latter contains both our intermediate working, various dead-ends as the pipeline was rapidly updated, and our final analyses.

Looking back over this, the biggest challenges for us, in analysing the data and writing up within 10 days, was not actually the genomics as such, but downloading ~8000 samples from the SRA and converting from .sra to .fastq.gz - despite the great speed provided by Aspera, we spent/wasted a lot of time (re)downloading background/comparison samples which were not always available/visible for download, possibly because so many of us were downloading at the same time. As a result of this, one thing we decided to do at the end was to see how long it would take to analyse the Listeria and Salmonella datasets if we already had the other samples on our servers, as if we were really in the business of doing this every day.


This sentence cites an article in the bibliography ([Author, 2014](https://example.com/articles/1)).

This sentence cites another article ([Author, 2015](https://example.com/articles/2)).
