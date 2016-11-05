---
layout: post
title: "吴鸿毅 CodeProject - How To Authorize(Geant4)"
date: 2016-01-30 12:47:57 +0800
comments: true
tags: [Geant4]
---
<!-- README.md --- 
;; 
;; Description: 
;; Author: Hongyi Wu(吴鸿毅)
;; Email: wuhongyi@qq.com 
;; Created: 四 1月 28 00:53:54 2016 (+0800)
;; Last-Updated: 六 7月 16 20:43:41 2016 (+0800)
;;           By: Hongyi Wu(吴鸿毅)
;;     Update #: 8
;; URL: http://wuhongyi.cn -->

# Authorize 文件夹说明

文件夹下有以下文件：

- Auth.wu
- Log
- README.html
- README.md
- README.pdf
- RunNumber
- UserInformation.txt

----

## Auth.wu

程序使用授权验证文件，需要由 吴鸿毅(wuhongyi@qq.com) 提供。
<!--more-->

## Log

每次程序启动、授权验证通过后会在该文件夹内生成一个名字为当前**年月日时分秒**的文件夹，并将**ReadData.txt**文件中要求的文件以及**RunNumber**文件复制到该文件夹内。可用来追溯每一次模拟的相关参数。这在模拟数据出现异常、忘记相关模拟参数时有很大用处。

## RunNumber

该文件内写有一个 int 型数据，用来控制生成数据文件编号。ReadData.txt 内填入要输出数据名称还有数据格式。如果该文件内数值为小于等于 -1 数值，则输出文件为 ReadData.txt 内填入参数。若为大于等于 0 数值，则在输出文件名上添加该数值作为数据编号，然后该文件内数据加 1。

例如：文件内数据为 98，执行一次程序输出文件名为 FileNameR0098\_tN.root，文件内数据变为 99。其中 FileName 为 ReadData.txt 内填入文件名，\_tN 为多线程输出特征，.root 说明 ReadData.txt 内填入数据格式为 root，R0098为当前数据编号。再执行一次程序则输出数据文件名为 FileNameR0099\_tN.root，依次类推。一般开始模拟前将该数值设为 0。

## UserInformation.txt

该文件当前可填写自己的邮箱地址，该地址在程序运行结束时候会收到相关提示。可在**ReadData.txt**文件中设置接收/不接收提示邮件。

![程序结束时邮件通知](/img/G4EmailNotice.png)

如上图，邮件正文包括了模拟粒子数、运行计算机IP、程序执行时间情况及相关参数的附件。IP可方便在外远程登陆计算机继续模拟。

<!-- README.md ends here -->
