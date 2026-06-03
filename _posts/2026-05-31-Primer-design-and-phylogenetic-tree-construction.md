---
layout: post
title: Primer design and phylogenetic tree construction
date: '2026-05-31'
categories: Protocols
tags: Workflow Bioinformatics
---
### 31/05/2026
## Primer Design and Phylogenetic Analysis of bacteria Using the 16S rRNA Gene 
### *Designing bacterial primers and building phylogenetic trees using primer3 and MEGA*

This protocol provides a brief overview for designing primers for PCR amplification of a gene of interest and the construction of a phylogenetic tree according to sequencing results.

### 1. <u>Choosing a gene for primer design</u>
The most important step of this workflow is choosing which gene to target for primer design. We must select a gene that is highly conserved across many organisms so we can reliably amplify and sequence it, yet contains enough slight variations to differentiate between species.

We will the widely used 16S rRNA gene used to identify bacteria.

The organisms we will study:

* Desulfovibrio vulgaris - KC462187.1
* Desulfovibrio desulfuricans - AF192154.1
* Desulfovibrio marinus - DQ365924.1
* Desulfovibrio hydrothermalis - AF458778.1

### 2. <u>Finding sequences on NCBI</u>

By searching the organism name+16S on NCBI, we will find our sequences in FASTA format.

![NCBI1.png](https://tony-7752.github.io/Research-method-2026-Tony/images/NCBI1.png)
**Figure 1:** NCBI

Using the nucleotide database, we search for our organism and the gene of interest.

We are interested in FASTA format for the sequence.

![NCBI2.png](https://tony-7752.github.io/Research-method-2026-Tony/images/NCBI2.png)
**Figure 2:** Choose the FASTA format

We will obtain the sequences for out organisms and move on to the next step.

```
>KC462187.1 Desulfovibrio vulgaris strain RL 16S ribosomal RNA gene, partial sequence
CGGGTGCTATACATGCAGTCGAGCGAATGGATTAAGAGCTTGCTCTTATGAAGTTAGCGGCGGACGGGTG
AGTAACACGTGGGTAACCTGCCCATAAGACTGGGATAACTCCGGGAAACCGGGGCTAATACCGGATAACA
TTTTGAACCGCATGGTTCGAAATTGAAAGGCGGCTTCGGCTGTCACTTATGGATGGACCCGCGTCGCATT
AGCTAGTTGGTGAGGTAACGGCTCACCAAGGCAACGATGCGTAGCCGACCTGAGAGGGTGATCGGCCACA
CTGGGACTGAGACACGGCCCAGACTCCTACGGGAGGCAGCAGTAGGGAATCTTCCGCAATGGACGAAAGT
CTGACGGAGCAACGCCGCGTGAGTGATGAAGGCTTTCGGGTCGTAAAACTCTGTTGTTAGGGAAGAACAA
GTGCTAGTTGAATAAGCTGGCACCTTGACGGTACCTAACCAGAAAGCCACGGCTAACTACGTGCCAGCAG
CCGCGGTAATACGTAGGTGGCAAGCGTTATCCGGAATTATTGGGCGTAAAGCGCGCGCAGGTGGTTTCTT
AAGTCTGATGTGAAAGCCCACGGCTCAACCGTGGAGGGTCATTGGAAACTGGGAGACTTGAGTGCAGAAG
AGGAAAGTGGAATTCCATGTGTAGCGGTGAAATGCGTAGAGATATGGAGGAACACCAGTGGCGAAGGCGA
CTTTCTGGTCTGTAACTGACACTGAGGCGCGAAAGCGTGGGGAGCAAACAGGATTAGATACCCTGGTAGT
CCACGCCGTAAACGATGAGTGCTAAGTGTTAGAGGGTTTCCGCCCTTTAGTGCTGAAGTTAACGCATTAA
GCACTCCGCCTGGGGAGTACGGCCGCAAGGCTGAAACTCAAAGGAATTGACGGGGGCCCGCACAAGCGGT
GGAGCATGTGGTTTAATTCGAAGCAACGCGAAGAACCTTACCAGGTCTTGACATCCTCTGACAACCCTAG
AGATAGGGCTTCTCCTTCGGGAGCAGAGTGACAGGTGGTGCATGGTTGTCGTCAGCTCGTGTCGTGAGAT
GTTGGGTTAAGTCCCGCAACGAGCGCAACCCTTGATCTTAGTTGCCATCATTTAGTTGGGCACTCTAAGG
TGACTGCCGGTGACAAACCGGAGGAAGGTGGGGATGACGTCAAATCATCATGCCCCTTATGACCTGGGCT
ACACACGTGCTACAATGGACGGTACAAAGAGCTGCAAGACCGCGAGGTGGAGCTAATCTCATAAAACCGT
TCTCAGTTCGGATTGTAGGCTGCAACTCGCCTACATGAAGCTGGAATCGCTAGTAATCGCGGATCAGCAT
GCCGCGGTGAATACGTTCCCGGGCCTTGTACACACCGCCCGTCACACCACGAGAGTTTGTAACACCCGAA
GTCGGTGGGGTAACCTTTTGGAGCCAGCCGCATAAGGTGACAGAGGGGG
```

### 3. <u>Alignment</u>

Copy and paste all the sequences into MEGA for our alignment.

![MEGA.png](https://tony-7752.github.io/Research-method-2026-Tony/images/MEGA.png)
**Figure 3:** MEGA

Use the alignment function to align all the sequences you have pasted in MEGA.

### 3. <u>Pick primers using primer3</u>

Go online to primer3, copy and paste a sequence of DNA and select pick primers for the program find the primers you need.

![primer3.png](https://tony-7752.github.io/Research-method-2026-Tony/images/primer3.png)
**Figure 4:** primer3

Select a fitting primer from the options you get:

![primer3-2.png](https://tony-7752.github.io/Research-method-2026-Tony/images/primer3-2.png)
**Figure 5:** primers

### 5. <u>Primer BLAST</u>

We will use primer BLAST to check if our designed primers fit the target organism. Go to Primer-BLAST and copy and paste your primers in the appropriate locations.

![primerb.png](https://tony-7752.github.io/Research-method-2026-Tony/images/primerb.png)
**Figure 6:** Paste primers that you selected

Choose the correct database and organism for the search.

![primerb2.png](https://tony-7752.github.io/Research-method-2026-Tony/images/primerb2.png)
**Figure 7:** Select parameters accordingly

Click Get Primers and wait for results.

![primer-result2.png](https://tony-7752.github.io/Research-method-2026-Tony/images/primer-result2.png)
**Figure 8:** BLAST results

Now we can see that indeed our primers are present in the database for our organism and will be viable for our experiment.


### 6. <u>Phylogenetic tree</u>

With MEGA we can also build a phylogenetic tree. Save the alignment we previously did, and in the main MEGA interface select file and open the alignment. You should have something like this now:

![MEGA2.png](https://tony-7752.github.io/Research-method-2026-Tony/images/MEGA2.png)
**Figure 9:** MEGA interface after importing an alignment

Now select phylogeny and select the method you wish to build a tree with. 

This is our result for this dataset:

![tree-result.png](https://tony-7752.github.io/Research-method-2026-Tony/images/tree-result.png)
**Figure 10:** Neighbor-joining tree, substitution model kimura2 with bootstrap

Here we can see the relation of the selected organism to each other based on the 16S sequences we used. We can differentiate between them thanks to variable regions in a relatively conserved gene.

Here we see that Desulfovibrio marinus and Desulfovibrio desulfuricans are the most closely related bacteria. Desulfovibrio vulgaris has split from the other bacteria the earliest as can be seen by the outgroup it is in.

