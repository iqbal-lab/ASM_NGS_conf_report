---
title: "Results"
order: 3
---


Just to give a flavour of what one might first do when we pick up a new dataset. we start with:


### Listeria: basic questions purely using challenge samples


We ran the Outbryk pipeline on the 18 Listeria samples using reference genome J2_064. Using 18 cores it took ? minutes to go from fastq to final VCFs for the Cortex independent workflow. We found ? SNPs, ? clean indels, ? clusters of phased SNPs, and  complex clusters of SNPs and indels - these latter two might be candidates for recombination events. 

As a result we were immediately able to answer the first question of the challenge

*Do the product isolates from facility #1 match the environmental swabs from the same facility?*

Answer: at the SNP level they agreed. We discuss below the question of whether phage presence or indels provide further information.


### Listeria - Outbryk on 18 ASM samples plus 4143 background samples



We ran the Cortex parallelised "independent workflow" first (one command-line instruction documented at this GitHub repo)	


This spread the computation over 18 processors on a single Dell server, using a peak of 30 Gb of RAM. 
Total elapsed compute time was ~2 days to go from fastq to final VCFs for all 4143 samples.

This resulted in ??? SNP, ? indel, and ?? clustered, phased SNP and indel calls.
The length distribution of indels was:. Each sample had a mean of ? SNPs and ? indels. The min/mean/max number of confident heterozygous calls was ?/?/?


We then produced a restricted VCF file for building an initial phylogenetic tree using FastTree; restricting to biallelic SNPs which did not overlap with other events gave us ?? sites. We built 3 trees, based on discarding any site with >5% missing data, >15% missing data and >20% missing data. The trees looked consistent around the ASM samples (we did not evaluate them elsewhere) although of course with some loss of resolution in the 20% tree. However it was clear from this that we could not restrict our analysis to a set of 144? samples, including ?? from ? and ?? clinical (PNUSAS) samples.

[Figure 1](#figure-1)

We used this tree to identify a set of 141 samples (including the 18 challenge samples), and ran the MASH tool to determine the best reference genome to use, and then used the Cortex joint analysis pipeline to get the best possible set of variant calls for this cluster. This step ran in ? RAM ? time, and found ?. We then ran FastTree again on the full set of SNP calls from this callset (excluding clusters of phased SNPs which are likely recombination events). This is the tree:

[Figure 2](#figure-2)


The tree revealed a number of clusters, which we then analysed individually. The single sample which is a clear outlier is SRR2352342 (ASM58) which is sequenced to much lower depth (20x) than all the others. This revealed to us that we were allowing too low confidence SNP calls to contribute to our tree; given the time constraints (we spent several days downloading ~6000 samples from the SRA and converting from their format), we have not been able to rerun yet. One of the interesting issues raised by this challenge is the difficulty of scaling efficiently while maintaining an good statistical model when dealing with growing volumes of heterogeneous data (our full Listeria set included old and new Illumina data, high and low coverages, and also some 454 data).

Our pipeline takes manually-specified clusters and automatically analyses them in terms of phage presence/absence:

[Figure 3](#figure-3)

This heatmap is clustered automatically, and shows clearly how certain samples (columns) group together in terms of phage content(phages are rows). The pipeline also takes a multi-sample de Bruijn graph and compares contig (so-called "unitigs" for the experts) presence across samples:

[Figure 4](#figure-4)

and  indels (no figure here, nothing to see). Looking at the clusters in turn:



---------
Facility 2 cluster:
---------
The two facility two samples (SRR2352236, SRR2352235) cluster closely with CFSAN007567 and CFSAN007549 in the tree. However, their phage profiles are significantly different, with SRR2352235 SRR2352236 clustering together based on their phage profile. See


[Figure 5](#figure-5)

There are no INDELs seperating these samples, but there is 1 indel present in the Facility 2 samples which is absent from all the facility 1 samples.




---------
Facility 1 cluster:
---------

We observed a cluster consisting of the Facility 1 samples and a number of CFSAN samples (CFSAN010069, CFSAN010071, CFSAN010073,CFSAN010074,CFSAN010075, CFSAN010077, CFSAN010089,CFSAN010090, CFSAN010092, CFSAN010093, CFSAN010094, CFSAN010097, CFSAN010098, CFSAN010973, CFSAN011017), all identical at the SNP level. These samples all broadly agreed in phage profile also, with 
a few exceptions: SRR2352237 looks different from all other samples, but this sample had only 16x depth of coverage and should perhaps have been excluded. CFSAN011017 and CFSAN011015 have very similar phage profiles which are distinct from all the other isolates in the clade.


[Figure 6](#figure-6)


---------
Mobilome/accessory genome comparison
---------

By working with a joint assembly graph of all samples, we are able very simply to pull out a presence/absence heatmap of contigs across samples. See

[Figure 7](#figure-7)

where each row is a sample (with the SNP phylogeny shown on the right) and each column is a contig >500bp long.
 This shows two things.

 1. The two facility 2 samples (top two rows) have some contigs which are absent from all the other samples (yellow block, top left). On BLAST-ing these we find evidence that these are phage-related mobile elements (we have not had time to examine all of these contigs yet). Some examples:
 ..* an adenine specific methylase, N12 class, involved in replication, recombination and repair
 ..* phage/plasmid primase, P4 family, C-terminal domain
 
 2. Sample SRR2352237 is an outlier here too, with a number of contigs seen in it alone.




------
Answering the challenge questions 
------

"Do the product isolates from facility #1 match the environmental swabs from facility #1?"

Yes.

"At facility #1, the manufacturer claims that since the positive swabs did not come from food contact surfaces the contamination must come from another source. Do the product isolates match any other food/environmental isolates currently in the NCBI/SRA database under BioProjects PRJNA212117 or PRJNA215355?"

Yes, the CFSAN samples mentioned and shown above.


"Do the environmental/product isolates from either facility match clinical isolates in the BioProjects listed in question 1, or any other clinical Listeria monocytogenes isolates available from other public data sources?"

Yes, as above.

"Can you identify the clinical cases associated with this contamination event?"

No.





### Salmonella full analysis on 2602 samples 

We ran the full pipeline on 2602 samples. 
We found X SNPs, Y indels, Z complex, W phased SNPs
Initial tree looked like this



What do we see? I'm going to enter below what I see on Rahcel's big tree (missingness 5%)
, but really should only do this on based on joint analysis

 - ASM31 now matches an enviromental isolate ASM14, plus a lagre number of PNUSAS clinical samples. Comparing at the indel and phage level.....

 - ASM48 matches a PNUSAS000470

 Can we resolve the Henk-ASMplus cluster of ASM27.28.35.36.43,47
 ie
 SRR2352211, SRR2352212, SRR2352219, SRR2352220, SRR2352227, SRR2352231
well, the first 3 still together
ie
 SRR2352211, SRR2352212, SRR2352219

ASM47 matches a PNUSAS

WE seem to be missing ASM51 = SRR2352235 from Rachel's tree








***************
***************

Not for final version but keeping this analysis as could be useful


------
Salmonella  - basic analysis based on 50 ASM samples plus ?? NY
------

One (small) initial step in proving the robustness of a bioinformatic pipeline is that it can be used easily by many people, so one of us (den Bakker) therefore ran Outbryk on his servers in Texas. We added ? samples from ? to the 50 samples provided by the ASM for this challenge, because xxxx.

The initial tree from the independent all-sample workflow looked like this:


[ASM_plus_thr5_annotated]

This immediately allows us to answer some questions:

"Do the 4 clinical isolates (ASM_26, ASM_31, ASM_49, and ASM_50) that are epidemiologically linked to eggs from state #2 match any of the environmental or food swabs collected at those facilities?"


ASM26 is a clinical sample from a person known to have consumed food with ASM40 (data provided as part of the challenge) from state 2. We found it was a close match to several other clinical samples (ASM37,38,39), to ASM25, from food traced back to eggs from  state 2.
The closest environmental samples we found were ASM19 (state1, site1, ?? SNPs away), and ASM14/21 from state2-site3 and state1-site1 respectively.


ASM31 (clinical, traced back to eggs from state 2) was a close match to NY56657565 from ????, but to none of the other ASM samples

ASM49 (clinical, tracedback to eggs from state 2) was a close match (quantify) to clinical samples ASM4,48 and environmental swab ASM19 from state 1-site1, and to chicken-feed sample ASM06, from state2-site1.

ASM50 (clinical tracedback to eggs from state 2) was almost identical to a number of samples from state 2 (ASM10,11) AND from state 1(ASM23,04). This close genetic relatedness across that wide geography either suggests a lack of resolution, or transmission (or both)


Looking beyind SNPs at these samples, we want to know:

 - Can we separate state1 and state2 genetically?
 - can we find other samples which cluster with this b lig "ASM-only" cluster


"Do any of the remaining 19 clinical isolates match clinical isolates from question #1? Do they match any of the food or environmental isolates?" 


Yes  ASM26 --> ASM37,38,39

No for ASM31

Yes ASM49 - matches ASM46,48

ASM50 very close to ASM27,28,35,36,,43,47


"Are there additional clinical clusters?"

Yes, clusters:
ASM41,ASM42
ASM44,ASM45
ASM29,32,30  - also match enviro swabs from site2 state2 (ASM7 and 13)







