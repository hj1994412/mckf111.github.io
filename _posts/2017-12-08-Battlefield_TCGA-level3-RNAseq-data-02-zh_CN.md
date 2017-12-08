---
layout: post
title: "TCGA����ս������������RNA-seq����02"
date: 2017-12-08
categories: Battlefield_TCGA(����)
tags: TCGA R DESeq2 firehose RNA-seq
author: Wenhu
mathjax: true
output: 
  html_document:
    keep_md: true
---

��ƪΪ�ڶ����֣���Ҫ��¼�������غ�������̡�




## ����ѡ��

һ��ʼ����������̽��һ�°���֯�Ͱ�����֯�Ļ�����컯������(differential expression�����DE)��Ϊ�˼ӿ�����ٶȣ�����ѡ����Firehose�����ڵ�һλ��**ACC (Adrenocortical carcinoma��������Ƥ�ʰ�)**����Ϊֻ������92���������������꾹���֣����в�û�а�����֯����(tumor_matched_normal�����TN)���������ֲ������а�֢��RNA-seq���ݶ�������TN�������Ҷ�ĿǰGDC�а�֢RNA-seq�������˸���ͳ�ƣ�����ͼ��

![TCGA_RNAseq_Summary](https://farm5.staticflickr.com/4728/38023598755_0da2d37dbe_b.jpg)

���ԣ����η�������ѡ�����ŵڶ�����412��������**BLCA (Bladder urothelial carcinoma��������·��Ƥ��)**��RNA-seq���ݡ�

> ������ְ���֯��������֯������������֯��������ο�[TCGA barcode](https://wiki.nci.nih.gov/display/TCGA/TCGA+barcode)�Ľ��ͣ�����ɸѡ����ʱ���õ���

## ��������

����firehose_get���أ����ؼ���װǰһ����˵������linux�����У�������Ŀ����Ŀ¼��

>�˴���һ����Ҫע�⣬**�����linux��Ŀ¼��windowsϵͳ��Ҳ�ɼ�**�������ȷ����������в�����Ҳ�����غʹ�������ݱ�����̽���ṩ�˷��㣬��������һ�������֣���㻹�Ǻ���Ҫ�ġ�����linux�ҲŸո����ţ������õķ�����[Biostar Handbook](https://www.biostarhandbook.com/)�н��ܵģ�

```
# ������Ŀ¼����һ���ļ��У��˴�������ʾ��(ע�⽫your_user_name�������Լ����û���)
mkdir -p '/mnt/c/Users/your_user_name/Desktop/unix'

# ���ļ������ӵ�linuxϵͳ��
ln -s  '/mnt/c/Users/your_user_name/Desktop/unix' ~/unix

# �鿴
cd
ls -lh

# �ܿ�����һ�б�ʾ�ɹ������unix�ļ����е������ļ���������Windows��Linux�н��в���
lrwxrwxrwx 1 mckf111 mckf111  31 Dec  8 14:10 unix -> /mnt/c/Users/your_user_name/Desktop/unix
```

�������������Ŀ�ļ��У�Ȼ��Ϳ��Կ�ʼ��������Ҫ�������ˡ�

```
firehose_get -tasks rnaseq_pre stddata latest blca

# ����ǰȷ�ϣ�����y���س������ؿ�ʼ

# ������ɺ�������ڲ�Ŀ¼
cd stddata__2016_07_15/BLCA/20160715/

# �鿴�ļ�
ls -lh

# ɾ��md5�ļ���aux�Լ�mage-tab�ļ�
rm -f *md5 *aux* *mage-tab*
ls -lh

# ��ѹ
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

# ����ֻ��ҪBLCA.uncv2.mRNAseq_raw_counts.txt�ļ������ͼ���ĩ
mkdir rnaseq_test
cp BLCA.uncv2.mRNAseq_raw_counts.txt ./rnaseq_test/
cd rnaseq_test/
ls -lh

-rwxrwxrwx 1 root root 34M Dec  8 14:00 BLCA.uncv2.mRNAseq_raw_counts.txt

```
�������˾��������ˣ��������Ļ��Ҫ��R������ˡ�
���ǿ���д�ű���Ȼ�������linux�����У�Ҳ����ֱ����RStudio�����У����Ĳ��ú��ߡ�



```r
library(DESeq2)
library(readr)
```

```r
# ��������
RSEM_raw_counts <- read.table("F:/biostar/test/stddata__2016_07_15/BLCA/20160715/gdac.broadinstitute.org_BLCA.mRNAseq_Preprocess.Level_3.2016071500.0.0/rnaseq_test/BLCA.uncv2.mRNAseq_raw_counts.txt", sep = '\t', header = TRUE)

# �ҳ�������֯��raw_counts����
x <- colnames(RSEM_raw_counts)
geneID <- RSEM_raw_counts[, 1] # �ѵ�һ��geneID�����ó���
colnames(RSEM_raw_counts)[1] <- "geneID" # ͬʱ����һ�����ø�ͬ��������
x <- x[-1] # ��ȥ��һ�е���������������
x <- substr(x, 14, 15) # ��ȡ��������14��15���ַ������������������
x <- as.integer(x)
x <- x %in% (10:19) # 10-19֮���������֯����
y <- colnames(RSEM_raw_counts)[-1]
y <- y[x] # �ҵ�������֯����������
tumor_matched_normal_counts <- cbind(geneID, subset(RSEM_raw_counts, select = y)) # Bingo

# �鿴
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

> һ��20�У���ȥ��һ��geneID������19��������֯������������������Ѱ�Ҷ�Ӧ��19������֯������


```r
# �ȵõ�����geneID�е�raw_counts������Ϊ�м����
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
# ��ɸѡ��������Ϣ��9-12λ�ַ�
y <- substr(x, 9, 12)

# ��ԭʼ�������ҳ��������Ͱ�����֯�����ľ���
z <- subset(RSEM_raw_counts, select = grep(paste(y, collapse = "|"), colnames(RSEM_raw_counts), value = TRUE)) # �Ը��ӣ���ҪŪ��grep���÷����ɣ�ѧR��Ҫ��help��
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
# �ٴ���ɸѡ������֯�����ľ��󣬸о����˵��䣬Ŀǰ����Rˮƽ���Ͳ������ˣ�����
tumor_counts <- subset(z, select = setdiff(colnames(z), colnames(tumor_matched_normal_counts)))

# ��geneID����
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
# ��󣬺ϲ���������
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
# ���һЩ�м����������ִ����������gc
rm(x, y, z)
gc()
```

```
##            used  (Mb) gc trigger  (Mb) max used  (Mb)
## Ncells  3595378 192.1    6861544 366.5  5301849 283.2
## Vcells 13011766  99.3   25922260 197.8 25914136 197.8
```

cts_BLCA����������Ҫ�ģ�����ֱ������DE������counts�����ˣ�

## �������

1. ΪʲôҪ����`Preprocess`�ļ���������ļ�����ʲô��
    + ��ʵҲ��������`illuminahiseq_rnaseqv2-RSEM_genes`����ļ�, `Preprocess`�е�����`raw_counts`��Ū�����ˣ���������������йؽ�ѹ����ļ����������뿴�˲��ģ�[Understanding TCGA mRNA Level3 analysis results files from FireBrowse](http://zyxue.github.io/2017/06/02/understanding-TCGA-mRNA-Level3-analysis-results-files-from-firebrose.html)��

2. ʲô��RSEM (RNA-Seq by Expectation Maximization)��
    + ����һ�ֶ�ת¼��(transcript)�������㷨���õ���raw_counts�봫ͳ�����õ����нϴ������Լ��ľ���counts��ʵ����������������С���㣬��������DE������Ҫ����counts���������½���˵����Ϊͳ��ѧ�ò��Σ�����ϸ�����ݻ���ο�[RSEM����ҳ](https://deweylab.github.io/RSEM/)�Լ���ƪ���ף�[RSEM: accurate transcript quantification from RNA-Seq data with or without a reference genome](https://bmcbioinformatics.biomedcentral.com/articles/10.1186/1471-2105-12-323)��
    
3. md5��aux�Լ�mage-tab�ļ���ʲô��
    + ��δ�����˽����������ṩ�������ţ�[TCGA wiki](https://wiki.nci.nih.gov/display/TCGA/TCGA+Wiki+Home)��

4. sessionInfo

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
##  collate  English_United States.1252  
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
