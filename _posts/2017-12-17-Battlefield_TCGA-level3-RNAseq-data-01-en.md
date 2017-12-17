---
layout: post
title: "Fundamental Analysis of TCGA RNA-seq Data-01"
date: 2017-12-17
categories: Battlefield_TCGA
tags: TCGA R DESeq2 firehose RNA-seq
author: Wenhu
mathjax: true
---

* content
{:toc}

> This is the first part of the overall analysis pipeline, mainly documenting introductions of important resources and the data downloading methods.

## Resources

* [TCGA (The Cancer Genome Atlas)](https://cancergenome.nih.gov/)：**Human** cancer database, on one hand, there is a huge number of molecular data (including DNA, RNA and protein levels) based on a series of collections of cancer tissue samples, tumor_matched_normal samples and a few normal tissue samples. On the other hand, it also contains multiple clinical data (such as the TNM grading of tumor, patient survival time, patients' age, sex, race and so on). Until now, it has documented nearly 2.5PB multi-level data volume of more than 10,000 patients, stretching across over 30 tumor types. **From 2016, the TCGA database has migrated to [GDC (Genomic Data Commons)](https://gdc.cancer.gov/), it is said TCGA will come to close in 2017, come on, there is only 2 weeks left!**




    + What is tumor_matched_normal samples？ ["Tumor, Matched Normal" Vs. "Normal, Matched Tumor"](https://www.biostars.org/p/86929/)
    
    + You will find important information of the data sources, tiers and pipelines in TCGA at: [TCGA WIKI](https://wiki.nci.nih.gov/display/TCGA/TCGA+Wiki+Home)
    
    + Multidimensionality is a key feature of life. There are several big levels of life, from biomolecule to organism, and it could be divided into substantial small sub-levels. It would be a little shortsighted for anyone who recognise TCGA as one ulti-database. The reason why I am talking about this is to introduce the origin of **GDC**, it is the new home for TCGA, but also embracing several big databases, such as [TARGET (Therapeutically Applicable Research To Generate Effective Treatments)](https://ocg.cancer.gov/programs/target), [CGCI (Cancer Genome Characterization Initiative)](https://ocg.cancer.gov/programs/cgci) and so on. This might be the trend - **integration of classified databases**, so that researchers could acquire multi-level information in one site, for better biology discovery and precision medicine. *There is no need to worry about losing my job, so many data to be analysed.*
    
* [GDAC (Genome Data Analysis Centers)](https://cancergenome.nih.gov/abouttcga/overview/howitworks/dataanalysiscenters): It is an important consortium belonged to TCGA Research Network, the famous members include [Firehose](http://firebrowse.org/) from Broad Institute and [cbioportal](http://www.cbioportal.org/) from Memorial Sloan-Kettering Cancer Center. In a nutshell, they merge, manipulate and visualize the TCGA raw data, via their own pipelines, to reduce the burden of data processing and facilitate the downstream analysis for researchers.

* [DESeq2](https://bioconductor.org/packages/release/bioc/html/DESeq2.html): A famous gene Defferential Expression (DE) finding and visulization R package, *actually, I was not sure which DE package to use in the first place, accidentally, I found the authors of this package living in the same city with me, maybe it's a good idea to keep the problem solver in sight. Bingo!*

## About Data

Honestly, the overall RNA-seq analysis goes from the first-hand fastq sequencing files, through aligning, annotating, counting, DE analysis and functional prediction, at last we could acquire useful information for interpreting some experimental phenomena or directing experiment designs. However, the sequencing files of TCGA data belong to tier1, individuals don't have access, what's more, the files are so big that it is hard for personal computers to analyse, even to store. So, I start here with tier3 data from Firehose, focusing on doing DE analysis.

About how to download TCGA data, there is a [compilation post here](http://www.biotrainee.com:8080/forum.php?mod=viewthread&tid=1696#lastpost), sorry, there is only Chinese version for now, but I will try to briefly introduce each method when I use it for the first time in my posts.

* Why do I use data from Firehose? Because Firehose has merged all the data coming from different samples into one single file, while one file for each sample in GDC. I am lazy, O(∩_∩)O~！

* There is a very simple data downloading tool, [firehose_get](https://confluence.broadinstitute.org/display/GDAC/Download), it contains installation guide and examples as well. It is required to have linux in your computer, at least one [bash on windows](https://docs.microsoft.com/en-us/windows/wsl/about).

* **P.S. I strongly recommend the new fish like me to buy [The Biostar Handbook](https://www.biostarhandbook.com/), only around 30 euros, but you could get much more than that, including how to set up your computer and install dozens of bioinformatics tools step by step. Furthermore, the book is easy reading and the author is very funny.**
