---
layout: post
title: "TCGA�1�7�1�7�1�7�1�7�0�9�1�7�1�7�1�7�1�7�1�7�1�7�1�7�1�7�1�7�1�7�1�7�1�7RNA-seq�1�7�1�7�1�7�1�702"
date: 2017-12-08
categories: Battlefield_TCGA(�1�7�1�7�1�7�1�7)
tags: TCGA R DESeq2 firehose RNA-seq
author: Wenhu
mathjax: true
output: 
  html_document:
    keep_md: true
---

�1�7�1�7�0�0�0�2�1�7�1�2�1�7�1�7�1�7�1�7�0�7�1�7�1�7�1�7�0�8�1�7�1�7�0�4�1�7�1�7�1�7�1�7�1�7�1�7�1�7�1�8�1�7�1�7�1�7�1�7�1�7�1�7�1�7�0�9�1�7




## �1�7�1�7�1�7�1�7�0�5�1�7�1�7

�0�5�1�7�1�7�0�3�1�7�1�7�1�7�1�7�1�7�1�7�1�7�1�7�1�7�1�7�0�7�1�7�1�7�0�5�1�7��1�7�1�7�1�7�0�9�1�7�0�8�1�7�1�7�1�7�1�7�1�7�0�9�1�7�0�3�1�7�1�7�1�7�1�7�1�7�4�8�1�7�1�7�1�7�1�7�1�7�1�7(differential expression�1�7�1�7�1�7�1�7�1�7DE)�1�7�1�7�0�2�1�7�0�2�0�3�1�7�1�7�1�7�1�7�1�7�1�8�0�2�1�7�1�7�1�7�1�7�1�7�0�5�1�7�1�7�1�7�1�7Firehose�1�7�1�7�1�7�1�7�1�7�1�1�1�7�0�5�˄1�7�1�7**ACC (Adrenocortical carcinoma�1�7�1�7�1�7�1�7�1�7�1�7�1�7�1�7�0�4�1�7�0�1�1�7)**�1�7�1�7�1�7�1�7�0�2�0�1�1�7�1�7�1�7�1�7�1�7�1�792�1�7�1�7�1�7�1�7�1�7�1�7�1�7�1�7�1�7�1�7�1�7�1�7�1�7�1�7�1�7�8�8�1�7�1�7�1�7�0�7�1�7�1�7�1�7�1�7�ӄ1�7�0�4�1�7�ф1�7�1�7�1�7�1�7�1�7�0�9�1�7�1�7�1�7�1�7(tumor_matched_normal�1�7�1�7�1�7�1�7�1�7TN)�1�7�1�7�1�7�1�7�1�7�1�7�1�7�1�7�1�7�0�2�1�7�1�7�1�7�1�7�1�7�1�7�ф1�7�0�6�1�7�1�7RNA-seq�1�7�1�7�1�7�1�4�1�7�1�7�1�7�1�7�1�7�1�7�1�7TN�1�7�1�7�1�7�1�7�1�7�1�7�1�7�0�0�1�7�0�7�0�2GDC�1�7�ф1�7�0�6RNA-seq�1�7�1�7�1�7�1�7�1�7�1�7�1�7�0�8�1�7�1�7�1�7�0�1�1�7�0�3�1�7�1�7�1�7�1�7�1�7�0�0�1�7�1�7

![TCGA_RNAseq_Summary](https://farm5.staticflickr.com/4728/38023598755_0da2d37dbe_b.jpg)

�1�7�1�7�1�7�0�9�1�7�1�7�1�7�1�7�Ǆ1�7�1�7�1�7�1�7�1�7�1�7�1�7�0�5�1�7�1�7�1�7�1�7�1�7�0�7�1�2�1�7�1�7�1�7�1�7�1�7412�1�7�1�7�1�7�1�7�1�7�1�7�1�7�1�7**BLCA (Bladder urothelial carcinoma�1�7�1�7�1�7�1�7�1�7�1�7�1�7�1�7���1�7�1�7�0�4�1�7�1�7)**�1�7�1�7RNA-seq�1�7�1�7�1�7�1�3�1�7

> �1�7�1�7�1�7�1�7�1�7�1�7�0�0�1�7�1�7�1�7�0�9�1�7�1�7�1�7�1�7�1�7�1�7�1�7�1�7�0�9�1�7�1�7�1�7�1�7�1�7�1�7�1�7�1�7�1�7�1�7�1�7�1�7�0�9�1�7�1�7�1�7�1�7�1�7�1�7�1�7�1�7�τ1�7[TCGA barcode](https://wiki.nci.nih.gov/display/TCGA/TCGA+barcode)�1�7�0�5�1�7�1�7�0�5�1�7�1�7�1�7�1�7�1�7�0�5�0�5�1�7�1�7�1�7�1�7�0�2�1�7�1�7�1�7�0�1�1�7�1�7�1�7

## �1�7�1�7�1�7�1�7�1�7�1�7�1�7�1�7

�1�7�1�7�1�7�1�7firehose_get�1�7�1�7�1�7�1�5�1�7�1�7�1�7�1�7�1�0�1�7�1�7�1�7�0�4�0�2�0�5�1�7�1�7�1�7�1�7�0�5�1�7�1�7�1�7�1�7�1�7�1�7linux�1�7�1�7�1�7�1�7�1�7���1�7�1�7�1�7�1�7�1�7�1�7�1�7�0�7�1�7�1�7�1�7�1�7�0�7�0�4�1�7�1�7

>�1�7�0�4�1�7�1�7�1�7�0�5�1�7�1�7�1�7�1�7�0�8�0�0�1�7�7�0**�1�7�1�7�1�7�1�7�1�7linux�1�7�1�7�0�7�0�4�1�7�1�7windows�0�3�0�1�1�7�1�7�0�6�1�7�0�9�1�7**�1�7�1�7�1�7�1�7�1�7�1�7�1�7�0�2�1�7�1�7�1�7�1�7�1�7�1�7�1�7�1�7�1�7�1�7�ӄ1�7�1�7�1�7�1�7�1�7�0�6�1�7�1�7�1�7�1�7�1�7�1�8�0�2�1�7�1�7�1�7�1�7�1�7�1�7�1�7�1�9�1�7�1�7�1�7�1�7�1�7�0�7�1�7�1�7�1�7�5�9�1�7�0�7�1�7�1�7�0�4�1�7�1�7�1�7�1�7�1�7�1�7�1�7�1�7�0�5�1�7�1�7�1�7�1�7�1�7�1�7�1�7�0�7�1�7�1�7�1�7�2�4�1�7�0�1�1�7�1�7�1�7�0�8�1�7�0�8�1�7�1�7�1�7�1�7�1�7linux�1�7�0�6�0�0�0�4�1�7�1�7�1�7�1�7�0�0�1�7�1�7�1�7�1�7�1�7�1�7�0�1�0�9�1�7�1�7�1�7�1�7�1�7[Biostar Handbook](https://www.biostarhandbook.com/)�1�7�߄1�7�1�7�1�9�0�0�1�7

```
# �1�7�1�7�1�7�1�7�1�7�1�7�0�7�0�4�1�7�1�7�1�7�1�7�0�5�1�7�1�7�1�7�0�4�1�7�1�7���1�7�1�7�0�4�1�7�1�7�1�7�1�7�1�7�1�7�1�7�0�5�1�7�1�7(�0�0�1�7�9�9your_user_name�1�7�1�7�1�7�1�7�1�7�1�7�1�7�0�4�1�7�1�7�1�7�1�7�0�4�1�7�1�7�1�7)
mkdir -p '/mnt/c/Users/your_user_name/Desktop/unix'

# �1�7�1�7�1�7�0�4�1�7�1�7�1�7�1�7�1�7�1�7�0�3�1�7linux�0�3�0�1�1�7�1�7
ln -s  '/mnt/c/Users/your_user_name/Desktop/unix' ~/unix

# �1�7�5�1
cd
ls -lh

# �1�7�1�9�1�7�1�7�1�7�1�7�1�7�0�5�1�7�҄1�7�0�5�1�7�0�6�1�7�1�7�1�7�1�7�1�7�1�7unix�1�7�0�4�1�7�1�7�1�7�1�7�ք1�7�1�7�1�7�1�7�1�7�1�7�0�4�1�7�1�7�1�7�1�7�1�7�1�7�1�7�1�7�1�7Windows�1�7�1�7Linux�1�7�߄1�7�1�7�ӄ1�7�1�7�1�7
lrwxrwxrwx 1 mckf111 mckf111  31 Dec  8 14:10 unix -> /mnt/c/Users/your_user_name/Desktop/unix
```

�1�7�1�7�1�7�1�7�1�7�1�7�1�7�1�7�1�7�1�7�1�7�1�7�1�7�0�7�1�7�0�4�1�7�1�7���1�7�0�6�1�7�1�7�0�3�1�7�1�7�0�7�1�7�0�3�1�7�1�7�1�7�1�7�1�7�1�7�1�7�1�7�0�8�1�7�1�7�1�7�1�7�1�7�1�7�1�7�0�5�1�7

```
firehose_get -tasks rnaseq_pre stddata latest blca

# �1�7�1�7�1�7�1�7�0�2�0�2�1�7�0�5�1�7�1�7�1�7�1�7�1�7y�1�7�1�7�1�7�1�1�1�7�1�7�1�7�1�7�1�7�1�7�1�3�1�7�0�3

# �1�7�1�7�1�7�1�7�1�7�1�7�0�7�1�7�1�7�1�7�1�7�1�7�1�7�1�7�1�8�1�7�0�7�0�4
cd stddata__2016_07_15/BLCA/20160715/

# �1�7�5�1�1�7�0�4�1�7
ls -lh

# �0�1�1�7�1�7md5�1�7�0�4�1�7�1�7�1�7aux�1�7�0�4�1�7mage-tab�1�7�0�4�1�7
rm -f *md5 *aux* *mage-tab*
ls -lh

# �1�7�1�7�0�9
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

# �1�7�1�7�1�7�1�7�0�1�1�7�1�7�0�8BLCA.uncv2.mRNAseq_raw_counts.txt�1�7�0�4�1�7�1�7�1�7�1�7�1�7�1�7�0�0�1�7�1�7�1�7�0�6
mkdir rnaseq_test
cp BLCA.uncv2.mRNAseq_raw_counts.txt ./rnaseq_test/
cd rnaseq_test/
ls -lh

-rwxrwxrwx 1 root root 34M Dec  8 14:00 BLCA.uncv2.mRNAseq_raw_counts.txt

```
�1�7�1�7�1�7�1�7�1�7�1�7�1�7�0�4�1�7�1�7�1�7�1�7�1�7�1�7�1�7�1�7�0�7�1�7�1�7�1�7�1�7�1�7�1�7�1�7�1�7�0�3�1�7�1�7�0�8�1�7�1�7R�1�7�1�7�1�7�1�7�1�7�1�7�0�5�1�7
�1�7�1�7�1�7�0�6�1�7�1�7�1�7�Մ1�7�0�3�1�7�1�7�1�7�0�6�1�7�1�7�1�7�1�7�1�7�1�7�1�7linux�1�7�1�7�1�7�1�7�1�7���1�7�0�6�1�7�1�7�1�7�1�7�0�1�1�7�1�7�1�7�1�7RStudio�1�7�1�7�1�7�1�7�1�7���1�7�1�7�1�7�1�7�0�4�1�7�1�7���1�7�1�7�1�1�1�7



```r
library(DESeq2)
library(readr)
```

```r
# �1�7�1�7�1�7�1�7�1�7�1�7�1�7�1�7
RSEM_raw_counts <- read.table("F:/biostar/test/stddata__2016_07_15/BLCA/20160715/gdac.broadinstitute.org_BLCA.mRNAseq_Preprocess.Level_3.2016071500.0.0/rnaseq_test/BLCA.uncv2.mRNAseq_raw_counts.txt", sep = '\t', header = TRUE)

# �1�7�0�7�1�7�1�7�1�7�1�7�1�7�1�7�1�7�0�9�1�7�1�7raw_counts�1�7�1�7�1�7�1�7
x <- colnames(RSEM_raw_counts)
geneID <- RSEM_raw_counts[, 1] # �1�7�0�5�1�7�0�5�1�7�1�7geneID�1�7�1�7�1�7�1�7�1�7���1�7�1�7�1�7
colnames(RSEM_raw_counts)[1] <- "geneID" # �0�4�0�2�1�7�1�7�1�7�1�7�0�5�1�7�1�7�1�7�1�7�1�7�0�3�1�7�0�4�1�7�1�7�1�7�1�7�1�7�1�7�1�7�1�7
x <- x[-1] # �1�7�1�7�0�4�1�7�1�7�0�5�1�7�ք1�7�1�7�1�7�1�7�1�7�1�7�1�7�1�7�1�7�1�7�1�7�1�7�1�7�1�7�1�7
x <- substr(x, 14, 15) # �1�7�1�7�0�0�1�7�1�7�1�7�1�7�1�7�1�7�1�7�1�714�1�7�1�715�1�7�1�7�1�7�0�7�1�7�1�7�1�7�1�7�1�7�1�7�1�7�1�7�1�7�1�7�1�7�1�7�1�7�1�7�1�7�1�7�1�7�1�7
x <- as.integer(x)
x <- x %in% (10:19) # 10-19�0�8�1�7�1�7�1�7�1�7�1�7�1�7�1�7�1�7�1�7�0�9�1�7�1�7�1�7�1�7
y <- colnames(RSEM_raw_counts)[-1]
y <- y[x] # �1�7�0�9�1�7�1�7�1�7�1�7�1�7�1�7�1�7�0�9�1�7�1�7�1�7�1�7�1�7�1�7�1�7�1�7�1�7�1�7
tumor_matched_normal_counts <- cbind(geneID, subset(RSEM_raw_counts, select = y)) # Bingo

# �1�7�5�1
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

> �0�5�1�7�1�720�1�7���1�7�1�7�1�7�0�4�1�7�1�7�0�5�1�7�1�7geneID�1�7�1�7�1�7�1�7�1�7�1�719�1�7�1�7�1�7�1�7�1�7�1�7�1�7�1�7�0�9�1�7�1�7�1�7�1�7�1�7�1�7�1�7�1�7�1�7�1�7�1�7�1�7�1�7�1�7�1�7�1�7�1�7�1�7�0�0�1�7�0�0�1�7�0�8�1�7�1�719�1�7�1�7�1�7�1�7�1�7�1�7�0�9�1�7�1�7�1�7�1�7�1�7�1�7


```r
# �1�7�0�0�0�1�1�7�1�7�1�7�1�7�1�7geneID�1�7�ք1�7raw_counts�1�7�1�7�1�7�1�7�1�7�1�7�0�2�1�7�ބ1�7�1�7�1�7�1�7
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
# �1�7�1�7�0�5�0�5�1�7�1�7�1�7�1�7�1�7�1�7�1�7�1�7�0�4�1�7�1�79-12�˄1�7�0�7�1�7
y <- substr(x, 9, 12)

# �1�7�1�7�0�9�0�3�1�7�1�7�1�7�1�7�1�7�1�7�1�7�0�7�1�7�1�7�1�7�1�7�1�7�1�7�1�7�1�7�0�8�1�7�1�7�1�7�1�7�1�7�0�9�1�7�1�7�1�7�1�7�1�7�0�6�1�7�1�7�1�7
z <- subset(RSEM_raw_counts, select = grep(paste(y, collapse = "|"), colnames(RSEM_raw_counts), value = TRUE)) # �1�7�0�0�1�7�1�7�0�5�1�7�1�7�1�7�0�8�0�7�1�7�1�7grep�1�7�1�7�1�7�1�7�1�7�1�7�1�7�0�4�1�7�0�1R�1�7�1�7�0�8�1�7�1�7help�1�7�1�7
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
# �1�7�1�6�1�7�1�7�1�7�0�5�0�5�1�7�1�7�1�7�1�7�1�7�1�7�0�9�1�7�1�7�1�7�1�7�1�7�0�6�1�7�1�7�8�8���1�7�1�7�1�7�1�7�0�5�1�7�1�7�4�8�0�7�0�2�1�7�1�7�1�7�1�7R�0�8�0�9�1�7�1�7�1�7�0�0�1�7�1�7�1�7�1�7�1�7�1�7�0�7�1�7�1�7�1�7�1�7�1�7
tumor_counts <- subset(z, select = setdiff(colnames(z), colnames(tumor_matched_normal_counts)))

# �1�7�1�7geneID�1�7�1�7�1�7�1�7
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
# �1�7�1�7�8�0�0�0�1�7�1�7�1�7�1�7�1�7�1�7�1�7�1�7�1�7
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
# �1�7�1�7�1�7�0�5���1�7�ބ1�7�1�7�1�7�1�7�1�7�1�7�1�7�1�7�1�7�1�7�0�4�1�7�1�7�1�7�1�7�1�7�1�7�1�7�1�7�1�7�1�7gc
rm(x, y, z)
gc()
```

```
##            used  (Mb) gc trigger  (Mb) max used  (Mb)
## Ncells  3595590 192.1    6861544 366.5  5302061 283.2
## Vcells 13012320  99.3   25922260 197.8 25914690 197.8
```

cts_BLCA�1�7�1�7�1�7�1�7�1�7�1�7�1�7�1�7�1�7�1�7�0�8�1�7�0�0�1�7�1�7�1�7�1�7�1�7�0�1�1�7�1�7�1�7�1�7�1�7�1�7DE�1�7�1�7�1�7�1�7�1�7�1�7counts�1�7�1�7�1�7�1�7�1�7�0�7�1�7

## �1�7�1�7�1�7�1�7�1�7�1�7�1�7

1. �0�2�0�3�0�0�0�8�1�7�1�7�1�7�1�7`Preprocess`�1�7�0�4�1�7�1�7�1�7�1�7�1�7�1�7�1�7�1�7�1�7�0�4�1�7�1�7�1�7�1�7�1�7�0�3�0�0�1�7�1�7
    + �1�7�1�7�0�6�0�6�1�7�1�7�1�7�1�7�1�7�1�7�1�7�1�7`illuminahiseq_rnaseqv2-RSEM_genes`�1�7�1�7�1�7�1�7�0�4�1�7, `Preprocess`�1�7�ք1�7�1�7�1�7�1�7�1�7`raw_counts`�1�7�1�7�0�7�1�7�1�7�1�7�1�7�1�7�0�7�1�7�1�7�1�7�1�7�1�7�1�7�1�7�1�7�1�7�1�7�1�7�1�7�1�7�1�7�1�7�ہ1�1�1�7�0�9�1�7�1�7�1�7�1�7�0�4�1�7�1�7�1�7�1�7�1�7�1�7�1�7�1�7�1�7�1�7�1�3�1�7�0�2�1�7�1�7�0�0�1�7[Understanding TCGA mRNA Level3 analysis results files from FireBrowse](http://zyxue.github.io/2017/06/02/understanding-TCGA-mRNA-Level3-analysis-results-files-from-firebrose.html)�1�7�1�7

2. �0�3�0�0�1�7�1�7RSEM (RNA-Seq by Expectation Maximization)�1�7�1�7
    + �1�7�1�7�1�7�1�7�0�5�1�7�0�6�1�7�0�8�0�4�1�7�1�7(transcript)�1�7�1�7�1�7�1�7�1�7�1�7�1�7�1�1�1�7�1�7�1�7�0�1�1�7�1�7�1�7raw_counts�1�7�1�0�0�1�1�7�1�7�1�7�1�7�1�7�0�1�1�7�1�7�1�7�1�7�߁0�2�1�7�1�7�1�7�1�7�1�7�1�7�0�4�1�7�1�7�0�6�1�7�1�7�1�7counts�1�7�1�7�0�6�1�7�1�7�1�7�1�7�1�7�1�7�1�7�1�7�1�7�1�7�1�7�1�7�1�7�1�7�1�7�1�7���1�7�1�7�1�7�0�4�1�7�1�7�1�7�1�7�1�7�1�7�1�7�1�7DE�1�7�1�7�1�7�1�7�1�7�1�7�0�8�1�7�1�7�1�7�1�7counts�1�7�1�7�1�7�1�7�1�7�1�7�1�7�1�7�1�7�0�5�1�7�1�7�1�7�0�5�1�7�1�7�1�7�1�7�0�2�0�1�1�7�1�7�0�1�1�7���1�7�1�7���1�7�1�7�1�7�1�7�1�7�0�6�1�7�1�7�1�7�1�7�1�7�1�9�1�7�1�7�1�7�τ1�7[RSEM�1�7�1�7�1�7�1�7�0�7](https://deweylab.github.io/RSEM/)�1�7�0�4�1�7�1�7�1�7�0�0�1�7�1�7�1�7�0�1�1�7[RSEM: accurate transcript quantification from RNA-Seq data with or without a reference genome](https://bmcbioinformatics.biomedcentral.com/articles/10.1186/1471-2105-12-323)�1�7�1�7
    
3. md5�1�7�1�7aux�1�7�0�4�1�7mage-tab�1�7�0�4�1�7�1�7�1�7�0�3�0�0�1�7�1�7
    + �1�7�1�7�Ą1�7�1�7�1�7�1�7�1�7�0�3�1�7�1�7�1�7�1�7�1�7�1�7�1�7�1�7�1�7�1�7�5�9�1�7�1�7�1�7�1�7�1�7�1�7�1�7�0�0�1�7[TCGA wiki](https://wiki.nci.nih.gov/display/TCGA/TCGA+Wiki+Home)�1�7�1�7

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
