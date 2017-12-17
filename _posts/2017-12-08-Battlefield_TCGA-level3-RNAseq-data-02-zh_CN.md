---
layout: post
title: "TCGA大作战——初步分析RNA-seq数据02"
date: 2017-12-08
categories: Battlefield_TCGA(中文)
tags: TCGA R DESeq2 firehose RNA-seq
author: Wenhu
mathjax: true
---

* content
{:toc}

> 本篇为第二部分，主要记录数据下载和整理过程。

## 数据选择

一开始，本来是想探索一下癌组织和正常组织(tumor_matched_normal，简称TN)的基因差异化表达情况(differential expression，简称DE)

为了加快分析速度，所以选了在Firehose上排在第一位的**ACC (Adrenocortical carcinoma，肾上腺皮质癌)**，因为只有区区92个样本，可下载完竟发现，其中并没有TN数据




原来并非所有癌症的RNA-seq数据都包括了TN样本，我对目前GDC中癌症RNA-seq数据做了个简单统计，见下图

本次分析，我选择了排第二，有412个样本的**BLCA (Bladder urothelial carcinoma，膀胱尿路上皮癌)**的RNA-seq数据。

<img src = "https://farm5.staticflickr.com/4728/38023598755_0da2d37dbe_b.jpg" alt = "万里长城别挡我">

> 如何区分癌组织、正常组织和对照组织样本，请参考[TCGA barcode](https://wiki.nci.nih.gov/display/TCGA/TCGA+barcode)的解释，后面筛选数据时会用到。

## 数据下载


利用`firehose_get`下载，下载及安装前一讲已说过，在linux环境中，进入项目运行目录。


> 此处有一点需要注意，**如何让linux的目录在windows系统中也可见**，可以方便在两者中操作，对于像我一样的新手，这点还是很重要的。

> Linux我才刚刚入门，所以用的方法是[Biostar Handbook](https://www.biostarhandbook.com/)中介绍的：

```
# 在任意地方创建一个文件夹，此处以桌面示例(注意将your_user_name换成你自己的用户名)
mkdir -p '/mnt/c/Users/your_user_name/Desktop/unix'

# 将文件夹链接到linux系统中
ln -s  '/mnt/c/Users/your_user_name/Desktop/unix' ~/unix

# 查看
cd
ls -lh

# 能看到这一行表示操作成功，今后，unix文件夹中的任意文件都可以在Windows和Linux中进行操作
lrwxrwxrwx 1 mckf111 mckf111  31 Dec  8 14:10 unix -> /mnt/c/Users/your_user_name/Desktop/unix
```


进入你自设的项目文件夹，然后就可以开始下载我们要的数据了。

> firehose_get 使用方法，只需记住依次四个参数(arg)即可，**参数不区分大小写**，详情可以在命令行直接输入`firehose_get`查看：

> `firehose_get [flags]  RunType  Date  [disease_cohort, ... ]`
1. `[flags]`：一般用`-tasks`，声明你要下载的数据类型，clinical？rnaseq？snp？等等
2. `RunType`：声明数据处理级别，一般只用到两个，stddata代表level3，analyses代表level4
3. `Date`：数据在firehose中的处理日期，一般都用最新的，latest
4. `[disease_cohort, ...]`：声明要下载的癌症类型，诸如acc，blca等等

> 简言之，就是*啥数据，啥级别，啥日子，啥肿瘤*，多写几遍就OK了。

```
firehose_get -tasks rnaseq_pre stddata latest blca

# 下载前会弹出确认信息

# 下载完成后进入最内层目录
cd stddata__2016_07_15/BLCA/20160715/

# 查看文件
ls -lh

# 删除md5文件，aux以及mage-tab文件
rm -f *md5 *aux* *mage-tab*
ls -lh

# 解压
tar zxvf gdac.broadinstitute.org_BLCA.mRNAseq_Preprocess.Level_3.2016071500.0.0.tar.gz
cd gdac.broadinstitute.org_BLCA.mRNAseq_Preprocess.Level_3.2016071500.0.0/
ls -lh

-rwxrwxrwx 1 root root  21M Dec  8 11:50 BLCA.mRNAseq_RPKM.txt
-rwxrwxrwx 1 root root  25M Dec  8 11:50 BLCA.mRNAseq_RPKM_Z_Score.txt
-rwxrwxrwx 1 root root  21M Dec  8 11:50 BLCA.mRNAseq_RPKM_log2.txt
-rwxrwxrwx 1 root root  22M Dec  8 11:50 BLCA.mRNAseq_RPKM_log2_PARADIGM.txt
-rwxrwxrwx 1 root root  21M Dec  8 11:50 BLCA.mRNAseq_median_length_normalized.txt
-rwxrwxrwx 1 root root 5.7M Dec  8 11:50 BLCA.mRNAseq_raw_counts.txt
-rwxrwxrwx 1 root root 154M Dec  8 11:50 BLCA.uncv2.mRNAseq_RSEM_Z_Score.txt
-rwxrwxrwx 1 root root  64M Dec  8 11:50 BLCA.uncv2.mRNAseq_RSEM_all.txt
-rwxrwxrwx 1 root root 121M Dec  8 11:50 BLCA.uncv2.mRNAseq_RSEM_normalized_log2.txt
-rwxrwxrwx 1 root root 125M Dec  8 11:50 BLCA.uncv2.mRNAseq_RSEM_normalized_log2_PARADIGM.txt
-rwxrwxrwx 1 root root 979K Dec  8 11:50 BLCA.uncv2.mRNAseq_geneid_transcriptid_mapping.txt
-rwxrwxrwx 1 root root  34M Dec  8 11:50 BLCA.uncv2.mRNAseq_raw_counts.txt
-rwxrwxrwx 1 root root 151M Dec  8 11:50 BLCA.uncv2.mRNAseq_scaled_estimate.txt
-rwxrwxrwx 1 root root  903 Dec  8 11:50 MANIFEST.txt

# 我们只需要BLCA.uncv2.mRNAseq_raw_counts.txt文件，解释见文末
mkdir rnaseq_test
cp BLCA.uncv2.mRNAseq_raw_counts.txt ./rnaseq_test/
cd rnaseq_test/
ls -lh

-rwxrwxrwx 1 root root 34M Dec  8 14:00 BLCA.uncv2.mRNAseq_raw_counts.txt

```


数据至此就下载完了，接下来的活就要在R中完成了。

我们可以写脚本，然后继续在linux中运行，也可行直接在RStudio中运行，本文采用后者。


```r
setwd("F:\\biostar\\test\\stddata__2016_07_15\\BLCA\\20160715\\gdac.broadinstitute.org_BLCA.mRNAseq_Preprocess.Level_3.2016071500.0.0\\rnaseq_test")
getwd()
```

```
## [1] "F:/biostar/test/stddata__2016_07_15/BLCA/20160715/gdac.broadinstitute.org_BLCA.mRNAseq_Preprocess.Level_3.2016071500.0.0/rnaseq_test"
```

```r
library(DESeq2)
library(readr)
```

```r
# 导入数据
RSEM_raw_counts <- read.table("F:/biostar/test/stddata__2016_07_15/BLCA/20160715/gdac.broadinstitute.org_BLCA.mRNAseq_Preprocess.Level_3.2016071500.0.0/rnaseq_test/BLCA.uncv2.mRNAseq_raw_counts.txt", sep = '\t', header = TRUE)

# 找出正常组织的raw_counts矩阵
x <- colnames(RSEM_raw_counts)
geneID <- RSEM_raw_counts[, 1] # 把第一列geneID单独拿出来
colnames(RSEM_raw_counts)[1] <- "geneID" # 同时给第一列设置个同样的列名
x <- x[-1] # 除去第一列的列名，非样本名
x <- substr(x, 14, 15) # 提取样本名的14到15号字符，其代表了样本类型
x <- as.integer(x)
x <- x %in% (10:19) # 10-19之间代表正常组织样本
y <- colnames(RSEM_raw_counts)[-1]
y <- y[x] # 找到正常组织样本的列名
tumor_matched_normal_counts <- cbind(geneID, subset(RSEM_raw_counts, select = y)) # Bingo

# 查看
colnames(tumor_matched_normal_counts)
```

```
##  [1] "geneID"          "TCGA.BL.A13J.11" "TCGA.BT.A20N.11"
##  [4] "TCGA.BT.A20Q.11" "TCGA.BT.A20R.11" "TCGA.BT.A20U.11"
##  [7] "TCGA.BT.A20W.11" "TCGA.BT.A2LA.11" "TCGA.BT.A2LB.11"
## [10] "TCGA.CU.A0YN.11" "TCGA.CU.A0YR.11" "TCGA.GC.A3BM.11"
## [13] "TCGA.GC.A3WC.11" "TCGA.GC.A6I3.11" "TCGA.GD.A2C5.11"
## [16] "TCGA.GD.A3OP.11" "TCGA.GD.A3OQ.11" "TCGA.K4.A3WV.11"
## [19] "TCGA.K4.A54R.11" "TCGA.K4.A5RI.11"
```


> 一共20列，除去第一列geneID，共有19个正常组织样本，而且代号都是11，代表癌旁组织，但此概念在临床上并不够严格，接下来，我们寻找对应的19个癌组织样本。



```r
# 先得到不含geneID列的raw_counts矩阵，作为中间变量
x <- colnames(tumor_matched_normal_counts)[-1]
x
```

```
##  [1] "TCGA.BL.A13J.11" "TCGA.BT.A20N.11" "TCGA.BT.A20Q.11"
##  [4] "TCGA.BT.A20R.11" "TCGA.BT.A20U.11" "TCGA.BT.A20W.11"
##  [7] "TCGA.BT.A2LA.11" "TCGA.BT.A2LB.11" "TCGA.CU.A0YN.11"
## [10] "TCGA.CU.A0YR.11" "TCGA.GC.A3BM.11" "TCGA.GC.A3WC.11"
## [13] "TCGA.GC.A6I3.11" "TCGA.GD.A2C5.11" "TCGA.GD.A3OP.11"
## [16] "TCGA.GD.A3OQ.11" "TCGA.K4.A3WV.11" "TCGA.K4.A54R.11"
## [19] "TCGA.K4.A5RI.11"
```

```r
# 再筛选出样本信息，9-12位字符
y <- substr(x, 9, 12)

# 从原始数据中找出包含癌和癌旁组织样本的矩阵
z <- subset(RSEM_raw_counts, select = grep(paste(y, collapse = "|"), colnames(RSEM_raw_counts), value = TRUE)) # 稍复杂，主要弄懂grep的用法即可，学R就要多help！
colnames(z)
```

```
##  [1] "TCGA.BL.A13J.11" "TCGA.BL.A13J.01" "TCGA.BT.A20N.11"
##  [4] "TCGA.BT.A20N.01" "TCGA.BT.A20Q.11" "TCGA.BT.A20Q.01"
##  [7] "TCGA.BT.A20R.11" "TCGA.BT.A20R.01" "TCGA.BT.A20U.11"
## [10] "TCGA.BT.A20U.01" "TCGA.BT.A20W.11" "TCGA.BT.A20W.01"
## [13] "TCGA.BT.A2LA.11" "TCGA.BT.A2LA.01" "TCGA.BT.A2LB.11"
## [16] "TCGA.BT.A2LB.01" "TCGA.CU.A0YN.11" "TCGA.CU.A0YN.01"
## [19] "TCGA.CU.A0YR.11" "TCGA.CU.A0YR.01" "TCGA.GC.A3BM.11"
## [22] "TCGA.GC.A3BM.01" "TCGA.GC.A3WC.11" "TCGA.GC.A3WC.01"
## [25] "TCGA.GC.A6I3.11" "TCGA.GC.A6I3.01" "TCGA.GD.A2C5.11"
## [28] "TCGA.GD.A2C5.01" "TCGA.GD.A3OP.11" "TCGA.GD.A3OP.01"
## [31] "TCGA.GD.A3OQ.11" "TCGA.GD.A3OQ.01" "TCGA.K4.A3WV.11"
## [34] "TCGA.K4.A3WV.01" "TCGA.K4.A54R.11" "TCGA.K4.A54R.01"
## [37] "TCGA.K4.A5RI.11" "TCGA.K4.A5RI.01"
```

```r
# 再从中筛选出癌组织样本的矩阵，感觉绕了点弯，目前咱这R水平，就不讲究了，哈哈
tumor_counts <- subset(z, select = setdiff(colnames(z), colnames(tumor_matched_normal_counts)))

# 把geneID加上
tumor_counts <- cbind(geneID, tumor_counts)
colnames(tumor_counts)
```

```
##  [1] "geneID"          "TCGA.BL.A13J.01" "TCGA.BT.A20N.01"
##  [4] "TCGA.BT.A20Q.01" "TCGA.BT.A20R.01" "TCGA.BT.A20U.01"
##  [7] "TCGA.BT.A20W.01" "TCGA.BT.A2LA.01" "TCGA.BT.A2LB.01"
## [10] "TCGA.CU.A0YN.01" "TCGA.CU.A0YR.01" "TCGA.GC.A3BM.01"
## [13] "TCGA.GC.A3WC.01" "TCGA.GC.A6I3.01" "TCGA.GD.A2C5.01"
## [16] "TCGA.GD.A3OP.01" "TCGA.GD.A3OQ.01" "TCGA.K4.A3WV.01"
## [19] "TCGA.K4.A54R.01" "TCGA.K4.A5RI.01"
```

```r
# 最后，合并两个矩阵
cts_BLCA <- cbind(tumor_matched_normal_counts, tumor_counts[-1])
colnames(cts_BLCA)
```

```
##  [1] "geneID"          "TCGA.BL.A13J.11" "TCGA.BT.A20N.11"
##  [4] "TCGA.BT.A20Q.11" "TCGA.BT.A20R.11" "TCGA.BT.A20U.11"
##  [7] "TCGA.BT.A20W.11" "TCGA.BT.A2LA.11" "TCGA.BT.A2LB.11"
## [10] "TCGA.CU.A0YN.11" "TCGA.CU.A0YR.11" "TCGA.GC.A3BM.11"
## [13] "TCGA.GC.A3WC.11" "TCGA.GC.A6I3.11" "TCGA.GD.A2C5.11"
## [16] "TCGA.GD.A3OP.11" "TCGA.GD.A3OQ.11" "TCGA.K4.A3WV.11"
## [19] "TCGA.K4.A54R.11" "TCGA.K4.A5RI.11" "TCGA.BL.A13J.01"
## [22] "TCGA.BT.A20N.01" "TCGA.BT.A20Q.01" "TCGA.BT.A20R.01"
## [25] "TCGA.BT.A20U.01" "TCGA.BT.A20W.01" "TCGA.BT.A2LA.01"
## [28] "TCGA.BT.A2LB.01" "TCGA.CU.A0YN.01" "TCGA.CU.A0YR.01"
## [31] "TCGA.GC.A3BM.01" "TCGA.GC.A3WC.01" "TCGA.GC.A6I3.01"
## [34] "TCGA.GD.A2C5.01" "TCGA.GD.A3OP.01" "TCGA.GD.A3OQ.01"
## [37] "TCGA.K4.A3WV.01" "TCGA.K4.A54R.01" "TCGA.K4.A5RI.01"
```

```r
# 清空一些中间变量，并且执行垃圾回收gc
rm(x, y, z)
gc()
```

```
##            used  (Mb) gc trigger  (Mb) max used  (Mb)
## Ncells  3595499 192.1    6861544 366.5  5301669 283.2
## Vcells 13012065  99.3   25922260 197.8 25914424 197.8
```


> cts_BLCA便是我们需要的，可以直接用于DE分析的counts矩阵了！


## 几点解释

1. **为什么要下载`Preprocess`文件，里面的文件都是什么？**
    + 其实也可以下载`illuminahiseq_rnaseqv2-RSEM_genes`这个文件, `Preprocess`中单独把`raw_counts`给弄出来了，方便后续操作
    
    
    + 有关解压后的文件内容详情请看此博文：[Understanding TCGA mRNA Level3 analysis results files from FireBrowse](http://zyxue.github.io/2017/06/02/understanding-TCGA-mRNA-Level3-analysis-results-files-from-firebrose.html)。

2. **什么是RSEM (RNA-Seq by Expectation Maximization)？**

    + 它是一种对转录本(transcript)计数的算法，得到的raw_counts与传统方法得到的有较大差别，最显见的就是counts是实数而不是整数，有小数点，而后续的DE分析需要整数counts，处理方法下节再说
    
    
    + 因为统计学得不牢，更详细的内容还请参考[RSEM宣传页](https://deweylab.github.io/RSEM/)以及这篇文献：[RSEM: accurate transcript quantification from RNA-Seq data with or without a reference genome](https://bmcbioinformatics.biomedcentral.com/articles/10.1186/1471-2105-12-323)。
    
3. **md5、aux以及mage-tab文件是什么？**
    + 还未深入了解过，给大家提供个传送门：[TCGA wiki](https://wiki.nci.nih.gov/display/TCGA/TCGA+Wiki+Home)。

4. **sessionInfo：**

```r
devtools::session_info()
```

```
## Session info -------------------------------------------------------------
```

```
##  setting  value                         
##  version  R version 3.4.3 (2017-11-30)  
##  system   x86_64, mingw32               
##  ui       RTerm                         
##  language (EN)                          
##  collate  Chinese (Simplified)_China.936
##  tz       Europe/Berlin                 
##  date     2017-12-08
```

```
## Packages -----------------------------------------------------------------
```

```
##  package              * version  date       source        
##  acepack                1.4.1    2016-10-29 CRAN (R 3.4.2)
##  annotate               1.56.1   2017-11-13 Bioconductor  
##  AnnotationDbi          1.40.0   2017-10-31 Bioconductor  
##  assertthat             0.2.0    2017-04-11 CRAN (R 3.4.2)
##  backports              1.1.1    2017-09-25 CRAN (R 3.4.1)
##  base                 * 3.4.3    2017-11-30 local         
##  base64enc              0.1-3    2015-07-28 CRAN (R 3.4.1)
##  bindr                  0.1      2016-11-13 CRAN (R 3.4.2)
##  bindrcpp               0.2      2017-06-17 CRAN (R 3.4.2)
##  Biobase              * 2.38.0   2017-10-31 Bioconductor  
##  BiocGenerics         * 0.24.0   2017-10-31 Bioconductor  
##  BiocParallel           1.12.0   2017-10-31 Bioconductor  
##  bit                    1.1-12   2014-04-09 CRAN (R 3.4.1)
##  bit64                  0.9-7    2017-05-08 CRAN (R 3.4.1)
##  bitops                 1.0-6    2013-08-17 CRAN (R 3.4.1)
##  blob                   1.1.0    2017-06-17 CRAN (R 3.4.2)
##  checkmate              1.8.5    2017-10-24 CRAN (R 3.4.2)
##  cluster                2.0.6    2017-03-10 CRAN (R 3.4.3)
##  colorspace             1.3-2    2016-12-14 CRAN (R 3.4.2)
##  compiler               3.4.3    2017-11-30 local         
##  data.table             1.10.4-3 2017-10-27 CRAN (R 3.4.2)
##  datasets             * 3.4.3    2017-11-30 local         
##  DBI                    0.7      2017-06-18 CRAN (R 3.4.2)
##  DelayedArray         * 0.4.1    2017-11-04 Bioconductor  
##  DESeq2               * 1.18.1   2017-11-12 Bioconductor  
##  devtools               1.13.4   2017-11-09 CRAN (R 3.4.2)
##  digest                 0.6.12   2017-01-27 CRAN (R 3.4.2)
##  dplyr                  0.7.4    2017-09-28 CRAN (R 3.4.2)
##  evaluate               0.10.1   2017-06-24 CRAN (R 3.4.2)
##  foreign                0.8-69   2017-06-22 CRAN (R 3.4.3)
##  Formula                1.2-2    2017-07-10 CRAN (R 3.4.1)
##  genefilter             1.60.0   2017-10-31 Bioconductor  
##  geneplotter            1.56.0   2017-10-31 Bioconductor  
##  GenomeInfoDb         * 1.14.0   2017-10-31 Bioconductor  
##  GenomeInfoDbData       0.99.1   2017-11-13 Bioconductor  
##  GenomicRanges        * 1.30.0   2017-10-31 Bioconductor  
##  ggplot2                2.2.1    2016-12-30 CRAN (R 3.4.2)
##  glue                   1.2.0    2017-10-29 CRAN (R 3.4.2)
##  graphics             * 3.4.3    2017-11-30 local         
##  grDevices            * 3.4.3    2017-11-30 local         
##  grid                   3.4.3    2017-11-30 local         
##  gridExtra              2.3      2017-09-09 CRAN (R 3.4.2)
##  gtable                 0.2.0    2016-02-26 CRAN (R 3.4.2)
##  Hmisc                  4.0-3    2017-05-02 CRAN (R 3.4.2)
##  hms                    0.4.0    2017-11-23 CRAN (R 3.4.2)
##  htmlTable              1.11.0   2017-12-01 CRAN (R 3.4.3)
##  htmltools              0.3.6    2017-04-28 CRAN (R 3.4.2)
##  htmlwidgets            0.9      2017-07-10 CRAN (R 3.4.2)
##  IRanges              * 2.12.0   2017-10-31 Bioconductor  
##  knitr                  1.17     2017-08-10 CRAN (R 3.4.2)
##  lattice                0.20-35  2017-03-25 CRAN (R 3.4.3)
##  latticeExtra           0.6-28   2016-02-09 CRAN (R 3.4.2)
##  lazyeval               0.2.1    2017-10-29 CRAN (R 3.4.2)
##  locfit                 1.5-9.1  2013-04-20 CRAN (R 3.4.2)
##  magrittr               1.5      2014-11-22 CRAN (R 3.4.2)
##  Matrix                 1.2-12   2017-11-16 CRAN (R 3.4.2)
##  matrixStats          * 0.52.2   2017-04-14 CRAN (R 3.4.2)
##  memoise                1.1.0    2017-04-21 CRAN (R 3.4.2)
##  methods              * 3.4.3    2017-11-30 local         
##  munsell                0.4.3    2016-02-13 CRAN (R 3.4.2)
##  nnet                   7.3-12   2016-02-02 CRAN (R 3.4.3)
##  parallel             * 3.4.3    2017-11-30 local         
##  pkgconfig              2.0.1    2017-03-21 CRAN (R 3.4.2)
##  plyr                   1.8.4    2016-06-08 CRAN (R 3.4.2)
##  purrr                  0.2.4    2017-10-18 CRAN (R 3.4.2)
##  R6                     2.2.2    2017-06-17 CRAN (R 3.4.2)
##  RColorBrewer           1.1-2    2014-12-07 CRAN (R 3.4.1)
##  Rcpp                   0.12.14  2017-11-23 CRAN (R 3.4.2)
##  RCurl                  1.95-4.8 2016-03-01 CRAN (R 3.4.1)
##  readr                * 1.1.1    2017-05-16 CRAN (R 3.4.2)
##  rlang                  0.1.4    2017-11-05 CRAN (R 3.4.2)
##  rmarkdown              1.8      2017-11-17 CRAN (R 3.4.3)
##  rpart                  4.1-11   2017-03-13 CRAN (R 3.4.3)
##  rprojroot              1.2      2017-01-16 CRAN (R 3.4.2)
##  RSQLite                2.0      2017-06-19 CRAN (R 3.4.1)
##  rstudioapi             0.7      2017-09-07 CRAN (R 3.4.2)
##  S4Vectors            * 0.16.0   2017-10-31 Bioconductor  
##  scales                 0.5.0    2017-08-24 CRAN (R 3.4.2)
##  splines                3.4.3    2017-11-30 local         
##  stats                * 3.4.3    2017-11-30 local         
##  stats4               * 3.4.3    2017-11-30 local         
##  stringi                1.1.6    2017-11-17 CRAN (R 3.4.2)
##  stringr                1.2.0    2017-02-18 CRAN (R 3.4.2)
##  SummarizedExperiment * 1.8.0    2017-10-31 Bioconductor  
##  survival               2.41-3   2017-04-04 CRAN (R 3.4.3)
##  tibble                 1.3.4    2017-08-22 CRAN (R 3.4.2)
##  tidyr                  0.7.2    2017-10-16 CRAN (R 3.4.2)
##  tools                  3.4.3    2017-11-30 local         
##  utils                * 3.4.3    2017-11-30 local         
##  withr                  2.1.0    2017-11-01 CRAN (R 3.4.2)
##  XML                    3.98-1.9 2017-06-19 CRAN (R 3.4.1)
##  xtable                 1.8-2    2016-02-05 CRAN (R 3.4.2)
##  XVector                0.18.0   2017-10-31 Bioconductor  
##  yaml                   2.1.15   2017-12-01 CRAN (R 3.4.3)
##  zlibbioc               1.24.0   2017-10-31 Bioconductor
```
