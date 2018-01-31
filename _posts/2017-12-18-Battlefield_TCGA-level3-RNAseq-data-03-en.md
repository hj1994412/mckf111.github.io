---
layout: post
title: "Fundamental Analysis of TCGA RNA-seq Data-03"
author: Wenhu
date: 2017-12-18
mathjax: yes
tags: TCGA RNA-seq
categories: Battlefield_TCGA
---

* content
{:toc}

> This is the last part of the overall analysis pipeline, mainly documenting how to use DESeq2 package for fundamental DE analysis. **Repost by indicating the source please!**

## Principle

Actually, the principles of most DE analysis tools are almost the same. If you want the tools to tell you which genes are differentially expressed and if the differences are significant or not

First of all, the expression of each gene is shown as numbers via sequencing and the tools needs this numbers to tell you the differences. Usually, we call this number matrix as **expression matrix**.




Secondly, there will be no difference if you don't make comparision, that means, you need to have more than 2 groups (such as control group and experimental group), so that the tools can evaluate the difference between them. What's more, following the basic experimental design protocol, there should be 3-6 replicates for each group, the tools also need this information to evaluate the variance. Usually, we call this grouping information as **grouping matrix**.

Lastly, you need to use functions in specific R package for generating **differential expression matrix**, including log fold change (LFC) and adjusted p-value (padj) values.

> The authors of most R package will provide users corresponding vignette--brief and clear illustration of the package, so as to let users grasp important functions to handle personal problems fastly, like "Quick Start". One could invoke it via `vignette("package_name")` command or just go ahead to the package website for it.

> *Actually, some of the vignettes are neither brief nor clear.*

## Creat grouping matrix

Now we already have `cts_BLCA` as expression matrix, we would create the grouping matrix: `coldata_BLCA`.


```r
library(DESeq2)
setwd("F:\\biostar\\test\\stddata__2016_07_15\\BLCA\\20160715\\gdac.broadinstitute.org_BLCA.mRNAseq_Preprocess.Level_3.2016071500.0.0\\rnaseq_test")
getwd()
```

```
## [1] "F:/biostar/test/stddata__2016_07_15/BLCA/20160715/gdac.broadinstitute.org_BLCA.mRNAseq_Preprocess.Level_3.2016071500.0.0/rnaseq_test"
```

```r
load(".RData")
```



```r
coldata_BLCA <- data.frame(c(rep("tumor_matched_normal", 19), rep("tumor", 19))) # sample volume is 19
row.names(coldata_BLCA) <- colnames(cts_BLCA)[-1] # set the row names of grouping matrix with the column names of expression matrix
colnames(coldata_BLCA) <- "condition" # name the one and only column 'condition'
head(coldata_BLCA)
```

```
##                            condition
## TCGA.BL.A13J.11 tumor_matched_normal
## TCGA.BT.A20N.11 tumor_matched_normal
## TCGA.BT.A20Q.11 tumor_matched_normal
## TCGA.BT.A20R.11 tumor_matched_normal
## TCGA.BT.A20U.11 tumor_matched_normal
## TCGA.BT.A20W.11 tumor_matched_normal
```


> The truth of grouping matrix is, **to declare the group origin of each sample**, so the easiest way is setting sample names as row names, and the one and only column contains the group information for each sample.


> **！！！**: You must ensure that the columns of the expression matrix and the rows of the grouping matrix are **in the same order**!


## Processing of Expression Matrix


> Set the first column 'geneID' as row names, avoiding the geneID as values in the matrix


```r
rownames(cts_BLCA) <- cts_BLCA$geneID
cts_BLCA <- cts_BLCA[, -1]
cts_BLCA[1:6, 1:3]
```

```
##             TCGA.BL.A13J.11 TCGA.BT.A20N.11 TCGA.BT.A20Q.11
## ?|100130426            0.00            0.00            0.00
## ?|100133144            3.46            4.45            6.18
## ?|100134869           11.54            6.55           11.82
## ?|10357              159.30          118.77          168.47
## ?|10431              840.00          844.00          769.00
## ?|136542               0.00            0.00            0.00
```


> round the expression values for DESeq2 analysis


```r
cts_BLCA <- round(cts_BLCA)
cts_BLCA[2, 1]
```

```
## [1] 3
```


## Create DESeqDataSet Object

> This is the most important step, **how to acquire DESeqDataSet using expression and grouping matrix**, you only need to understand 3 parameters, `countData = ` - expression matrix; `colData = ` - grouping matrix; `design = ~` - the name of the column containing grouping information, here, we only have one column - `condition`.


```r
dds_BLCA <- DESeqDataSetFromMatrix(countData = cts_BLCA, colData = coldata_BLCA, design = ~condition)
```

```
## converting counts to integer mode
```

```r
dds_BLCA
```

```
## class: DESeqDataSet 
## dim: 20531 38 
## metadata(1): version
## assays(1): counts
## rownames(20531): ?|100130426 ?|100133144 ... psiTPTE22|387590
##   tAKR|389932
## rowData names(0):
## colnames(38): TCGA.BL.A13J.11 TCGA.BT.A20N.11 ... TCGA.K4.A54R.01
##   TCGA.K4.A5RI.01
## colData names(1): condition
```

```r
# reset the level of `condition` (factor type), to put `tumor_matched_normal` as first level. Normally we shall put control group in the first level to facilitate the following analysis by DESeq2.
levels(dds_BLCA$condition)
```

```
## [1] "tumor"                "tumor_matched_normal"
```

```r
dds_BLCA$condition <- relevel(dds_BLCA$condition, ref = "tumor_matched_normal")
levels(dds_BLCA$condition)
```

```
## [1] "tumor_matched_normal" "tumor"
```

## DESeq() for DE analysis, results() for integrating DE results


```r
dds_BLCA <- DESeq(dds_BLCA) # core function
```

```
## estimating size factors
```

```
## estimating dispersions
```

```
## gene-wise dispersion estimates
```

```
## mean-dispersion relationship
```

```
## final dispersion estimates
```

```
## fitting model and testing
```

```
## -- replacing outliers and refitting for 2033 genes
## -- DESeq argument 'minReplicatesForReplace' = 7 
## -- original counts are preserved in counts(dds)
```

```
## estimating dispersions
```

```
## fitting model and testing
```


```r
results_BLCA <- results(dds_BLCA, alpha = 0.05) # set FDR cutoff as 0.05(5 %)
results_BLCA
```

```
## log2 fold change (MLE): condition tumor vs tumor matched normal 
## Wald test p-value: condition tumor vs tumor matched normal 
## DataFrame with 20531 rows and 6 columns
##                      baseMean log2FoldChange     lfcSE        stat
##                     <numeric>      <numeric> <numeric>   <numeric>
## ?|100130426      2.269245e-02     -0.1493150 2.9603153 -0.05043889
## ?|100133144      3.048973e+01      0.4909953 0.4470821  1.09822187
## ?|100134869      2.628814e+01      0.4721437 0.3298126  1.43155131
## ?|10357          3.128778e+02      0.4785988 0.1741214  2.74864904
## ?|10431          1.975495e+03      0.3593770 0.1892440  1.89901384
## ...                       ...            ...       ...         ...
## ZYX|7791         1.343512e+04     -1.2140793 0.2524407  -4.8093638
## ZZEF1|23140      2.365497e+03     -0.6871970 0.2016152  -3.4084583
## ZZZ3|26009       1.346220e+03      0.2787894 0.1748336   1.5945984
## psiTPTE22|387590 1.539298e+02     -1.0541917 0.5103782  -2.0655108
## tAKR|389932      1.637556e-01     -1.1790186 2.9596947  -0.3983582
##                        pvalue         padj
##                     <numeric>    <numeric>
## ?|100130426       0.959772648           NA
## ?|100133144       0.272107617   0.40996871
## ?|100134869       0.152272273   0.26200503
## ?|10357           0.005984142   0.01894278
## ?|10431           0.057562656   0.12141683
## ...                       ...          ...
## ZYX|7791         1.514114e-06 1.516851e-05
## ZZEF1|23140      6.533108e-04 2.867382e-03
## ZZZ3|26009       1.108021e-01 2.042393e-01
## psiTPTE22|387590 3.887470e-02 8.871335e-02
## tAKR|389932      6.903662e-01 8.043282e-01
```

## Fundamental Analysis of Results

> **counts plot**: visualize the counts of genes have the highest and lowest differences


```r
topGene <- rownames(results_BLCA)[which.min(results_BLCA$padj)] # find out the gene which has the lowest padj value
plotCounts(dds_BLCA, gene = topGene, intgroup = c("condition"))
```

<img src="http://res.cloudinary.com/dgnsud9ue/image/upload/v1513419132/unnamed-chunk-8-1_lteem4.png" alt="万里长城别挡我">


```r
bottomGene <- rownames(results_BLCA)[which.max(results_BLCA$padj)] # find out the gene which has the highest padj value
plotCounts(dds_BLCA, gene = bottomGene, intgroup = c("condition"))
```

<img src="http://res.cloudinary.com/dgnsud9ue/image/upload/v1513419132/unnamed-chunk-8-2_anwono.png" alt="万里长城别挡我">


> **order the results**


```r
# import the first 100 genes which have the lowest padj values
results_BLCA_ordered <- results_BLCA[order(results_BLCA$padj), ] # order
results_ordered_dataframe <- as.data.frame(results_BLCA_ordered)[1:100, ]
write.csv(results_ordered_dataframe, file = "results.csv")

```

> Afterwards, you could carry on all kinds of analysis based on `results_BLCA`, such as GO analysis, clustering heatmap, etc. I would not illustrate them in this series of posts.

> __*Actually, I am still learning them...*__


> Please subscribe [RSS](http://bioinfostar.com/feed.xml) if you wanna receive my newest post!
