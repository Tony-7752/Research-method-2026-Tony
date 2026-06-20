---
layout: post
title: qPCR Gene Selection
date: '2026-06-17'
categories: Protocols
tags: qPCR reference genes
---
### 17/06/2026
## qPCR gene selection
### *Protocol for selecting genes for qPCR analysis*

This protocol outlines the selection of 2 target genes and one reference gene for qPCR analysis.

### 1. <u>What is qPCR analysis?</u>
A method to measure DNA or RNA amount in a sample with PCR amplification. It is based on a regular PCR reaction, with the only difference being the addition of a fluorescent dye that, as the reaction progresses, increases the sample signal thus indicating the amount of DNA in the sample. With a refernce gene of known amount of expression and by comparing number of cycles, we can compare gene abundances.

![qpcr.png](https://tony-7752.github.io/Research-method-2026-Tony/images/qpcr.png)

Figure 1: qPCR cycle

### 2. <u>Selecting a reference gene</u>
For the reference gene, also known as housekeeping gene, we want to select a gene that is not altered by the treatment we will subject the target genes to. So for every treatment we do we might  need to select different references. We will use this gene to normalise our results and to see how has the expression changed in our target genes.

For this protocol, we will be looking into *Prochlorococcus* and how do some of its genes change expression in a phosphorus limited environment.

![Prochlorococcus_marinus_2.jpeg](https://tony-7752.github.io/Research-method-2026-Tony/images/Prochlorococcus_marinus_2.jpeg)

Figure 2: *Prochlorococcus marinus*, Ohio state university

We will want to select a gene not affected by the phosphorus limited environment, so preferably a gene involved in fundamental cell functions, one that it cannot survive without and will not be down or up regulated during the treatment.

### Our gene of choise is: *secA*

This gene has a central role in coupling the hydrolysis of ATP to the transfer of proteins into and across the cell membrane. It also probably participates in protein translocation into and across both the cytoplasmic and thylakoid membranes in cyanobacterial cells.

This is a central gene to the normal function of cyanobacterial cells, which we expect will not change its expression rate when subject to P-limited conditions.

### 3. <u>Selecting target genes</u>
The treatment we have decided to subject the cells to is P-limitation. We will select target genes who we expect to see a change in their expression.

### The first target gene: *pstS*

This gene encodes a high-affinity phosphate-binding protein located at the cell membrane. It is basically a phosphorus "sensor", which we expect to be *upregulated* during phosphorus limited conditions, as the cell desperately struggles to find more phosphorus.

### The second target gene: *sqdB*

SqdB encodes UDP-sulfoquinovose synthase, possibly enabling marine picocyanobacteria to minimize their phosphorus requirements by substitution of phospholipids with sulphur-containing glycerolipids. Probably as a form of adaptation to phosphorus limited water, some organisms are capable of substituting phosphorus with sulfur in the cell membrane.

This gene is likely to be upregulated in a phosphorus limited environment, as the organism will turn to use more sulfur in the cell membrane, rather than the depleted phosphorus.

### 4. <u>Now what?</u>

Let's say we have now all the genes we need and we have decided on what treatment to do. We run the qPCR amalysis, and we now need to analyse the results we got.

Analysis of qPCR data is done by using the number of cycles it takes for every gene to pass a certain fluorescence threshold (**Ct**). This threshold indicates the amount of cycles are enough for the genes to be amplified to a certain amount above the background level.

![Qpcr-cycling.png](https://tony-7752.github.io/Research-method-2026-Tony/images/Qpcr-cycling.png)

Figure 3: Fluorescence levels crossing the background threshold in a qPCR run

If Ct for a gene of interest is the same as the refernce gene we have selected, with and without the treatment, we can say it is not affected by the treatment. But what if there is a difference in the normal amount of expression of the gene of interes? 

### That's where cycle normalization comes in to play.

![qpcr2.png](https://tony-7752.github.io/Research-method-2026-Tony/images/qpcr2.png)

Figure 4: qPCR run Ct data of several genes. Tubulin is the reference gene.

At any given time in the cell, many genes are expressed, all at a different pace. To account for that when we test gene expression in response to a treatment, we need to normalize gene expression to some baseline so we can see the actual difference in expression caused by the treatment.

### 5. <u>Δ𝐶𝑡, ΔΔ𝐶𝑡 & Fold change</u>

To normalize the Ct values and get data we can work and compare genes with, we will use several values:

* ### Δ𝐶𝑡 = 𝐶𝑡target−𝐶𝑡reference
* ### ΔΔ𝐶𝑡 = Δ𝐶𝑡experimental-Δ𝐶𝑡control
* ### Fold change = 2<sup>−ΔΔCt</sup>

**Δ𝐶𝑡** is the difference between the amount of cycles it took the target gene to achieve fluorescence threshold compared to the reference gene. If this value is **positive**, then the target gene has been **down-regulated compared to the reference gene**, baceuse it took more cycles to reach fluorescence threshold.

**ΔΔ𝐶𝑡** is the difference between the **Δ𝐶𝑡** values of the control (no treatment) and the experimental conditions. If this value is **positive** then that means the **experimental conditions have caused the gene to be down-regulated**.

**Fold change** - Because during PCR, DNA is doubled every cycle, this value indicates the change in expression of the genes ("This gene's expression was 4 times higher" = 2<sup>-2</sup>=0.25, two cycles less). This value is what we want to look at to see how has the treatment affected gene expression.

![qpcr3.png](https://tony-7752.github.io/Research-method-2026-Tony/images/qpcr3.png)

Figure 5: Δ𝐶𝑡, ΔΔ𝐶𝑡 & Fold change of two genes

Have a look at the *ascs* gene. **Δ𝐶𝑡** value is **positive**,and the control is higher than treatment. This means that **Ct of *ascs* was higer than the reference gene, and this gene is expressed less than the reference gene**. For the same gene, the **ΔΔ𝐶𝑡** value was negative, which means the **experimental conditions have caused the gene to be expressed more**.

To summarize, here is a plot of all the genes in the class exercise and their fold change:

![fold-change.png](https://tony-7752.github.io/Research-method-2026-Tony/images/fold-change.png)

Figure 5: Fold change of genes in class exercise

The red line indicates the baseline of 1, meaning 2<sup>-0</sup>=1, indicating that the gene has the same expression level both in control or treatment conditions. Higher than 1 indicates the gene is expressed more after treatment conditions. This plot however, does not indicate expression levels relating to the reference gene, because the data has been normalized (Δ𝐶𝑡).

