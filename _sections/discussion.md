---
title: "Discussion"
order: 4
---

Cortex allows rapid, sensitive and accurate determination of SNP, indel and larger structural variants in bacteria. Each SNP genotype call comes with a confidence, which is the log likelihood of the data given the called genotype minus the log likelihood of the next most likely genotype. Typically users can use this to calibrate their callset location on the sensitivity-specificity curve.

We have combined Cortex with a process of first analysing samples of interest along with thousands of background samples, to get a rough but "global" phylogeny; this is followed by a more detailed set of variant calls on specific clusters, using a reference genome chosen for appropriateness to that cluster.  We were excited to trial the MASH tool from Adam Phillipy's lab, as a means of choosing an appropriate reference genome for each follow-up cluster, but in the case of this data, MASH chose the reference genome we were going to use anyway, so we'll be testing this in more detail in future. 

By doing multi-sample variant assembly we are also able to assay the accessory genome either via querying for presence of curated sequence (in our case phages) or in a hypothesis free way, simply looking at contig sharing across samples. Even in the relatively small datasets for this challenge, the accessory genome provided interesting and complementary information to the SNP/indel calls.

Outbryk is still currently in the proof of concept stage, and does not make statements about whether a given cluster is an outbreak or not. Outbryk is not yet to be considered alpha software, so users trial it at their own risk, although any user would be viewed with great affection by us. Cortex on the other hand is very robust, and you may choose to integrate it directly into your workflows. It is now hosted on github (https://github.com/iqbal-lab/cortex), a fact which we have not broadcast while we heavily tested the new workflows, which are designed to scale massively, and to make it easiser for users to run. 