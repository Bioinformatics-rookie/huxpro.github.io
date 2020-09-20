# RNAvelocity

参考链接:https://github.com/velocyto-team/velocyto.R

​              http://velocyto.org/velocyto.py/index.html

​              http://pklab.med.harvard.edu/velocyto/notebooks/R/chromaffin2.nb.html

​              https://htmlpreview.github.io/?https://github.com/satijalab/seurat-wrappers/blob/master/docs/velocity.html

​              https://github.com/velocyto-team/velocyto.R/issues/16

## 10X单细胞数据生成loom文件

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

接下来是生成loom文件，运行velocyto需要准备三个文件，基因组注释文件(gtf)，repeat_masker.gtf(重复序列注释文件)，cellranger的结果文件夹(以样本名WT_1为例，里面包含cell matrix和bam文件)

```
velocyto -m Ppatens_masked.gtf WT_1/ Ppatens.gtf
```

运行结束后会在WT_1文件夹下生成velocyto文件夹，里面有velocyto.loom的问价，可以用于下一步的分析

## velocyto.R的安装

接下来，将使用velocyto.R对loom文件进行分析，首先要安装R包

```
library(devtools)
install_github("velocyto-team/velocyto.R")
```

## 数据读入

```
>library(velocyto.R)
>setwd("/datadisk02/PP_cleandata")
>wt1<-read.loom.matrices("data/WT_1/velocyto/WT_1.loom")
reading loom file via hdf5r...
> str(wt1)
List of 3
 $ spliced  :Formal class 'dgCMatrix' [package "Matrix"] with 6 slots
  .. ..@ i       : int [1:29461442] 0 1 8 22 24 26 31 33 37 42 ...
  .. ..@ p       : int [1:7074] 0 3838 10389 11272 14043 19053 19704 21047 23662 32215 ...
  .. ..@ Dim     : int [1:2] 26233 7073
  .. ..@ Dimnames:List of 2
  .. .. ..$ : chr [1:26233] "Pp3c1_120" "Pp3c1_140" "Pp3c1_145" "Pp3c1_190" ...
  .. .. ..$ : chr [1:7073] "WT_1:AAACGAAGTTCACCGGx" "WT_1:AAACGCTGTATTCCTTx" "WT_1:AAAGTCCCATGGGTCCx" "WT_1:AAAGTCCTCTAGTACGx" ...
  .. ..@ x       : num [1:29461442] 27 2 23 1 1 1 5 1 9 4 ...
  .. ..@ factors : list()
 $ unspliced:Formal class 'dgCMatrix' [package "Matrix"] with 6 slots
  .. ..@ i       : int [1:3609298] 8 49 50 133 141 167 190 192 214 279 ...
  .. ..@ p       : int [1:7074] 0 313 1280 1338 1545 2088 2120 2241 2432 3671 ...
  .. ..@ Dim     : int [1:2] 26233 7073
  .. ..@ Dimnames:List of 2
  .. .. ..$ : chr [1:26233] "Pp3c1_120" "Pp3c1_140" "Pp3c1_145" "Pp3c1_190" ...
  .. .. ..$ : chr [1:7073] "WT_1:AAACGAAGTTCACCGGx" "WT_1:AAACGCTGTATTCCTTx" "WT_1:AAAGTCCCATGGGTCCx" "WT_1:AAAGTCCTCTAGTACGx" ...
  .. ..@ x       : num [1:3609298] 142 1 2 1 1 1 1 2 1 1 ...
  .. ..@ factors : list()
 $ ambiguous:Formal class 'dgCMatrix' [package "Matrix"] with 6 slots
  .. ..@ i       : int [1:1596138] 8 49 190 549 575 635 876 1076 1293 1298 ...
  .. ..@ p       : int [1:7074] 0 170 568 596 674 928 949 998 1094 1609 ...
  .. ..@ Dim     : int [1:2] 26233 7073
  .. ..@ Dimnames:List of 2
  .. .. ..$ : chr [1:26233] "Pp3c1_120" "Pp3c1_140" "Pp3c1_145" "Pp3c1_190" ...
  .. .. ..$ : chr [1:7073] "WT_1:AAACGAAGTTCACCGGx" "WT_1:AAACGCTGTATTCCTTx" "WT_1:AAAGTCCCATGGGTCCx" "WT_1:AAAGTCCTCTAGTACGx" ...
  .. ..@ x       : num [1:1596138] 40 3 1 1 1 1 1 1 1 2 ...
  .. ..@ factors : list()

```

合并三个重复（我的是有三个重复，需要合并再分析，因为在跑Seurat的时候就合并了三个重复）

```
>WT <- vector( mode = "list" )
>x <- cbind(wt1$spliced,wt2$spliced)
>WT$spliced<-cbind(x,wt3$spliced)
>y <- cbind(wt1$unspliced,wt2$unspliced)
>WT$unspliced<-cbind(y,wt3$unspliced)
>z<-cbind(wt1$ambiguous,wt2$ambiguous)
>WT$ambiguous(z,wt3$ambiguous)
save(WT,file="WT-loom.rds")
```

## 用Seurat做RNA Velocity

利用原有的UMAP(tSNE)标注RNA velocity

```
>library(Seurat)
## remotes::install_github('satijalab/seurat-wrappers')
>library(SeuratWrappers)
## 导入Seurat对象,之前分析的结果
>load("WT.rds")
```

统一loom对象和Seurat的细胞名与基因名

```
>colnames(WT$spliced)<-gsub("_","",colnames(WT$spliced))
>colnames(WT$spliced)<-gsub(":","_",colnames(WT$spliced))
>colnames(WT$spliced)<-gsub("x","-1",colnames(WT$spliced))
>colnames(WT$unspliced)<-gsub("_","",colnames(WT$unspliced))
>colnames(WT$unspliced)<-gsub(":","_",colnames(WT$spliced))
>colnames(WT$unspliced)<-gsub("x","-1",colnames(WT$unspliced))
>colnames(WT$ambiguous)<-gsub("_","",colnames(WT$ambiguous))
>colnames(WT$ambiguous)<-gsub(":","_",colnames(WT$ambiguous))
>colnames(WT$ambiguous)<-gsub("x","-1",colnames(WT$ambiguous))
>rownames(WT$spliced)<-gsub("_","-",rownames(WT$spliced))
>rownames(WT$unspliced)<-gsub("_","-",rownames(WT$unspliced))
>rownames(WT$ambiguous)<-gsub("_","-",rownames(WT$ambiguous))
## 其实也可以只修改一个的行名和列名，让其他的矩阵的行名与列名等于修改之后的，保险起见每个都进行修改
```

提取spliced与unspliced文件，并提取原有的Seurat的UAMP图

```
## 由于Seurat的对象筛选了数据，所以两个文件细胞并不相同，以Seurat对象为准
> WT$spliced<-WT$spliced[,c(rownames(PPdata@meta.data))]
> WT$unspliced<-WT$unspliced[,c(rownames(PPdata@meta.data))]
> WT$ambiguous<-WT$ambiguous[,c(rownames(PPdata@meta.data))]
> save(WT,file="WT-loom.rds")
> sp <- WT$spliced
> unsp <- WT$unspliced
> WTumap <- PPdata@reductions$umap@cell.embeddings
## 估计细胞和细胞的距离
cell.dist <- as.dist(1-armaCor(t(seurat.object@reductions$umap@cell.embeddings)))
fit.quantile <- 0.02
rvel.cd <- gene.relative.velocity.estimates(sp,unsp,deltaT=2,kCells=10, cell.dist=cell.dist,fit.quantile=fit.quantile,n.cores=24)

save(WT,rvel.cd,file="WT-loom1.rds")
```

## 提取原seurat对象的UMAP图的细胞颜色

```
p1 <- show.velocity.on.embedding.cor(WTumap,rvel.cd,n=30,scale='sqrt',cell.colors=ac(colors,alpha=0.5),cex=0.8,arrow.scale=2,show.grid.flow=T,min.grid.cell.mass=1.0,grid.n=50,arrow.lwd=1,do.par=F,cell.border.alpha = 0.1,n.cores=24,main="Cell Velocity")#,cc=p1$cc)
```

