---
layout: post
title: "High Speed Downloading of SRA, SAM and Fastq Files"
author: Wenhu
date: 2017-12-24
mathjax: yes
tags: SRA ascp Aspera_Connect
categories: Data_download
---

* content
{:toc}

> This is a brief tutorial about methods of downloading sra, sam and fastq files, mainly focusing on Aspera Connect.

## NCBI-SRA and EBI-ENA databases

[SRA: Sequence Read Archive](https://www.ncbi.nlm.nih.gov/sra): It belongs to NCBI (National Center for Biotechnology Information), is a database storing high throughput sequencing (HTS) raw data, alignment information and metadata. Almost all HTS data in published publications will be asked uploading to here, and stored as .sra compressed file format.




[ENA: European Nucleotide Archive](https://www.ebi.ac.uk/ena): It belongs to EBI (European Bioinformatics Institute), although it has the same funtion with SRA, more annotations and friendlier website make it preferable. What's more, you could download directly `fastq.gz` files from it.

## File Downloading

> Mostly, we download sra files for the purpose of getting corresponding fastq or sam files, so as to use them in our own pipeline for downstream analysis.

1. **Places**: You should search ENA database first with the SRR (SRA Run) accession number to check if it is there. If not, go to SRA database.

2. **Methods**: 

    + **First Choice -- [Aspera Connect](http://downloads.asperasoft.com/en/downloads/8?list)**. It is a commercial high speed file transfer software produced by IBM. Since it has contract with NCBI and EBI, we could use it to download data in those two databases **for free**. **Many sites can transfer data at 200-500Mbps. and nearly all sites can transfer at faster than 10Mbps.**

    + If the Aspera Connect doesn't work, I would recommend the **[prefetch](https://trace.ncbi.nlm.nih.gov/Traces/sra/sra.cgi?view=toolkit_doc&f=prefetch) command in sratoolkit**.

    + At last, please try [fastq-dump](https://trace.ncbi.nlm.nih.gov/Traces/sra/sra.cgi?view=toolkit_doc&f=fastq-dump) and [sam-dump](https://trace.ncbi.nlm.nih.gov/Traces/sra/sra.cgi?view=toolkit_doc&f=sam-dump) in sratoolkit. **If the connection of `fastq-dump` is unstable, I would suggest the [wonderdump](http://data.biostarhandbook.com/scripts/wonderdump.sh) script in [Biostar Handbook](https://www.biostarhandbook.com/)**.

> **Warning**: Try not to use `wget` or `curl` to download, it might cause incompletion in downloaded sra files.

## Installation of Aspera Connect command line tool -- `ascp`

> Firstly, go to [Aspera Connect](http://downloads.asperasoft.com/en/downloads/8?list), choose the linux version and copy link address

```
wget http://download.asperasoft.com/download/sw/connect/3.7.4/aspera-connect-3.7.4.147727-linux-64.tar.gz

tar zxvf aspera-connect-3.7.4.147727-linux-64.tar.gz

# install
bash aspera-connect-3.7.4.147727-linux-64.sh

# check the .aspera directory
cd # go to root directory
ls -a # if you could see .aspera, the installation is OK

# add environment variable
echo 'export PATH=~/.aspera/connect/bin:$PATH' >> ~/.bashrc
source ~/.bashrc

# check help file
ascp --help
```

> The installation is finished now, then I will introduce how to download data in SRA and ENA with `ascp`

> `ascp` one-liner: **_ascp [options] target-file storage-directory_**，[online documentation](https://download.asperasoft.com/download/docs/ascp/2.6/html/index.html?https://download.asperasoft.com/download/docs/ascp/2.6/html/fasp/ascp.html)

> Some need-to-know options

`-v` verbose mode, let you know what the program is doing in time, better add it for debugging.

`-T` Disable encryption, otherwise downloading will be interrupted sometimes.

`-i` Use public key authentication and specify the private key file, the address normally is `~/.aspera/connect/etc/asperaweb_id_dsa.openssh`.

`-l` Set the target transfer rate in Kbps, normally is 200m - 500m. The default rate is rather low, you would better declare it explicitly.

`-k` Enable resuming partially transferred files, better set value 1.

`-Q` Enable fair transfer policy, I don't understand, use it when download data from ENA database.

`-P` Set the TCP port used for fasp session initiation, just use value 33001.

## `ascp` Examples

1. From SRA database: remember first, the data location is `ftp-private.ncbi.nlm.nih.gov`, and the username for SRA in Aspera is `anonftp`, details below: 

    + If I want to download `SRR949627.sra`，firstly I need to go to [ncbi ftp-private](ftp-private.ncbi.nlm.nih.gov) or [ncbi faspftp](https://www.ncbi.nlm.nih.gov/projects/faspftp/) to look for the downloading link, *since the sra addresses are similar, finding the link is not a big deal*.

    ```
    ascp -v -i ~/.aspera/connect/etc/asperaweb_id_dsa.openssh -k 1 -T -l200m anonftp@ftp-private.ncbi.nlm.nih.gov:/sra/sra-instant/reads/ByRun/sra/SRR/SRR949/SRR949627/SRR949627.sra ~/biostar/aspera/
    ```

    + **Note: There is a ':' after `anonftp@ftp-private.ncbi.nlm.nih.gov`, not '/'！**
    
    + Normally, the sra files in NCBI have similar link, like `/sra/sra-instant/reads/ByRun/sra/SRR/...`, this is easy for writing batch downloading scripts.


2. From ENA database: remember first, the data location is `fasp.sra.ebi.ac.uk`, and the username for ENA in Aspera is `era-fasp`, details below: 

    + I would still try to download `SRR949627`, GOOD news is there is already `fastq.gz` file in ENA, we don't bother to download the compressed sra and transfer it to fastq file. Where is the link? You could search the [ENA](https://www.ebi.ac.uk/ena) for it, or go to the ftp location `ftp.sra.ebi.ac.uk`, **note, it begins with `ftp`, not `fasp`!**

    ```
    ascp -QT -l 300m -P33001 -i ~/.aspera/connect/etc/asperaweb_id_dsa.openssh era-fasp@fasp.sra.ebi.ac.uk:/vol1/fastq/SRR949/SRR949627/SRR949627_1.fastq.gz ~/biostar/aspera/
    ```

    + **Note: There is a ':' after `era-fasp@fasp.sra.ebi.ac.uk`, not '/'！**

    + Normally, the sra files in EBI have similar link, like `vol1/fastq/...`, this is easy for writing batch downloading scripts.

> OK, that's all! Have fun!


## References

[使用速铂Aspera下载NGS数据](http://boyun.sh.cn/bio/?p=1933)

[Aspera助力快速下载NCBI基因组与SRA原始数据](https://mp.weixin.qq.com/s/oCmng_iD3-z_BDx6cUC4Fw)


> Please subscribe [RSS](http://bioinfostar.com/feed.xml) if you wanna receive my newest post!
