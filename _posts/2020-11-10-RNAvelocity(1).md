---
layout: post
title: "RNA速率：软件下载与loom文件准备"
subtitle: "RNA velocity(1)"
author: "zhouxiaozhao"
catalog: true
tags:
     - bioinformatics
     - ScRNAseq
     - RNA_velocity
---





参考链接:
[https://github.com/velocyto-team/velocyto.R](https://github.com/velocyto-team/velocyto.R)

[http://velocyto.org/velocyto.py/index.html](http://velocyto.org/velocyto.py/index.html)

[http://pklab.med.harvard.edu/velocyto/notebooks/R/chromaffin2.nb.html](http://pklab.med.harvard.edu/velocyto/notebooks/R/chromaffin2.nb.html)

[https://htmlpreview.github.io/?https://github.com/satijalab/seurat-wrappers/blob/master/docs/velocity.html](https://htmlpreview.github.io/?https://github.com/satijalab/seurat-wrappers/blob/master/docs/velocity.html)

[https://github.com/velocyto-team/velocyto.R/issues/16](https://github.com/velocyto-team/velocyto.R/issues/16)

[https://www.jianshu.com/p/bce19672879e](https://www.jianshu.com/p/bce19672879e)

## 目录

[RNA速率:软件下载与loom文件准备](https://www.zhouxiaozhao.cn/2020/11/10/RNAvelocity(1)/)

[RNA速率：数据读入](https://www.zhouxiaozhao.cn/2020/11/12/RNAvelocity(2)/)

[RNA速率：使用Seurat的结果做RNA velocity](https://www.zhouxiaozhao.cn/2020/11/14/RNAvelocity(3)/)

## velocyto下载

首先是下载velocyto生成loom文件

```
## 1. 创建python>3.6的环境
conda create -n velocyto python=3.6
## 2. 安装前置软件
conda install numpy scipy cython numba matplotlib scikit-learn h5py click
pip install pysam
## 3. 安装velocyto
pip install velocyto
## 4. 测试
velocyto --help
Usage: velocyto [OPTIONS] COMMAND [ARGS]...

Options:
  --version  Show the version and exit.
  --help     Show this message and exit.

Commands:
  run            Runs the velocity analysis outputting a loom file
  run10x         Runs the velocity analysis for a Chromium Sample
  run-dropest    Runs the velocity analysis on DropEst preprocessed data
  run-smartseq2  Runs the velocity analysis on SmartSeq2 data (independent bam file per cell)
  tools          helper tools for velocyto
```

## repeat_masker.gtf生成

运行velocyto需要准备三个文件，单细胞数据分析的结果文件，基因组注释文件，重复序列注释文件，其中前两个在单细胞分析时就会得到，关键是这个repeat_masker.gtf

本人是做植物的，所以本次教程主要关注植物类repeat_masker.gtf的获得，人和小鼠的重复序列文件比较好得到，植物类首先可以看一下Phtozome数据库上想要研究的物种的注释文件夹下有没有reapeat.gtf，没有就要我们自己生成了

![image-20201124191831469](/img/posts/2020.11.10/image-20201124191831469.png)

重复序列的注释文件我们一直没怎么关注过，对于做过基因组注释的童鞋来说，大家都忽略了一步，其实repeatmasker就可以生成重复序列的注释文件。

```
RepeatMasker -e ncbi -species arabidopsis -pa 40 -gff  TAIR10.fa
```

生成gff文件后，可以看到重复序列的点位以及属性，velocyto主要使用的就是位点

![image-20201124191648003](/img/posts/2020.11.10/image-20201124191648003.png)

## loom文件生成

接下来是生成loom文件，运行velocyto需要准备三个文件，基因组注释文件(gtf)，repeat_masker.gtf(重复序列注释文件)，cellranger的结果文件夹(以样本名WT_1为例，里面包含cell matrix和bam文件)

```
velocyto -m TAIR10_masked.gtf WANG/ TAIR10.gtf
```

运行结束后会在WANG文件夹下生成velocyto文件夹，里面有velocyto.loom的文件，可以用于下一步的分析
