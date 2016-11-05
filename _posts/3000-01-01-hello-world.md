---
layout: post
title: Hello ! Read Me.
date: 3000-01-01 00:00:00 +0800
comments: true
tags: [Linux,Geant4-ROOT,Nuclearphysics]
---
&#160; &#160; &#160; &#160;**请确保连接国际网下访问本站，教育网免费IP下访问可能存在部分内容无法正常显示(数学公式、底下评论框等)。在Chrome浏览器下访问效果更佳！评论支持匿名（有个选项打上勾），无须注册相关帐号即可评论。**  
Welcome to Hongyi Wu(吴鸿毅) blog! If you have any problems when using my blog, you can ask me for email: wuhongyi@qq.com / wuhongyi@pku.edu.cn.
<!--more-->

<!-- ## Tags -->

<!-- [Linux](/tags/Linux/index.html) -->
<!-- [Software](/tags/Software/index.html) -->
<!-- [Geant4-ROOT](/tags/Geant4-ROOT/index.html) -->
<!-- [Nuclearphysics](/tags/Nuclearphysics/index.html) -->


&#160; &#160; &#160; &#160;1987 年 9 月，CANET（中国学术网）在北京计算机应用技术研究所内正式建成中国第一个国际互联网电子邮件节点，并于 9 月 14 日发出了中国第一封电子邮件：“Across the Great Wall we can reach every corner in the world.(越过长城，走向世界)”，揭开了中国人使用互联网的序幕。这封电子邮件是通过意大利公用分组网 ITAPAC 设在北京的 PAD 机，经由意大利 ITAPAC 和德国 DATEX-P 分组网，实现了和德国卡尔斯鲁厄大学的连接，通信速率最初为 300bps。现在我们时常能感受到了“越过长城走向世界"里面的那个长城的伟大。中国的前辈们很有先见之明，在 N 年前已经知道了后辈们将会遭遇什么，并以神的口吻告诉我们“越过长城，走向世界”。

## 常用配置

[Hongyi Wu Github](https://github.com/wuhongyi)
下载
[yum SL6 SL7](https://github.com/wuhongyi/ScientificLinuxYumSet)
[emacs linux](https://github.com/wuhongyi/EmacsSet)
[emacs windows](https://github.com/wuhongyi/EmacsSet)
[linux latex fonts](https://github.com/wuhongyi/fonts) 
[cmake template](https://github.com/wuhongyi/cmakeTemplate)
[latex template](https://github.com/wuhongyi/LatexTemplate)


## 本网站由 Hexo 搭建
Hexo新建博客：
为了预览效果，本地得安装 mathjax
安装 npm nodejs
~~~
# npm install -g hexo
$ npm init Hexo   //建立文件夹
$ cd Hexo
$ npm install    //这是使用默认模板，可以下载额外的模板来安装
~~~

常用命令
~~~
hexo clean
hexo g  //生成
hexo s  //预览 localhost:4000
hexo deploy  //发布
~~~

----
执行hexo server提示找不到该指令(在Hexo 3.0 后server被单独出来了，需要安装server):
~~~
npm install hexo-server --save
~~~

----
部署提示找不到git(在Hexo 3.0版本后deploy git 被分开的，所以需要安装):
~~~
npm install hexo-deployer-git --save
~~~

----
部署的时候执行：hexo deploy 命令行没有任何输出，也没有错误。 
解决办法： 
在部署的_config.yml文件中，找到deploy:标签，在每个冒号后面必须要空格，否则就会出现上述问题。我的配置如下：
~~~
deploy:
  type: git
  repository: git@github.com:wuhongyi/wuhongyi.github.io.git
  branch: master
~~~

## gitbook 制作
得先安装 npm nodejs
```
# npm install -g gitbook-cli
$ gitbook -V
```

新建 gitbook

```
//在当前路径下新建Gitbook文件夹，并在里面生成README.md SUMMARY.md 文件
gitbook init Gitbook
gitbook serve [-p port] <Pathto>/Gitbook
```


<br />
----
&#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160;**Update: 3. Last-Modified: 2016-08-03 18:27**
