---
title: "Discussion"
order: 4
---

Cortex allows both rapid, sensitive and accurate determination of SNP, indel and larger structural variants in bacteria, and we have combined it here with a process of first analysing the challenge data along with thousands of background samples, to get a rough but global phylogeny, followed by a more detailed set of variant calls on specific clusters. By doing multi-sample variant assembly we are also able to assay the accessory genome either via querying for presence of curated sequence (in our case phages) or in a hypothesis free way, simply looking at contig sharing across samples. We were excited to trial the MASH tool from Adam Phillipy's lab, as a means of choosing an appropriate reference genome for each follow-up cluster, but in the case of this data, MASH chose the reference genome we were going to use anyway, so we'll be testing this in more detail in future. 