---
title: "Results"
order: 4
---


### Listeria: basic questions purely using challenge samples


We ran the Outbryk pipeline on the 18 Listeria samples using reference genome J2_064. Using 18 cores it took ? minutes to go from fastq to final VCFs for the Cortex independent workflow. We found ? SNPs, ? clean indels, ? clusters of phased SNPs, and  complex clusters of SNPs and indels - these latter two might be candidates for recombination events. 

As a result we were immediately able to answer the first question of the challenge

*Do the product isolates from facility #1 match the environmental swabs from the same facility?*
Answer: at the SNP level they agreed. We discuss below the question of whether phage presence or indels provide further information.


### Listeria - running the Outbryk pipeline on 18 ASM samples plus 4143 background samples



We ran the Cortex parallelised "independent workflow" with this single command:

perl run_indep_wkflow_with_gnu_par.pl  --index INDEX --ref_fa L2624.fasta --dir_for_ref_objects ref/ --vcftools_dir ~/apps/vcftools_0.1.13/ --outdir results/ --stampy_bin /apps/stampy/1.0.24-py2.7/stampy.py --kmer 31 --mem_height 20 --mem_width 100 --procs 18 --prefix LISTERIA

This spread the computation over 18 processors on a single Dell server, using a peak of 30 Gb of RAM. 
Total elapsed compute time was ~2 days to go from fastq to final VCFs for all 4143 samples.

This resulted in ??? SNP, ? indel, and ?? clustered, phased SNP and indel calls.
The length distribution of indels was:

Each sample had a mean of ? SNPs and ? indels. The min/mean/max number of confident heterozygous calls was ?/?/?


We then produced a restricted VCF file for building an initial phylogenetic tree using FastTree; restricting to biallelic SNPs which did not overlap with other events gave us ?? sites. We built 3 trees, based on discarding any site with >5% missing data, >15% missing data and >20% missing data. The trees looked consistent around the ASM samples (we did not evaluate them elsewhere) although of course with some loss of resolution in the 20% tree. However it was clear from this that we could not restrict our analysis to a set of 144? samples, including ?? from ? and ?? clinical (PNUSAS) samples.


![_figures/list_tree_indep.tiff]

The joint analysis pipeline ran in ? RAM ? time, and found ?
We found ?, tree looked like ?, did indels resolve any otherwise identical samples? Phages?
Some specific example(s) where indel//phage provided better resolution, if such exist



------
Answering the challenge questions - ALL OF THIS NEEDS TO BE UPDATED BASED ON THE JOINT TREE
------

"Do the product isolates from facility #1 match the environmental swabs from facility #1?"


ASM53/SRR2352237 seems to be slightly isolated from the indep-tree - what does joint analysis show?
ASM54/SRR2352238 is extremely similar to the environmental isolates from that facility (specifically xxxxx)


"At facility #1, the manufacturer claims that since the positive swabs did not come from food contact surfaces the contamination must come from another source. Do the product isolates match any other food/environmental isolates currently in the NCBI/SRA database under BioProjects PRJNA212117 or PRJNA215355?"

We did used phylogenetic placement, based on the SNPs called between these samples, to rapidly test all the BioProjects samples from PRJNA212117 and PRJNA215355. We found ????



"Do the environmental/product isolates from either facility match clinical isolates in the BioProjects listed in question 1, or any other clinical Listeria monocytogenes isolates available from other public data sources?"



"Can you identify the clinical cases associated with this contamination event?"

???




-----
 Salmonella full analysis on ? samples 
----

We ran the full pipeline on ? samples (ids here xxxx)
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









The results of the experiment. See [Figure 1](#figure-1) and [Figure 2](#figure-2), as well as [Table 1](#table-1), for details.
