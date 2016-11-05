---
layout: post
title: "代码中特殊的注释技术——TODO、FIXME和XXX的用处"
date: 2013-12-25 18:59:00 +0800
comments: true
tags: [Linux,C/C++]
---

本文内容概要： 代码中特殊的注释技术——TODO、FIXME和XXX的用处。<!--more-->
**前言：**  
在阅读Qt  Creator的源代码时，发现一些注释中有FIXME英文单词，用英文词典居然查不到其意义！
实际上，在阅读一些开源代码时，我们常会碰到诸如：TODO、FIXME和XXX的单词，它们是有其特殊含义的。、

## TODO
说明：  
如果代码中有该标识，说明在标识处有功能代码待编写，待实现的功能在说明中会简略说明。

## FIXME
说明：  
如果代码中有该标识，说明标识处代码需要修正，甚至代码是错误的，不能工作，需要修复，如何修正会在说明中简略说明。

## XXX
说明：  
如果代码中有该标识，说明标识处代码虽然实现了功能，但是实现的方法有待商榷，希望将来能改进，要改进的地方会在说明中简略说明。

----
eclipse 中特殊的注释：  
在 eclipse 中，TODO、FIXME 和 XXX 都会被 eclipse 的 task 视图所收集。在项目发布前，检查一下 task 视图是一个很好的习惯。此外，在 eclipse 中，我们可自定义自己的特殊注释标签。如在 C/C++ 中，进入 window—>preferences—>C/C++—>Task Tags 窗口即可添加特殊标签，默认只有 TODO、FIXME 和 XXX。

----
转载自博客网址：http://blog.csdn.net/reille/。  作者：reille
 
