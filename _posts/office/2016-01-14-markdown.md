---
layout: post
title: "Linux 下 markdown 生成html、pdf"
date: 2016-1-14 10:16:16 +0800
comments: true
tags: [Linux,markdown]
---

在Linux[^Footnote]系统中，编辑markdown可以用retext工具，但是其需要较高版本的python，所以没能使用。

~~~
retext name.md
~~~
<!--more-->
要将markdown文件转换成html文件，可以用discount或python-markdown软件包提供的markdown，系统中默认安装的python-markdown好像存在问题，所以安装discount。

### 先卸载系统中的python-markdown
~~~
yum remove python-markdown.noarch
~~~

### 然后安装discount
~~~
yum install discount.x86_64 discount-devel.x86_64
~~~

### 用discount提供的markdown工具
~~~
markdown -o name.html name.md
~~~

将生成的html用网页打开，利用浏览器就能将其存成pdf格式了。

# 利用Pandoc将markdown文件转化为pdf
pandoc是能够在几十种文件格式之间自如的转换，得依赖各种格式文件所需要库。

### pandoc安装
~~~
yum install pandoc.x86_64
~~~

如果markdown文件中不包含中文字符，那么直接使用下面的命令就可以将markdown文件无缝转换为Latex支持的pdf文件。

~~~
pandoc name.md -o name.pdf
~~~

英文pdf的输出

~~~
pandoc name.md -o name.pdf --latex-engine=xelatex
~~~

为了解决中文编译的问题，需要做以下的工作：

- 将markdown文档的编码方式改为utf-8。使用emacs默认就是UTF-8编码的。
- 编译pandoc默认的latex引擎是pdflatex，是不支持中文的，因此需要手动设置编译时所用的引擎为xelatex。
- 这时编译可能没有错误了，但是得到的pdf文档中可能所有的中文都没有了。这是字体的问题，因为编译时默认的字体时不支持中文的，所以我们得手动设置中文字体。显然，所设的字体应该为系统中已装的字体，且字体的名字不能写错。
- 中文字符应该能够显示了，但是你可能会发现很多文字已经超出了文档的边界无法显示了，这是因为pandoc对中文的支持不太好，不能自动换行。解决方案是使用编辑好的latex模板来生成pdf。然后编译命令加上： \--template=name.tex
- 你也可以使用自己定义的模板来生成tex和pdf文件。首先使用命令 pandoc -D latex > name.tex 生成一个默认的模板，在对这个模板进行修改，如字体、自动换行等。
- pandoc表格排版方式要求用 \-- \-- \-- 来分割列。这样排版的并不通用。
中文pdf的输出

~~~
pandoc name.md -o name.pdf --latex-engine=xelatex  -V mainfont="SimSun"
~~~

中文调用模板pdf的输出

~~~
pandoc name.md -o name.pdf --latex-engine=xelatex --template=pm-name.tex
~~~

在emacs下使用时须配置markdown-mode。修改markdown-mode 的Customization

一般是通过 M-x customize-mode 来修改，将Markdown Command 修改为:

~~~
pandoc -f markdown -t html -s -c /PathToFile/name.css --mathjax --highlight-style espresso
~~~

上面这句话有两个位置需要注意：

- name.css，可以自定义你的生成的html文件的样式
- mathjax参数，在C-c C-c p做浏览器预览时，可以自动加载数学公式

也可以直接修改在markdown-mode.el文件中。

## 使用自定义的css来渲染网页，然后用浏览器生成pdf

非emacs下可通过在命令行中执行如下代码生成自定义的html文件：

~~~
pandoc -s name.md -c name.css -o name.html
~~~


**参考网页**

> http://blog.sina.com.cn/s/blog_5ee56d450101dah2.html

[^Footnote]:SclentificLinux6.6

