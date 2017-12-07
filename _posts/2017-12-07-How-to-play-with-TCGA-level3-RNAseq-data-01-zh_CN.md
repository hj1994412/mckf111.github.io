
<!-- rnb-text-begin -->

---
layout: post
title: "TCGA����ս������������RNA-seq����01"
date: 2017-12-07
categories: Battlefield_TCGA(����)
tags: TCGA R DESeq2 firehose RNA-seq
author: Wenhu
mathjax: true
output: 
  html_document:
    keep_md: true
---

��ƪΪ��һ���֣���Ҫ��¼��Ҫ��Դ��ַ�Լ�TCGA���ݵ����ط�ʽ��




## ���ʼ���Դ
******

* [TCGA, The Cancer Genome Atlas](https://cancergenome.nih.gov/)��**����**��֢������ͼ�ף�һ�����ݿ⣬��Ҫ�����ռ���֢���˰���֯��������֯�걾�Լ���������������Ӧ��֯�Ķ��ձ걾��*����ÿ�ְ�����*����Ȼ��ͨ�������ĸ�ͨ�����򷽷�����ȡ�����������׶�����Ӳ�������ݣ���һ���棬�����ռ��˲��˵��ٴ���۲�����Ϣ�����������ķ��ںͷּ�����������ʱ�䣬���ߵ����䡢�Ա�����ȵȣ�������һ���������ݿ��ԭʼ���ݽ����˱�׼�����������˲��ֳ����ĺ������ܷ�������ȱ��һ��������ԡ�Ŀǰ�����Ѿ���¼�˳���10000�����ˣ�30���ְ�֢�ĸߴ�2.5PB�Ķ�ά���ݡ�**��2016�꿪ʼ��TCGA�����ݿ��Ѿ�Ǩ�Ƶ�[GDC, Genomic Data Commons](https://gdc.cancer.gov/)(���������ݹ���)��վȥ�ˣ�������2017��TCGA����رգ���2018����һ���²��������ͣ�**

    + ʲô�ǰ�����֯�� ["Tumor, Matched Normal" Vs. "Normal, Matched Tumor"](https://www.biostars.org/p/86929/)
    
    + TCGA������Դ���ּ�����������(pipeline)�ľ�����Ϣ���ȥά����� [TCGA WIKI](https://wiki.nci.nih.gov/display/TCGA/TCGA+Wiki+Home)
    
    + ���������һ����Ҫ�ص���Ƕ�ά�ԣ�������ԣ����ӷ��Ӳ��棨��ʵ�Ѿ�����������ѧ�ˣ�ֱ���������壬���Ի��ֳ�������Ĳ�Σ�ÿ������ְ������ϸ��ά�ȣ����������TCGA�Ѿ����Ӵ󡢺�ȫ���ˣ���ʵ�㻹�����뵽�ܶ�δ������Ĳ�����Ϣ����˵��Щ��Ŀ����̸̸**GDC**����������������TCGA���¶��ң�������ӵ�ұ��˺ü����������ݿ⣬��[TARGET,Therapeutically Applicable Research To Generate Effective Treatments](https://ocg.cancer.gov/programs/target), [CGCI, Cancer Genome Characterization Initiative](https://ocg.cancer.gov/programs/cgci)�ȵȣ���Ҳ���ǽ��Ĵ����ƣ�**�������ݿ������**�����������о���һվʽ��ȡ��ά����Ϣ��������׼ȷ�ķ��֣���ν�ľ�׼ҽ�ơ�����ֻ��˵������ô�����ݣ���TNȫ�����ŵĻ������ʧҵ�ˡ�
    
* [GDAC, Genome Data Analysis Centers](https://cancergenome.nih.gov/abouttcga/overview/howitworks/dataanalysiscenters)������˼�壬�����ݷ����ģ�TCGA Research Network������һ����Ҫ���壬��Ա����Ϊ�����ľ���Broad Institute��[Firehose������ˮ����](http://firebrowse.org/)��Memorial Sloan-Kettering Cancer Center��[cbioportal](http://www.cbioportal.org/)�ˣ�����֮�����Ƕ�TCGA��ԭʼ���ݽ����˺ϲ������ִ������ӻ����������о���Աǰ�����ݴ���ķ��߹�����������������ι��ܷ�����Ч�ʡ�

* [DESeq2](https://bioconductor.org/packages/release/bioc/html/DESeq2.html)�������Ļ�����컯���(Defferential Expression, DE)���������ӻ�R����������ȷ�����ĸ�DE������֣���Ȼ�䷢�֣��˰����ߵ�ʵ���Ҿ�������¥���棬Ϊ�˷����Ժ��״ţ���ѡ���ˣ�*ѧ�˲ŷ���ˮ����ò���ͳ��ѧ*��

## �й�����
******
��ʵ��ȫ�׵�ת¼��(RNA-seq)�����ô��õ�һ�ֵ�fastq�����ļ���ʼ���ȶԡ�ע�͡�������������������ܷ�������TCGA�Ĳ���ԭʼ����һ��������level1�ģ�����û��Ȩ�����أ����������������ˣ����˵��Թ������涼�治�£�����˵�����ˡ����ԣ���ֱ�Ӵ�Firehose��level3�������֣��൱���Ѿ���ת¼���ͻ��򶼼Ǻ����ˣ���һ����ֱ�ӽ�����������

�����������TCGA���ݣ�[�������](http://www.biotrainee.com:8080/forum.php?mod=viewthread&tid=1696#lastpost)�����൱��ϸ�ˣ����ԣ����������˾ͽ��£�����ϵͳ׸����

* Ϊɶ��Firehose�������أ���Ϊ���Ѿ���ÿ�ְ�֢�������������ݰ���������ϲ���һ���ļ���ȥ�ˣ���GDC����һ������һ���ļ������Ǹ����ˣ�лл��

* Firehose�ṩ��һ���ܼ��׵����ع��ߣ�[firehose_get](https://confluence.broadinstitute.org/display/GDAC/Download)����ҳ���а�װ������ʹ�þ����������Ҫ��ĵ�����linuxϵͳ������Ҫ��һ��[bash on windows](https://docs.microsoft.com/en-us/windows/wsl/about)��

* **˵�����⻰��ǿ�ҽ�������һ����֪������ŵ���������[The Biostar Handbook](https://www.biostarhandbook.com/), 100��ԪǮ����ȫ�ﳬ��ֵ���ؼ������а����ְ��ֽ����������ϵͳ����һ����װ�����ŷ������õ���ʮ��������Ӵ����ǣ����⣬��ֱ�ӿ�Ӣ�İ棬��ȫ�ļ��ʻ㣬������Ȥ��࣬��������Ϊ[���ż�����](http://www.biotrainee.com/)�ķ���ǡǡ�����������Ķ�����Ȥ��**


<!-- rnb-text-end -->

