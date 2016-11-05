---
layout: post
title: "Scientific Linux 下用Octopress免费静态博客系统在Github上搭建个人网站"
date: 2015-08-10 20:03:52 +0800
comments: true
tags: [Linux,Software,Octopress,Github]
---

本教程讲解在Scientific Linux 下用Octopress免费静态博客系统在Github上搭建个人网站。这里只是说说怎么建立网站，发布到Github上还需要Github的使用技能。如果不想使用默认的模板，还得取下载模板来使用或者增加写模板。这些技能网上都很多的。
<!--more-->
## 必备软件安装
我网盘的软件：
[下载 autoconf-2.67](http://share.weiyun.com/d036ee46d3d059db86b043dfc3bb6b35)
[下载 ruby-2.2.2](http://share.weiyun.com/4ebfe8e205851608acf17184725d6302)

系统自带的 ruby ，因为其版本太低
自己去下载 ruby 高一点的版本，我安装的是2.2.2。安装在/usr下   ./configure --prefix=/usr
ruby2.2.2 要求 autoconf 版本2.67及以上，安装在/usr下     ./configure --prefix=/usr

然后通过 yum 安装软件 nodejs
~~~
# yum install nodejs
~~~

## 安装octopress
可以下载我网盘的版本[下载 octopress-master.zip](http://share.weiyun.com/bec313a85a8f635e3bfa47a1dc2733aa)

也可以直接在Github上下载octopress源码。
~~~
git clone git://github.com/imathis/octopress.git  octopress
~~~
就是去下来放在你想要放的地方

然后在该文件夹內，替换镜像源网站
~~~
$  gem sources -a https://ruby.taobao.org/
$  gem sources -r http://rubygems.org/
$  gem -l
~~~
将文件Gemfile的第一行网址改为 https://ruby.taobao.org


然后在该文件夹內执行
~~~
# gem install bundler    //安装bundler
$ bundle install
$ rake install           //安装octopress默认主题
~~~

然后就是生成和预览博客了
~~~
$ rake generate
$ rake preview
~~~
在浏览器中输入 localhost:4000 回车等待就可预览网页了。


如果要支持在网页中插入Latex数学公式，那么还需要做以下配置：
在 source/_includes/custom/footer.html文件中加入以下：
~~~
<!-- mathjax config similar to math.stackexchange -->
<script type="text/x-mathjax-config">
  MathJax.Hub.Config({
    tex2jax: {
      inlineMath: [ ['$','$'], ["\\(","\\)"] ],
      processEscapes: true
    }
  });
</script>
<script type="text/x-mathjax-config">
    MathJax.Hub.Config({
      tex2jax: {
        skipTags: ['script', 'noscript', 'style', 'textarea', 'pre', 'code']
      }
    });
</script>
<script type="text/x-mathjax-config">
    MathJax.Hub.Queue(function() {
        var all = MathJax.Hub.getAllJax(), i;
        for(i=0; i < all.length; i += 1) {
            all[i].SourceElement().parentNode.className += ' has-jax';
        }
    });
</script>
<script type="text/javascript"
   src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML">
</script>
~~~

然后将_config.yml 中的 markdown: rdiscount 改为 markdown: kramdown

## 发布
发布命令
首次使用配置，在文件夹内执行以下第一句，并按要求输入第二句：
~~~
rake setup_github_pages
url: git@github.com:username/username.github.io.git
~~~
然后使用以下语句就能发布了。
~~~
rake deploy
~~~

<br />
----
&#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160;**Update: 6. Last-Modified: 2015-09-27 21:02**
