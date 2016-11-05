---
layout: post
title: "shell 脚本加密"
date: 2013-10-17 00:33:00 +0800
comments: true
tags: [Linux,shell]
---

&#160; &#160; &#160; &#160; 如何保护自己编写的 shell 程序\?要保护自己编写的 shell 脚本程序，方法有很多，最简单的方法有两种：1、加密 2、设定过期时间，下面以 shc 工具为例说明：
<!--more-->
## 一、下载安装shc工具
shc 是一个加密 shell 脚本的工具.它的作用是把 shell 脚本转换为一个可执行的二进制文件.
~~~bash
# wget http://www.datsi.fi.upm.es/~frosal/sources/shc-3.8.7.tgz
~~~
安装:
~~~bash
# tar zxvf shc-3.8.7.gz
# cd shc-3.8.7
# mkdir /usr/local/man/man1/     (install 时会把 man 文件放入该目录,如果该目录不存在需提前建好) 这一步需要root权限
# make test
# make
# make test
# make strings
# make install       这一步需要root权限
~~~
## 二、加密方法:
~~~bash
shc -r -f script-name
~~~
注意:要有 **-r** 选项, **-f** 后跟要加密的脚本名。运行后会生成两个文件,script-name.x 和 script-name.x.c  
script-name.x 是加密后的可执行的二进制文件，
~~~bash
./script-name 即可运行
~~~
script-name.x.c 是生成 script-name.x 的原文件(c语言)

~~~bash
# shc -v -f test.sh
~~~
**-v** 是 verbose 模式, 输出更详细编译日志;**-f** 指定脚本的名称。
~~~bash
# ll test*
~~~
~~~
-rwxr-xr-x 1 oracle oinstall 1178 Aug 18 10:00 test.sh
-rwx--x--x 1 oracle oinstall 8984 Aug 18 18:01 test.sh.x
-rw-r--r-- 1 oracle oinstall 14820 Aug 18 18:01 test.sh.x.c
~~~
~~~bash
# file test.sh.x
~~~
~~~
test.sh.x: ELF 32-bit LSB executable, Intel 80386, version 1 (SYSV), for GNU/Linux 2.2.5, dynamically linked (uses shared libs), stripped
~~~
可以看到生成了动态链接可执行二进制文件test.sh.x和C源文件testup.sh.x.c, 注意生成的二进制文件因为是动态链接形式, 所以在其它平台上不能运行。

生成静态链接的二进制可执行文件可以通过下面的方法生成一个静态链接的二进制可执行文件:
~~~bash
$ CFLAGS=-static shc -r -f test.sh
$ file testup.sh.x
~~~

## 三. 通过sch加密后的脚本文件很安全吗?

&#160; &#160; &#160; &#160;一般来说是安全的, 不过可以使用gdb和其它的调试工具获得最初的源代码. 如果需要更加安全的方法, 可以考虑使用 wzshSDK。 另外 shc 还可以设置脚本的运行期限和自定义返回信息:
~~~bash
$ shc -e 03/31/2007 -m "the mysql backup scrīpt is now out of date." -f test.sh
~~~
**-e** 表示脚本将在 2007 年 3 月 31 日前失效, 并根据 **-m** 定义的信息返回给终端用户。

----
**题外：**
如果你仅仅是看不见内容就行了的话，不妨用
~~~bash
gzexe a.sh
~~~
原来的 a.sh 就被存为 a.sh~，新的 a.sh 是乱码，但是可以用 sh 的方式运行。


----
转自： http://blog.chinaunix.net/uid-26495963-id-3180203.html 
