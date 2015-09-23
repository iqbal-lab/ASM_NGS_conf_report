---
title: Abstract
order: 1
container: header
---
Reference-free methods have been shown to have unmatched accuracy in analysis of genetic sequence data, with the particular benefit of avoiding reference-bias and accessibility of a wider range of genetic variation than simple isolated SNPs. We have been building a framework, called Outbryk, for performing basic genomic epidemiology with the Cortex variation assembler. This report describes our state of progress of development, and the results of our analysis of the challenge data in the 2015 ASM Rapid NGS pipelines for genomic epidemiology conference.

The core of our approach is to use a highly scalable per-sample variant discovery against a fixed reference genome using Cortex, build a unified site-list of SNPs, genotype all samples at all sites, and from this VCF rapidly produce a tree. This tree provides some clustering of samples, and within any potential outbreak cluster, a joint multi-sample reference-free analysis is used, using a refernece genome chosen to be closer to this cluster for coordinates only 
