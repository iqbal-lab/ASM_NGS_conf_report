---
title: "Introduction"
order: 2
---
This report, submitted as part of our competition entry in the 2015 ASM Rapid NGS pipelines for genomic epidemiology conference in Washington, documents our goals and results in applying our reference-free methods to putative outbreaks. We have demonstrated previously that reference-free methods ([Cortex](http://www.nature.com/ng/journal/v44/n2/full/ng.1028.html)) that preserve polymorphism and focus on sample/strain comparison rather than full-chromosomal reconstructon, result in extremely high accuracy variant discovery and genotyping. These methods not only generate high confidence SNP, indel and structural variant (SV) calls, but also naturally highlight putative recombination events where SNPs/indels are clustered, and allow multi-sample graph queries to determine gene presence. These methods are now extremely stable and robust; however the flexibility of the system provides a challenge for the user: should one compare samples one at a time against a reference (and if so which one), should one compare all-against-all (and if so how does that scale as datasets expand), and so on. 


We have therefore started to build a framework, called Outbryk, specifically for the application of Cortex to outbreaks. We use two "workflows" in Cortex - the "independent" workflow compares samples independently against the reference, collects all the SNP/indel/SV calls, and goes back to genotype all samples at all sites. This workflow has recently been re-engineered to use radically less memory, and now scales to millions of bacteria (the bottleneck is combining VCFs at the end). The second workflow ("joint") compares all-against-all samples in a cluster  in a single "multicolour" de Bruijn graph, and only detects events segregating between these samples. The same re-engineering has been done on this workflow, but is poorly tested so for now this step uses a lot of memory - in principle 500 _Listeria monocytogenes_ samples would take 15Gb of RAM for example.

We have two goals:

1. To demonstrate the speed, accuracy and resolution of genome analysis with Cortex. 
2. To trial our new Outbryk pipeline.


The approach we take in Outbryk is straightforward. Firstly, multiple jobs are run in parallel, one per sample, discovering variation in each sample by comparison with a single fixed reference genome. A VCF is generated per sample, each job taking between 2 and 8 minutes roughly. A single process then combines all these VCFs to generate a single non-redundant site-list containing all SNPs, indels and SVs called in any sample.  All samples are then genotyped in parallel, at all of these sites, resulting in a final VCF file.

We then use FastTree to build a phylogenetic tree of all samples, based on isolated SNPs only, and use this purely for detecting clusters of samples of interest. Finally, within each of these clusters, we rapidly determine a more appropriate reference genome *for that cluster*, using the MASH tool from Phillippy et al, and jointly discover variation amongst this cluster. The de Bruijn graph of all cluster samples is constructed, and variation that is segregating within these samples is discovered. The reference is only used after this stage, to provide coordinates.
We also use this multi-sample graph to detect presence of genes and phages of interest.

In the case of this conference challenge, it has recently been shown ([Wang, 2014](doi:10.1128/JCM.00202-15)) that since the (SNP) mutation rate of Listeria is relatively low, indels and phages can provide extra resolution on clusters that otherwise seem egenetically homogeneous, and we wanted to see if our methods allowed us to see this in these data.


The underlying variant discovery/genotyping methods are mature and robust, but the genomic epidemiology pipeline Outbryk is in rapid development and frankly, not super robust right now. It can be found here: http://github.com/iqbal-lab/outbryk . Our analysis of the conference challenge data is here http://github.com/iqbal-lab/ASM_rapid_ngs_challenge_2015. The latter contains both our intermediate working, various dead-ends as the pipeline was rapidly updated, and our final analyses in a state of undress. We won't quite manage to make our exact analysis totally reproducible in time for the deadline, but we will have gone a long way, and this work will continue.

