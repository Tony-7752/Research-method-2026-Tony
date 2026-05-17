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

### 1. <u>Importing files into QIIME2</u>

To import files into a format QIIME2 can read, we need to use the import function:
```bash
qiime tools import \
  --type 'SampleData[PairedEndSequencesWithQuality]' \
  --input-path manifest.csv \
  --output-path demux-paired-end.qza \
  --input-format PairedEndFastqManifestPhred33V2
```
This command takes your manifest file (which contains the locations of all the paired-end FASTQ files) and transforms them into a QIIME2 artifact called `demux-paired-end.qza`. 

```bash
qiime demux summarize --i-data demux-paired-end.qza --o-visualization demux.qzv
```
This will produce a visualization of the raw data.

### 2. <u>Removing PCR primers</u>

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

### 3. <u>De-noising with DADA2</u>

DADA2 is a pipeline itself, used to truncate, correct sequencing machine errors and assemble paired end sequences. This next part is quite computationally demanding and will take some time to run.

 Notice the truncation length is suited for 16S. for 18S the length should be adjusted.

```bash
qiime dada2 denoise-paired --i-demultiplexed-seqs demux-paired-end-trimmed.qza --p-trunc-len-f 230 --p-trunc-len-r 220 --o-representative-sequences rep-seqs.qza --o-table table.qza --o-denoising-stats stats-dada2.qza --p-n-threads 48 --verbose

qiime metadata tabulate --m-input-file stats-dada2.qza --o-visualization stats-dada2.qzv

qiime feature-table tabulate-seqs --i-data rep-seqs.qza --o-visualization rep-seqs.qzv

qiime feature-table summarize --i-table table.qza --o-visualization table.qzv --m-sample-metadata-file metadata.tsv
```
To check our results after the algorithm has finished, we can use QIIME2 view. Simply drag the `.qzv` file into the QIIME2 view website and look at the results:

![qiime2-view-example.png](https://tony-7752.github.io/Research-method-2026-Tony/images/qiime2-view-example.png)

Figure 1: QIIME2 view of DADA2 information.

### Tracking Read Retention
After running DADA2, it is critical to verify how many sequences survived the filtering and merging steps. The table below represents the exact read retention statistics for our samples:

| Sample ID | Input Reads | Filtered Reads | Denoised Reads | Merged Reads | Non-Chimeric |
|---|---|---|---|---|---|
| Bio_1 | 35520 | 33489 | 31095 | 29372 | 29049 |
| Bio_10 | 28726 | 26750 | 25541 | 24526 | 24266 |
| Bio_2 | 37198 | 35354 | 34511 | 33423 | 32687 |
| Bio_3 | 33274 | 31485 | 30460 | 29562 | 29241 |
| Bio_4 | 39833 | 37512 | 35430 | 33424 | 32295 |


### 4. <u>Clustering OTU's</u>

What we get from DADA2 is alot of ASV's (amplicon sequence variants). These are sequences that can vary as little as in one nucleotide, causing the algorithm to bin them in different variants. This can be *too accurate* sometimes, and we will want some degrees of freedom for our analysis. This is why we will use OTU's (Operational taxonomic units). This will allow some difference between sequences to our degree of choise.

```bash
qiime vsearch cluster-features-de-novo --i-table table.qza --i-sequences rep-seqs.qza --p-perc-identity 0.99 --o-clustered-table table_99.qza --o-clustered-sequences rep-seqs-99.qza
```


### 5. <u>Clasification</u>

Now after we have OTU's, we need to classify them into who they actually are. In this example we will use only one clasifier (sklearn) and database (SILVA).

```bash
qiime feature-classifier classify-sklearn --i-classifier /media/bioinf/Data/DATA_for_QIIME2_2021.4/silva-138-99-515-806-nb-classifier.qza --i-reads rep-seqs-99.qza --o-classification taxonomy.qza --p-n-jobs -48 --verbose
```

### 6. <u>Export out of QIIME2</u>

Now once we did all we needed in QIIME2, we want to export the data out for more downstream analysis. 


QIIME 2 stores data in specialized `.qza` artifacts. To perform statistical analysis and generate ecological graphs in software like R or Python, we must extract the raw data tables from these artifacts. 

Since we focused solely on the SILVA database for 16S classification, we need to export our sequence counts and our taxonomic assignments. 

**Exporting the Feature Table and Taxonomy:**
```bash
# 1. Export the feature table (sequence counts per sample)
qiime tools export \
  --input-path table_99.qza \
  --output-path ./phyloseq

# 2. Convert the exported BIOM table into a human-readable TSV format
biom convert -i phyloseq/feature-table.biom -o phyloseq/otu_table.txt --to-tsv

# 3. Export the SILVA taxonomy assignments
qiime tools export \
  --input-path taxonomy.qza \
  --output-path ./phyloseq
  ```
  
### References and useful links

  * [QIIME 2 Amplicon Analysis Documentation](https://amplicon-docs.qiime2.org/) - The main platform for this analysis.
* [DADA2 Pipeline Official Documentation](https://benjjneb.github.io/dada2/) - The de-noising pipeline (a QIIME2 plug-in).
* [DNeasy PowerSoil Pro Kit Official Protocol](https://www.qiagen.com/en-US/resources/download/KitHandbook/hb-2495-006-hb-dny-powersoil-pro-0623-ww) - The kit used to extract the DNA that was used in this analysis.