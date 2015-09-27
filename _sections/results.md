---
title: "Results"
order: 3
---


Just to give a flavour of what one might first do when we pick up a new dataset. we start with:


### Listeria: basic questions purely using challenge samples


We ran the Outbryk pipeline on the 18 Listeria samples using reference genome J2_064. Using 18 cores it took 16 minutes to go from fastq to final VCFs for the Cortex independent workflow. As a result we were immediately able to answer the first question of the challenge: "Do the product isolates from facility #1 match the environmental swabs from the same facility?". Answer: at the SNP level they agreed. We discuss below the question of whether phage presence or indels provide further information.


### _Listeria monocytogenes_ - Outbryk on 18 ASM samples plus 4159 background samples



We ran the Cortex parallelised "independent workflow" first (one command-line instruction documented at this GitHub repo)	
on the 18 ASM samples, plus more than 4000 background samples.

This spread the computation over 18 processors on a single Dell server, using a peak of 30 Gb of RAM. 
Total elapsed compute time was ~2 days to go from fastq to final VCFs for all 4159 samples.
This resulted in 180220 SNP, 5620 indel, 62108 complex variants and 480095 clustered/phased SNPs.



We then produced a restricted VCF file for building an initial phylogenetic tree using FastTree; restricting to biallelic SNPs which did not overlap with other events gave us ?? sites. We then went back and genotyped 1700 samples at these sites (we would have done the whole set, but were concerned about running out of time for the challenge. Looking back we could have done them all)

 We built 3 trees, based on discarding any site with >5% missing data, >15% missing data and >20% missing data. The trees looked consistent around the ASM samples (we did not evaluate them elsewhere) although of course with some loss of resolution in the 20% tree. However it was clear from this that we could restrict our analysis to a set of 144 samples.

[Figure 1](#figure-1)

We used this tree to identify a set of 141 samples (including the 18 challenge samples), and ran the MASH tool to determine the best reference genome to use, and then used the Cortex joint analysis pipeline to get the best possible set of variant calls for this cluster. This step ran in 12 minutes, and found 819 SNPs, 32 indels, 25 complex variants, and 277 clusters of phased SNPs (perhaps recombination events). We then ran FastTree again on the full set of SNP calls from this callset (excluding clusters of phased SNPs which are likely recombination events). This is the tree:

[Figure 2](#figure-2)


The tree revealed a number of clusters, which we then analysed individually. The single sample which is a clear outlier is SRR2352342 (ASM58) which is sequenced to much lower depth (20x) than all the others. This revealed to us that we were allowing too low confidence SNP calls to contribute to our tree; given the time constraints (we spent several days downloading ~6000 samples from the SRA and converting from their format), we have not been able to rerun yet. One of the interesting issues raised by this challenge is the difficulty of scaling efficiently while maintaining an good statistical model when dealing with growing volumes of heterogeneous data (our full Listeria set included old and new Illumina data, high and low coverages, and also some 454 data).

Our pipeline takes manually-specified clusters and automatically analyses them in terms of phage presence/absence:

[Figure 3](#figure-3)

This heatmap is clustered automatically, and shows clearly how certain samples (columns) group together in terms of phage content(phages are rows). The pipeline also takes a multi-sample de Bruijn graph and compares contig (so-called "unitigs" for the experts) presence across samples:

[Figure 4](#figure-4)

and indels (no figure here, nothing to see). Looking at the clusters in turn:



### Facility 2 cluster 

The two facility two samples (SRR2352236, SRR2352235) cluster closely with CFSAN007567 and CFSAN007549 in the tree. However, their phage profiles are significantly different, with SRR2352235 SRR2352236 clustering together based on their phage profile. See


[Figure 5](#figure-5)

There are no INDELs seperating these samples, but there is 1 indel present in the Facility 2 samples which is absent from all the facility 1 samples.





### Facility 1 cluster 

We observed a cluster consisting of the Facility 1 samples and a number of CFSAN samples (CFSAN010069, CFSAN010071, CFSAN010073,CFSAN010074,CFSAN010075, CFSAN010077, CFSAN010089,CFSAN010090, CFSAN010092, CFSAN010093, CFSAN010094, CFSAN010097, CFSAN010098, CFSAN010973, CFSAN011017), all identical at the SNP level. These samples all broadly agreed in phage profile also, with 
a few exceptions: SRR2352237 looks different from all other samples, but this sample had only 16x depth of coverage and should perhaps have been excluded. CFSAN011017 and CFSAN011015 have very similar phage profiles which are distinct from all the other isolates in the clade.


[Figure 6](#figure-6)



### Mobilome/accessory genome comparison 


By working with a joint assembly graph of all samples, we are able very simply to pull out a presence/absence heatmap of contigs across samples. See

[Figure 7](#figure-7)

where each row is a sample (with the SNP phylogeny shown on the right) and each column is a contig >500bp long.
 This shows two things.

 Firstly, the two facility 2 samples (top two rows) have some contigs which are absent from all the other samples (yellow block, top left). On BLAST-ing these we find evidence that these are phage-related mobile elements (we have not had time to examine all of these contigs yet). Some examples:

1. an adenine specific methylase, N12 class, involved in replication, recombination and repair
2. phage/plasmid primase, P4 family, C-terminal domain
 
 
 Secondly, sample SRR2352237 is an outlier here too, with a number of contigs seen in it alone.





### Answering the challenge questions 

_Do the product isolates from facility #1 match the environmental swabs from facility #1?_

Yes.

_At facility #1, the manufacturer claims that since the positive swabs did not come from food contact surfaces the contamination must come from another source. Do the product isolates match any other food/environmental isolates currently in the NCBI/SRA database under BioProjects PRJNA212117 or PRJNA215355?_

Yes, the CFSAN samples mentioned and shown above.


_Do the environmental/product isolates from either facility match clinical isolates in the BioProjects listed in question 1, or any other clinical Listeria monocytogenes isolates available from other public data sources?_

We found various CFSAN matches, as above, but we do not believe they are clinical.

_Can you identify the clinical cases associated with this contamination event?_

No.





### _Salmonella enteritidis_ full analysis on 2602 samples 

We ran the full pipeline on 2602 samples. This took 26 hours to call variants independently on each sample, and then another 15 hours to go back and genotype all samples at the non-redundant union of those callsets.

We found 272,380 SNPs, 31,665 indels, 37544 complex variants (mostly clusters of SNPs and indels), and 330039 clusters of phased SNPs that are often putative recombination events.

The initial tree (built using FastTree) can be found on GitHub here:
[Initial tree](https://github.com/iqbal-lab/ASM_NGS_conf_report/blob/gh-pages/data/salmonella/trees/SALMONELLA_ASM.combined.vcf.filtered_missing.conf_thresh5.missingness_thresh0.05.treea2)

but displayed you can see the tree here:

[Figure 8](#figure-8)



We used this initial tree to split the samples into two sets, along with background samples that were closely related. For each cluster, we used MASH to choose a closer reference genome, and then use the Cortex joint workflow to find segregating variants within these two clusters. We called these (arbitrarily) the "upper" and "lower" groups and analysed them separately. 


We found 1294 SNPs, 67 indels, 2 complex variants and 94 sets of clustered+phased SNPs in the lower group,
and X in the upper group. Trees of these groups are here: [salmonella final trees for upper/lower groups](https://github.com/iqbal-lab/ASM_NGS_conf_report/tree/gh-pages/data/salmonella/trees), and we display them in the following figures


[Figure 9](#figure-9)


[Figure 10](#figure-10)


Two of the 4 clinical isolates in the challenge dataset  were epidemiologially linked to eggs from state 3 were in the lower group (ASM_49,ASM_50). ASM_49 was 1 SNP away from 

* ASM_6 (chicken feed, state 2, site 1)
* ASM9, ASM10 and ASM11 (environmental swabs from state 2, site 1)
* ASM15 (env swab, state2, site 3)
* ASM16 (env swab, state 2, site 4). 
* ASM_50 was also 1 SNP away from all of those except ASM16.

The other 2 of the 4 were in the upper group. 

ASM26 (clinical) was 0 SNPs away from 

* ASM25 (food, traceback to eggs from state 2). 

ASM31 (clinical, traced back to eggs from state 2) was 0 SNPs from 
* 3 samples fron environmental swabs in state 2, site 2 (ASM7,13,17).


_"Do any of the remaining 19 clinical isolates match clinical isolates from question #1?"_

Yes: all of the following "matches" were 0 SNPs apart: 

* ASM26 matched clinical samples ASM37,38,39.
* ASM31 matched clinical samples ASM29,30,32
* ASM49 matched clinical samples ASM46,47,48,50
* ASM50 matched clinical samples ASM46.47,48,49

_"Do they match any of the food or environmental isolates?"_

No


_"Are there additional clinical clusters"_

Yes, see above.




### Phage analysis example

In general we found close agreement in phage profile between samples that were phylogenetically close. However, we did notice one interesting example. Clinical sample ASM_31 was found to be closely related to PNUSAS000339, differing by 2 confident SNPs (coord:1927955, A/G, coverages 40,0 for one sample and 0,69 on the other, and coord:2650779, T/G, with coverages 16,0 on one sample and 0,90 on the other) and one confident indel (coord:721390, TC/T, with coverages 25,0 on one sample and 0,55 on the other). Thus from a pure core-genome analysis one would think they were identical. However we did find noticeable differences in their phage profiles.


[Figure 11](#figure-11)








