---
layout: post
title: "TCGA大作战——初步分析RNA-seq数据03"
author: Wenhu
date: 2017-12-16
mathjax: yes
tags: TCGA R DESeq2 firehose RNA-seq
categories: Battlefield_TCGA(中文)
---

* content
{:toc}

> 本篇为第三部分，主要记录使用DESeq2包做差异分析。

## 基本原则

其实，大多数DE分析软件的思路都是差不多的，你要想让软件告诉你哪些基因表达有差异，差异显不显著，那么

首先，通过测序，每个实验组的基因表达都会以数值来计量，软件得知道你的表达数值才能计算差异(根据软件的不同，可以是原始数值(raw_counts)，也可以是标准化后的数据)，这个信息俗称**表达矩阵**




其次，没有比较，就没有差异，你必然要有两个或两个以上的实验组(group，比如处理组和对照组)，得到的表达量才有比较的可能，同时，按照实验设计的一般原则，每组需要有3到6个重复(replicate)，这些信息你也得告诉软件，否则，它拿到一堆表达量，却不知道拿谁和谁比，这就尴尬了，那么这个信息俗称**分组矩阵**

最后，就是根据软件的不同，利用不同的函数处理上述两个矩阵，得到**差异信息矩阵**，其中包括差异倍数(log fold change)，差异显著程度(adjusted p-value)，这要彻底理解，还得需要修炼统计学功底，我在努力中。

> 对于大多数R包，作者都会编写一个相应的vignette/vin'jet/, 原意是简短而清晰的描述，在这里，可以理解为类似"Quick Start"的资料，可以让读者快速学会使用该R包中的重要函数来处理问题，一般通过`vignette("package_name")`函数来调用，或者直接去该R包的介绍页面寻找链接。

> *实际上，有些包的vignette写得既不简短也不清晰。*

## 构造分组矩阵

紧接着上一讲的内容，我们目前得到了`cts_BLCA`矩阵，这个就是**表达矩阵**，那么，接下来我们继续构建**分组矩阵**: `coldata_BLCA`。


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
coldata_BLCA <- data.frame(c(rep("tumor_matched_normal", 19), rep("tumor", 19))) # 因为样本数较小，此处我就直接写了数字
row.names(coldata_BLCA) <- colnames(cts_BLCA)[-1] # 将表达矩阵的列名（除去第一列geneID），即样本名，作为分组矩阵的行名
colnames(coldata_BLCA) <- "condition" # 命名唯一的一列为condition，方便后续处理
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


> 分组矩阵的实质就是，**要声明每个样本是属于哪种实验组的**，比如TCGA barcode中，-11样本属于正常组，-01样本属于肿瘤组，所以，最简单的分组矩阵就是，样本名作为行名，唯一的一列里面包含相对应的每个样本的组别信息。


> **着重强调**：表达矩阵的列名中样本的排列顺序必须和分组矩阵的行名中样本顺序完全一致！即如果表达矩阵中是样本A-B-C，则分组矩阵也必须是A-B-C，其他顺序都不行，即便每个样本的分组信息没有错误。


## 表达矩阵的进一步处理


> 将表达矩阵的第一列geneID改成行名，这样矩阵中所有的数值才是表达值


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


> 对表达矩阵中的值取整数，这样才可用于DESeq2的分析，当然，直接导入也可以，软件自己会做取整处理


```r
cts_BLCA <- round(cts_BLCA)
cts_BLCA[2, 1]
```

```
## [1] 3
```


## 创建差异表达数据集(DESeqDataSet object)

> 关键一步，通过表达矩阵和分组矩阵得到差异表达数据集，关键记住三个参数，`countData`-表达矩阵；`colData`-分组矩阵；`design`-分组矩阵中包含组别信息的列名。因为我们此处用的是最简单的单列分组矩阵，所以第三个参数只能是`condition`。


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
# 重新设置`condition`（factor类型）的level，使得`tumor_matched_normal`为first level, 相当于实验中的对照组要放在第一位，方便后续DESeq函数处理！
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

## 差异分析DESeq及差异信息矩阵results


```r
dds_BLCA <- DESeq(dds_BLCA) # 核心函数DESeq()
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
# 显示结果——差异信息矩阵results
results_BLCA <- results(dds_BLCA, alpha = 0.05) # 设置FDR cutoff值为0.05(5 %)
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

## 结果初探

> **counts plot**: 看看高差异和低差异基因的counts可视化情况


```r
topGene <- rownames(results_BLCA)[which.min(results_BLCA$padj)] # 找出padj值最低的基因(padj：adjusted p-value)
plotCounts(dds_BLCA, gene = topGene, intgroup = c("condition")) # DESeq2包自带的作图函数，第三个参数为interested group(感兴趣的组别信息，即分组矩阵列名)
```

<img src="http://res.cloudinary.com/dgnsud9ue/image/upload/v1513419132/unnamed-chunk-8-1_lteem4.png" alt="万里长城别挡我">


```r
bottomGene <- rownames(results_BLCA)[which.max(results_BLCA$padj)] # 找出padj值最高的基因
plotCounts(dds_BLCA, gene = bottomGene, intgroup = c("condition"))
```

<img src="http://res.cloudinary.com/dgnsud9ue/image/upload/v1513419132/unnamed-chunk-8-2_anwono.png" alt="万里长城别挡我">


> **结果排序**


```r
# 导出前100个差异较大的基因(padj值较小)
results_BLCA_ordered <- results_BLCA[order(results_BLCA$padj), ] # 按padj大小排序
results_ordered_dataframe <- as.data.frame(results_BLCA_ordered)[1:100, ]
write.csv(results_ordered_dataframe, file = "results.csv")

# 此时，在当前工作目录下会产生一个.csv文件，可用Excel打开，里面存放了前100个差异较大基因的数据
```

> 后续，大家可以继续利用`results_BLCA`做各种功能分析和可视化，譬如GO分析，聚类热图等等，在此暂时不展开讲。

> __*其实，是我还不太会。。。*__
