---
layout: post
title: "TCGA大作战——初步分析RNA-seq数据01"
date: 2017-12-07
categories: Battlefield_TCGA(中文)
tags: TCGA R DESeq2 firehose RNA-seq
author: Wenhu
mathjax: true
---

* content
{:toc}

本篇为第一部分，主要记录重要资源地址以及TCGA数据的下载方式。




## 名词及资源

* [TCGA (The Cancer Genome Atlas)](https://cancergenome.nih.gov/)：**人类**癌症基因组图谱，一个数据库，主要用来收集癌症病人癌组织及癌旁组织标本以及极少量正常人相应组织的对照标本（*不是每种癌都有*），然后通过主流的高通量测序方法，获取基因乃至蛋白多个分子层面的数据；另一方面，它还收集了病人的临床宏观层面信息（诸如肿瘤的分期和分级，患者生存时间，患者的年龄、性别、种族等等），更进一步，该数据库对原始数据进行了标准化处理，并做了部分常见的后续功能分析，但缺乏一定的针对性。目前，它已经收录了超过10000名病人，30多种癌症的高达2.5PB的多维数据。**从2016年开始，TCGA的数据库已经迁移到[GDC (Genomic Data Commons, 基因组数据共享)](https://gdc.cancer.gov/)网站去了，官网称2017年TCGA将会关闭，距2018还有一个月不到，加油！**

    + 什么是癌旁组织？ ["Tumor, Matched Normal" Vs. "Normal, Matched Tumor"](https://www.biostars.org/p/86929/)
    
    + TCGA数据来源、分级及处理流程(pipeline)的具体信息请多去维基逛逛 [TCGA WIKI](https://wiki.nci.nih.gov/display/TCGA/TCGA+Wiki+Home)
    
    + 生命现象的一个重要特点就是多维性（多层面性），从分子层面直至生命个体，可以划分出数个大的层次，每个层次又包含诸多细分维度，你可能觉得TCGA已经很庞大、很全面了，但其实你还可以想到很多未被纳入的层面信息。说这些，目的是谈谈**GDC**的由来，它不仅是TCGA的新东家，它还左拥右抱了好几个大型数据库，如[TARGET (Therapeutically Applicable Research To Generate Effective Treatments)](https://ocg.cancer.gov/programs/target), [CGCI (Cancer Genome Characterization Initiative)](https://ocg.cancer.gov/programs/cgci)等等，这也许是今后的大趋势，**分类数据库的整合**，以期能让研究者一站式获取各维度信息，做出更准确的发现，所谓的精准医疗。*但我只想说，搞那么多数据，全是生信的活啊，不愁失业了。*
    
* [GDAC (Genome Data Analysis Centers)](https://cancergenome.nih.gov/abouttcga/overview/howitworks/dataanalysiscenters)：顾名思义，做数据分析的，TCGA Research Network下属的一个重要团体，成员中最为著名的就是Broad Institute的[Firehose（消防水龙）](http://firebrowse.org/)和Memorial Sloan-Kettering Cancer Center的[cbioportal](http://www.cbioportal.org/)了，简言之，它们对TCGA的原始数据进行了合并、部分处理及可视化，减少了研究人员前期数据处理的繁冗工作，极大提高了下游功能分析的效率。

* [DESeq2](https://bioconductor.org/packages/release/bioc/html/DESeq2.html)：著名的基因差异化表达(Defferential Expression, DE)搜索及可视化R包，本来不确定从哪个DE软件入手，陡然间发现，此包作者的实验室就在我们楼侧面，为了方便以后套磁，就选它了，*学了才发现水很深，我得补补统计学*。

## 有关数据

其实，全套的转录组(RNA-seq)分析得从拿到一手的fastq测序文件开始，比对、注释、计数、差异分析、功能分析。但TCGA的测序原始数据一来是属于level1的，个人没有权限下载，二来，即便下载了，个人电脑估计连存都存不下，更别说分析了。所以，我直接从Firehose的level3数据入手，相当于已经对转录本和基因都记好数了，下一步，直接进入差异分析。

关于如何下载TCGA数据，[这个帖子](http://www.biotrainee.com:8080/forum.php?mod=viewthread&tid=1696#lastpost)讲得相当详细了，所以，我是遇到了就讲下，不再系统赘述。

* 为啥用Firehose的数据呢？因为它已经把每种癌症的所有样本数据按数据种类合并到一个文件中去了，而GDC中是一个样本一个文件，我是个懒人，谢谢！

* Firehose提供了一个很简易的下载工具，[firehose_get](https://confluence.broadinstitute.org/display/GDAC/Download)，网页上有安装方法和使用举例，这个需要你的电脑有linux系统，至少要有一个[bash on windows](https://docs.microsoft.com/en-us/windows/wsl/about)。

* **说句题外话，强烈建议像我一样不知如何入门的新手们买本[The Biostar Handbook](https://www.biostarhandbook.com/), 100多元钱，完全物超所值，关键是其中包含手把手教你如何配置系统，如何装bash on windows，并一次性弄好生信分析常用的数十种软件，从此无忧！另外，推荐直接看英文版，完全四级词汇，文中乐趣多多，而个人认为[生信技能树论坛](http://www.biotrainee.com/)中对于本书的中文翻译恰恰翻丢了这种阅读的乐趣，反正英语是躲不掉的，刚好拿这本书练练阅读，一举两得~**

