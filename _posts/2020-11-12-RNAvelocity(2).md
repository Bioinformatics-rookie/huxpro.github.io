---
layout: post
title: "RNA速率：数据读入"
subtitle: "RNA velocity(2)"
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

[https://www.cnblogs.com/raisok/p/12425258.html](https://www.cnblogs.com/raisok/p/12425258.html)

## 目录

[RNA速率:软件下载与loom文件准备](https://www.zhouxiaozhao.cn/2020/11/10/RNAvelocity(1)/)

[RNA速率：数据读入](https://www.zhouxiaozhao.cn/2020/11/12/RNAvelocity(2)/)

[RNA速率：使用Seurat的结果做RNA velocity](https://www.zhouxiaozhao.cn/2020/11/14/RNAvelocity(3)/)

## velocyto.R的安装

loom文件生成后，接下来将使用velocyto.R对loom文件进行分析

```
library(devtools)
install_github("velocyto-team/velocyto.R")
```

## 数据读入

```
>library(velocyto.R)
>setwd("/datadisk02/Jiawei_Wang_2019")
>wt<-read.loom.matrices("WANG/velocyto/WANG.loom")
reading loom file via hdf5r...
>str(wt1)
List of 3
 $ spliced  :Formal class 'dgCMatrix' [package "Matrix"] with 6 slots
 $ unspliced:Formal class 'dgCMatrix' [package "Matrix"] with 6 slots
 $ ambiguous:Formal class 'dgCMatrix' [package "Matrix"] with 6 slots
>save(wt,file="WT-loom.rds")
>emat<-WT$spliced
#做直方图查看数据分步
>hist(log10(colSums(emat)),col='wheat',xlab='cell size')
```

![image-20201125191718068](/img/posts/2020.11.12/image-20201125191718068.png)

后续过程我想用Seurat的聚类进行分析，所以velocyto.R的聚类就不运行了