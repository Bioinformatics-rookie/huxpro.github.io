---
layout: post
title: "炫酷你的Github主页"
subtitle: "Cool your Github"
author: "zhouxiaozhao"
catalog: true
tags:
     - github
---



## 1.在github上新建一个仓库并clone到本地

注意： 新仓库的名字要与你的登录账号名保持一致（我这里已经创建过所以有重名警告）

![image-20200929135310913](\img\posts\2020.9.22\image-20200929135310913.png)

我比较喜欢使用atom来进行clone，所以我选择了添加README文件，如果喜欢用git可以不添加README文件，按照如下命令执行

```
进入一个文件夹
git clone git@github.com:你的用户名/你的用户名.git
echo "# 主页展示的内容" >>README.md
git init
git add .
git commit -m "first commit"
git remote add origin git@github.com:你的用户名/你的用户名.git
git push -u origin master
```

使用atom进行clone，ctrl+shift+p输入github clone，将主页网址clone到本地

![image-20200929140904438](/img/posts/2020.9.22/image-20200929140904438.png)

![image-20200929141047285](/img/posts/2020.9.22/image-20200929141047285.png)

此时你的主页也出现了一个界面，个性化设置主要是修改这个README来使的主页显示相应的内容

![image-20200929141416709](../img/posts/2020.9.22/image-20200929141416709.png)

## 修改README文件

其实接下来的步骤就因人而异了，修改自己的README文件，变成自己想要的模式吧，分享一下自己的界面[https://github.com/Bioinformatics-rookie](https://github.com/Bioinformatics-rookie)

![image-20200929154053868](/img/posts/2020.9.22/image-20200929154053868.png)

再给大家推荐一个[Github的主页](https://github.com/Rishit-dagli)，我个人感觉挺好看的，原文件在我的项目里example.md

![image-20200929154426301](/img/posts/2020.9.22/image-20200929154426301.png)