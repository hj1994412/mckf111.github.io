---
layout: post
title: "SRA、SAM以及Fastq文件高速下载方法"
author: Wenhu
date: 2017-12-23
mathjax: yes
tags: SRA Aspera prefetch fastq-dump
categories: Data_download(中文)
---

* content
{:toc}

> 这是个简短的教程，目的是介绍几种比较方便快捷的下载SRA、SAM及Fastq文件的方法。

## NCBI-SRA和EBI-ENA数据库

[SRA数据库: Sequence Read Archive](https://www.ncbi.nlm.nih.gov/sra)：隶属NCBI (National Center for Biotechnology Information)，它是一个保存高通量测序**原始数据**以及比对信息和元数据 (metadata) 的数据库，所有已发表的文献中高通量测序数据基本都上传至此，方便其他研究者下载及再研究。其中的数据则是通过压缩后以.sra文件格式来保存的。




[ENA数据库：European Nucleotide Archive](https://www.ebi.ac.uk/ena)：隶属EBI (European Bioinformatics Institute)，功能同SRA，并且对数据做了注释，界面更友好，当然对于我们来说，最诱人的当属可直接下载fastq (.gz)文件这一项了。

## sra文件下载方式

> 多数情况下，我们下载sra文件是为了获取相应的fastq或者sam文件，这样可以和自己的pipeline对接上，直接分析，所以

1. **找地方**：用手头上的SRR (SRA Run)序列号去ENA搜索，如果有，就在这儿下；如果没有，就去SRA数据库下载

2. **选方法**：

    + **首选[Aspera Connect](http://downloads.asperasoft.com/en/downloads/8?list)软件**，这是IBM旗下的商业高速文件传输软件，与NCBI和EBI有协作合同，我们可以免费使用它下载高通量测序文件，体验飞一般的感觉，速度可飚至300-500M/s。下载完成后，本地用fastq-dump提取fastq文件，用sam-dump提取SAM文件。

    + 其次，如果上述方法不奏效，**优先使用sratoolkit中的[prefetch](https://trace.ncbi.nlm.nih.gov/Traces/sra/sra.cgi?view=toolkit_doc&f=prefetch)命令**。

    + 最后，使用sratoolkit中的[fastq-dump](https://trace.ncbi.nlm.nih.gov/Traces/sra/sra.cgi?view=toolkit_doc&f=fastq-dump)和[sam-dump](https://trace.ncbi.nlm.nih.gov/Traces/sra/sra.cgi?view=toolkit_doc&f=sam-dump)命令下载，**如果fastq-dump不稳定，推荐大家尝试[Biostar Handbook](https://www.biostarhandbook.com/)中的[wonderdump](http://data.biostarhandbook.com/scripts/wonderdump.sh)脚本**。

> **警告**：不要用wget或curl去下载sra文件，这会导致下载的文件不完整！

## Aspera Connect命令行工具ascp的安装

> 首先，进入[Aspera Connect](http://downloads.asperasoft.com/en/downloads/8?list)的下载页面，选择linux版本，复制下载地址

```
wget http://download.asperasoft.com/download/sw/connect/3.7.4/aspera-connect-3.7.4.147727-linux-64.tar.gz

tar zxvf aspera-connect-3.7.4.147727-linux-64.tar.gz

# 安装
bash aspera-connect-3.7.4.147727-linux-64.sh

# 查看是否有.aspera文件夹
cd # 去根目录
ls -a # 如果看到.aspera文件夹，代表安装成功

# 永久添加环境变量
echo 'export PATH=~/.aspera/connect/bin:$PATH' >> ~/.bashrc
source ~/.bashrc

# 查看帮助文档
ascp --help
```

> 至此，安装完成，下面介绍如何利用`ascp`在SRA和ENA中下载数据

> `ascp`的用法：**_ascp [参数] 目标文件 目标地址_**，[在线文档](https://download.asperasoft.com/download/docs/ascp/2.6/html/index.html?https://download.asperasoft.com/download/docs/ascp/2.6/html/fasp/ascp.html)

> 先了解几个`ascp`命令的常用参数

`-v` verbose mode 唠叨模式，能让你实时知道程序在干啥，方便查错。*有些作者的程序缺乏人性化，运行之后，只见光标闪，压根不知道运行到哪了*

`-T` 取消加密，否则有时候数据下载不了

`-i` 提供私钥文件的地址，我也不知道干嘛的，反正不能少，地址一般是~/.aspera/connect/etc中的asperaweb_id_dsa.openssh文件

`-l` 设置最大传输速度，一般200m到500m，如果不设置，反而速度会比较低，可能有个较低的默认值

`-k` 断点续传，一般设置为值1

`-Q` 不懂，一般加上它

`-P` 提供SSH port，一般是33001，反正我不懂

## `ascp`使用举例

1. SRA数据库下载：首先记住，数据的存放地址是`ftp-private.ncbi.nlm.nih.gov`，SRA在Aspera的用户名是`anonftp`，下载举例：

    + 如果我想下载`SRR949627.sra`文件，首先我需要找到地址，去[ncbi ftp-private](ftp-private.ncbi.nlm.nih.gov)或者[ncbi faspftp](https://www.ncbi.nlm.nih.gov/projects/faspftp/)，一层层寻找，直至找到，然后记下链接地址，就可以开始下载了：

    ```
    ascp -v -i ~/.aspera/connect/etc/asperaweb_id_dsa.openssh -k 1 -T -l200m anonftp@ftp-private.ncbi.nlm.nih.gov:/sra/sra-instant/reads/ByRun/sra/SRR/SRR949/SRR949627/SRR949627.sra ~/biostar/aspera/
    ```

    + **注意：`anonftp@ftp-private.ncbi.nlm.nih.gov`后面是:号，不是路径/！**
    
    + 一般来说，NCBI的sra文件前面的地址都是一样的`/sra/sra-instant/reads/ByRun/sra/SRR/...`，那么写脚本批量下载也就不难了！


2. ENA数据库下载：这里和上面不同，数据的存放地址是`fasp.sra.ebi.ac.uk`，ENA在Aspera的用户名是`era-fasp`，下载举例：

    + 同样，我还是下载`SRR949627`，方便的是ENA中可以直接下载`fastq.gz`文件，不用再从sra文件慢吞吞的转换了，那么地址呢，可以去[ENA](https://www.ebi.ac.uk/ena)搜索，再复制下fastq.gz文件的地址，或者可以去**ENA的ftp地址`ftp.sra.ebi.ac.uk`搜索，注意，是ftp，不是fasp！**记下链接地址，就可以下载了：

    ```
    ascp -QT -l 300m -P33001 -i ~/.aspera/connect/etc/asperaweb_id_dsa.openssh era-fasp@fasp.sra.ebi.ac.uk:/vol1/fastq/SRR949/SRR949627/SRR949627_1.fastq.gz ~/biostar/aspera/
    ```

    + **注意：`era-fasp@fasp.sra.ebi.ac.uk`后面是:号，不是路径/！**

    + 一般来说，EBI的sra文件前面的地址也都是一样的`vol1/fastq/...`，那么写脚本批量下载也就不难了！

> 好了，就写到这里，赶快试试看吧！


## 参考资料

[使用速铂Aspera下载NGS数据](http://boyun.sh.cn/bio/?p=1933)

[Aspera助力快速下载NCBI基因组与SRA原始数据](https://mp.weixin.qq.com/s/oCmng_iD3-z_BDx6cUC4Fw)


