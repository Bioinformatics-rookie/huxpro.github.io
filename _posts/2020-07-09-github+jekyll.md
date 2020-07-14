---
layout: post
title: "利用github和jekyll构建个人博客"
subtitle: "Using GitHub and Jekyll to build personal blog"
author: "zhouxiaozhao"
tags:
     - blog
---

我们知道，一个网站要能够在任何地方都能够被访问，那么需要部署到服务器上，但是对于我们来说，构建服务器的花销是巨大的。幸运的是，github就提供了这样的功能，只要按照github格式要求，新建一个仓库，把你的网站代码上传到里面，你就有一个自己的免费网站了。

废话不多说，让我们利用jekyll和github来构建自己的博客吧！！

---

# 一：软件下载

jekyll支持mac、linux、windows等，鉴于大部分童鞋使用的是windows系统，那我就用windows构建

## 1.下载安装ruby installer

https://rubyinstaller.org/ ，点击相应的版本进入下载页面即可下载

![image-20200709153231758](/img/posts/7.9-jekyll/image-20200709153231758.png)

## 2.下载rubygems

https://rubygems.org/pages/download ，可以选择相应版本下载

![image-20200709154144808](/img/posts/7.9-jekyll/image-20200709154144808.png)

下载完成后解压到你想放的位置，比如我放在了“D:/soft/rubygems3-1.4”下,打开cmd用命令执行

```R
D:
cd soft/rubygems3-1.4
ruby setup.rb
gem install jekyll
```

---

## 二：网站构建

软件下载有点耗时，在此期间我们可以挑选一个自己喜欢的主题来构建自己的博客

首先你要到[GitHub](http://github.com)上注册一个账号,例如jungleblack007（用户名可以在设置里改），创建完成后可以去[jekyll主题网站](http://jekyllthemes.org/)选择自己喜欢的主题，然后在这个主题基础上修改到满足自己需求的博客.

我选择的是[潘柏信](https://github.com/leopardpan/leopardpan.github.io)的博客主题，因为这个主题有中文说明，而且有个B站up主对该主题进行了详细的操作，适合我们入门，先让我们看一下该博客的外貌！

![image-20200709170946728](/img/posts/7.9-jekyll/image-20200709170946728.png)



我们找到作者的[源代码块](https://github.com/leopardpan/leopardpan.github.io),点击右上角的fork可以将它拷到我们自己的github中，顺便star一下支持作者

拷到自己的github后，我们点击setting，先进行改名，推荐：你的用户名.github.io

![image-20200709212928730](/img/posts/7.9-jekyll/image-20200709212928730.png)

然后下拉找到GitHub Pages，source选择master branch，我已经点了所以看不到，然后上方出现一个网址，这个就是你的域名了，可以先看看和原博客有没有区别

![image-20200709213017466](/img/posts/7.9-jekyll/image-20200709213017466.png)

![image-20200709213344986](/img/posts/7.9-jekyll/image-20200709213344986.png)

---

## 三：运行jekyll生成静态网页

把别人的主题fork以后，我们可以利用[atom](https://atom.io/)把主题代码转到自己的电脑上

打开atom，按ctrl+shift+p，输入github clone，选择要克隆的网址和要保存的路径。点clone即可

![image-20200709211103034](/img/posts/7.9-jekyll/image-20200709211103034.png)

clone完成后，就会有如下界面

![image-20200713145237970](/img/posts/7.9-jekyll/image-20200713145237970.png)

修改_config.xml,把一些信息修改为自己的

![image-20200713150055386](/img/posts/7.9-jekyll/image-20200713150055386.png)

图像也可以换，根据img的文件夹的图片名称可以换成你想要的，名字要一致，img/posts主要放置的是笔记中的图片，可以删掉，以后写笔记的时候想插入图片需要把图片路径设置成img/posts/XXXX。赞赏功能的图片在paying里，需要赞赏就改成自己的二维码，不需要赞赏就删掉new-old.html里的所有内容

![image-20200713150830556](/img/posts/7.9-jekyll/image-20200713150830556.png)

此时，我们可以用jekyll来生成一个静态网页查看

```
D
cd data/git
bundler install
bundle exec jekyll serve
```

http://127.0.0.1:4000 这是我们生成的一个静态页面，浏览器输入即可查看

![image-20200713151505404](/img/posts/7.9-jekyll/image-20200713151505404.png)

可以看到背景和个人信息都已经改为了自己的，说明修改成功！

---

## 四，写一篇自己的博客

jekyll比较好的一点是可以识别笔记文件，我们可以把写好的文件放到_post文件夹下，然后在md文件前面加一串代码。

![image-20200713152310147](/img/posts/7.9-jekyll/image-20200713152310147.png)

写好后可以再运行bundle exec jekyll serve查看自己写的博客是否已经能在静态网页上查看

![image-20200713152501584](/img/posts/7.9-jekyll/image-20200713152501584.png)

---

## 五：添加网站统计功能

我们可以给自己的网站添加统计功能，在这个主题中也支持该功能，推荐使用百度统计，首先注册账号

注册结束后添加新的域名统计

![image-20200713152912413](/img/posts/7.9-jekyll/image-20200713152912413.png)

添加结束后点击管理—代码获取，会看到一串代码，

```
<script>
var _hmt = _hmt || [];
(function() {
  var hm = document.createElement("script");
  hm.src = "https://hm.baidu.com/hm.js?60e2d4433f4c77b3ae5db5cac1b62829";
  var s = document.getElementsByTagName("script")[0];
  s.parentNode.insertBefore(hm, s);
})();
</script>

```

?后面的60e2d4433f4c77b3ae5db5cac1b62829可以粘贴替换掉_config.yml文件中的百度统计

![image-20200713153259817](/img/posts/7.9-jekyll/image-20200713153259817.png)

这样统计功能就添加上了

---

## 六：添加评论功能

评论功能也可以根据自己的喜好添加，我添加评论功能使用的是[来必力](http://livere.com/)

注册账号后添加网页，与百度统计类似，添加结束后点击管理界面-代码管理，可以看到一串代码

```
<!-- 来必力City版安装代码 -->
<div id="lv-container" data-id="city" data-uid="MTAyMC81MDkzOC8yNzQyMA==">
<script type="text/javascript">
   (function(d, s) {
       var j, e = d.getElementsByTagName(s)[0];

       if (typeof LivereTower === 'function') { return; }

       j = d.createElement(s);
       j.src = 'https://cdn-city.livere.com/js/embed.dist.js';
       j.async = true;

       e.parentNode.insertBefore(j, e);
   })(document, 'script');
</script>
<noscript>为正常使用来必力评论功能请激活JavaScript</noscript>
</div>
<!-- City版安装代码已完成 -->
```



将该代码复制，替换掉_include/comments.html中的全部内容即可（这里的bioinformatics-rookie.github.io是我自己的博客，文中出现的jungleblack007.github.io是我用来做试验的账号）

---



## 七：上传到github

所有内容修改完成后可以将自己的代码上传到github，atom也提供此功能。

当你所有的内容修改完毕后，atom右侧unstaged changes会显示你修改了什么内容,点击stage all,所有的会移动到下方的staged changes.

![image-20200713164302396](/img/posts/7.9-jekyll/image-20200713164302396.png)

在 commit message中随便写点东西点击下方按钮，在push出会出现一个待上传的指令

![image-20200713164434895](/img/posts/7.9-jekyll/image-20200713164434895.png)

ctrl+左键点击push，使用force push功能可以将自己修改过的内容传到github服务器上，这样输入你的github域名就可以打开自己的网页了，后续的更改同样采取此操作。

---

## 参考链接

[](https://github.com/leopardpan/leopardpan.github.io)

[](https://www.jianshu.com/p/9f71e260925d)

[](https://www.bilibili.com/video/BV14x411t7ZU?t=537)

---

转载请注明转载请注明：[周小钊的博客](https://www.zhouxiaozhao.cn)- [利用github和jekyll构建个人博客](https://bioinformatics-rookie.github.io/2020/07/09/github+jekyll/)
