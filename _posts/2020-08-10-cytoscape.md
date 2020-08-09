---
layout: post
title: "基因共表达网络图的绘制"
subtitle: "ploting of gene co-expression networks "
author: "zhouxiaozhao"
catalog: true
tags:
   - bioinformatics
   - cytoscape
---



前面的教程已经进行了WGCNA分析并将各个模块的网络图绘制文件导出，接下来将进行共表达网络的绘制，本次教程依旧根据[PlantTech的Cytoscape课程](https://ke.qq.com/course/858014)编写，希望对大家有所帮助。

WGCNA教程

* [WGCNA分析原理](https://www.zhouxiaozhao.cn/2020/08/03/WGCNA1/)
* [WGCNA应用场景](https://www.zhouxiaozhao.cn/2020/08/05/WGCNA2/)
* [WGCNA分析实践](https://www.zhouxiaozhao.cn/2020/08/08/WGCNA3/)

# 1.数据导入

WGCNA分析后有许多模块，我选择上次分析中与Luminal 亚型最相关的pink模块进行分析。

打开cytoscape， 选择File>import>import network from file,打开WGCNA分析中得到的CytoscapeInput-edges-pink.txt文件，可以看到如下界面

![image-20200809153614145](img\posts\2020.8.10\image-20200809153614145.png)

接下来我们需要修改每个表头（每一列的属性），改成如下即可

![image-20200809153920044](img\posts\2020.8.10\image-20200809153920044.png)



![image-20200809154201219](img\posts\2020.8.10\image-20200809154201219.png)

可以看到做出来的网络非常丑，不着急，下一步进行网络的修改，美化一下

# 2.修改网络

点击Tools>networkanalyzer>network analysis>analyze network弹出以下界面

![image-20200809154808164](img\posts\2020.8.10\image-20200809154808164.png)

选择第二个的无方向网络，接着选择node degree distribution，点击下方的Visualize Parameters。

![image-20200809155054787](img\posts\2020.8.10\image-20200809155054787.png)

出现四个选择项：上边两个是调节node的，下边两个是调节edge的，大家可以根据自己的需要去选择。

![image-20200809155417958](img\posts\2020.8.10\image-20200809155417958.png)

![image-20200809155457756](img\posts\2020.8.10\image-20200809155457756.png)

可以看到所有的node已经根据节点数的多少有不同的大小。

在最下方的表格中将数据按照节点数目排序，将大于100的node选出，在name一列选择相应的node然后右键select node from selected rows可以将想要的node选出，然后拖拽到合适的位置。

![image-20200809155908673](img\posts\2020.8.10\image-20200809155908673.png)

选中degree大于100的所有点，点击layout>circular layout>select node only ,将选中的nodes按照圆形排列

![image-20200809160436454](img\posts\2020.8.10\image-20200809160436454.png)

出现了奇怪的图，点击layout>setting,设置Circle size的大小，可以设置圆的大小。

![image-20200809160617035](img\posts\2020.8.10\image-20200809160617035.png)

![image-20200809160724561](img\posts\2020.8.10\image-20200809160724561.png)

最后，我们再修改一下node和egde的颜色

左侧的设置栏我们可以修改node的颜色以及标签的颜色

![image-20200809160924680](img\posts\2020.8.10\image-20200809160924680.png)

然后点击下方的edge，修改edge的状态。在做WGCNA分析时，不同基因的之间的相关度有一个weight值，我们可以根据weight值的大小判断两个基因之间的相关性。

将Edge color to arrows打钩，选择最上方的Color（Unselected）选项。column选择weight，mapping type选择continuous mapping选项

![image-20200809161342472](img\posts\2020.8.10\image-20200809161342472.png)

点击那个渐变的色块，我们可以修改颜色

![image-20200809161513817](img\posts\2020.8.10\image-20200809161513817.png)

点击上面的三个小箭头，然后点击下方的Edge Color，可以对不同位置的颜色进行修改。

最后放上成品图，可能有点丑，大家根据自己的需要自行修改即可

![image-20200809161715090](img\posts\2020.8.10\image-20200809161715090.png)

# 3.数据导出

最后，我们可以将网络的表格文件和网络图导出

首先导出网络表格，点击最右侧的export，将表格文件导出至想要的位置即可![image-20200809161832004](img\posts\2020.8.10\image-20200809161832004.png)

接下来是导出图片，选择file>export>network to image,选择想要的图片文件格式，需要注意，保存图片时其实是对主界面进行一个截图操作，先将网络调成自己想要的大小然后再导出即可

![image-20200809162141042](img\posts\2020.8.10\image-20200809162141042.png)

最后我们也可以将图例导出，选择create legend，保存想要的图例格式即可

![image-20200809162301105](img\posts\2020.8.10\image-20200809162301105.png)

# 后记

该课程主要借鉴了[PlantTech的Cytoscape课程](https://ke.qq.com/course/858014)编写，视频我已下载，大家有需要可以去[百度云](https://pan.baidu.com/s/15IxHHWJ7t1uvhc8QSVykEQ)下载（提取码：5z41）

视频压缩包解压密码是我博客about界面下的一行小字（出自《蒲公英女孩》，标点是英文），如果链接炸了去博客找我联系方式。

网络图绘制教程个人感觉还是用视频教程更好，目前有录制视频的打算，录制好之后我会放到B站，主要还是看自己的时间允不允许。