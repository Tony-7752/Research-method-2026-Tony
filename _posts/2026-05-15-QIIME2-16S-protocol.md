---
layout: post
title: QIIME2 16S protocol
date: '2026-05-15'
categories: Protocols
tags: QIIME2 pipeline
---
### 17/05/2026
## QIIME2 16S pipeline
### *Protocol for applying a 16S rRNA microbiome analysis using QIIME2*

This protocol outlines a basic 16S microbiome pipeline I am doing for my research.

### 1. <u>*Importing files into QIIME2*</u>

To import files into a format QIIME2 can read, we need to use the import function:
```bash
qiime tools import --type 'SampleData[PairedEndSequencesWithQuality]'  --input-path manifest.csv  --output-path demux-paired-end.qza  --input-format PairedEndFastqManifestPhred33V2
```
This command takes your manifest file (which contains the locations of all the paired-end FASTQ files) and transforms them into a QIIME2 artifact called **demux-paired-end.qza**. 

```bash
qiime demux summarize --i-data demux-paired-end.qza --o-visualization demux.qzv
```
This will produce a visualization of the raw data.

### 2. <u>*Removing PCR primers*</u>

Before analysing our data we want to make sure no PCR primers are left on our reads. They will waste computational power and give us no additional information. The following command removes the primers coresponding to a sequence that we give it. In this case the primers are 515F/806R 16S primers.

```bash
qiime cutadapt trim-paired \
   --i-demultiplexed-sequences demux-paired-end.qza \
   --p-cores 48 \
   --p-front-f GTGYCAGCMGCCGCGGTAA \
   --p-front-r GGACTACNVGGGTWTCTAAT \
   --p-discard-untrimmed \
   --p-no-indels \
   --o-trimmed-sequences demux-paired-end-trimmed.qza
```
This command will visualize the trimmed data:
```bash
qiime demux summarize --i-data demux-paired-end-trimmed.qza --o-visualization demux-trimmed.qzv
```

### 3. <u>*De-noising with DADA2*</u>

DADA2 is a pipeline itself, used to truncate, correct sequencing machine errors and assemble paired end sequences. This next part is quite computationally demanding and will take some time to run.

 Notice the truncation length is suited for 16S. for 18S the length should be adjusted.

```bash
qiime dada2 denoise-paired --i-demultiplexed-seqs demux-paired-end-trimmed.qza --p-trunc-len-f 230 --p-trunc-len-r 220 --o-representative-sequences rep-seqs.qza --o-table table.qza --o-denoising-stats stats-dada2.qza --p-n-threads 48 --verbose

qiime metadata tabulate --m-input-file stats-dada2.qza --o-visualization stats-dada2.qzv

qiime feature-table tabulate-seqs --i-data rep-seqs.qza --o-visualization rep-seqs.qzv

qiime feature-table summarize --i-table table.qza --o-visualization table.qzv --m-sample-metadata-file metadata.tsv
```
To check our results after the algorithm has finished, we can use QIIME2 view. Simply drag the .qzv file into the QIIME2 view website and look at the results:

