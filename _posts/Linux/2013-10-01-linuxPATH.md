---
layout: post
title: "linux PATH环境变量全解析"
date: 2013-10-01 14:43:00 +0800
comments: true
tags: [Linux,PATH]
---

&#160; &#160; &#160; &#160;关于 PATH 的作用：PATH 说简单点就是一个字符串变量，当输入命令的时候 LINUX 会去查找 PATH 里面记录的路径。比如在根目录/下可以输入命令 ls。
<!--more-->
在 /usr 目录下也可以输入 ls,但其实 ls 这个命令根本不在这个两个目录下。  
事实上当你输入命令的时候 LINUX 会去 /bin,/usr/bin,/sbin 等目录下面去找你此时输入的命令，而 PATH 的值恰恰就是 /bin:/sbin:/usr/bin:……。  
其中的冒号使目录与目录之间隔开。  

## 关于新增自定义路径：
假设新安装了一个命令在 /usr/locar/new/bin 下面，如果想像 ls 一样在任何地方都使用这个命令，就需要修改环境变量 PATH 了。  
准确的说就是给 PATH 增加一个值 /usr/locar/new/bin。
 
## 需要一行bash命令:
~~~bash
export PATH=$PATH:/usr/locar/new/bin
~~~
这条命令的意思为: 使 PATH 自增:/usr/locar/new/bin,
即
~~~bash
PATH=PATH+":/usr/locar/new/bin";
~~~

## 通常的做法是：
把这行 bash 命令写到 /root/.bashrc 的末尾，然后当你重新登陆 LINUX 的时候（linux 启动时就会执行这个文件），新的默认路径就添加进去了。  
当然，也可以直接用命令：
~~~bash
$ source /root/.bashrc
~~~
执行这个文件重新登陆了。可以用 echo $PATH 命令查看PATH的值。

## 关于删除自定义路径：
如果发现新增的路径 /usr/locar/new/bin 已经没用了，可以修改 /root/.bashrc 文件里面你新增的路径。
或者修改 /etc/profile 文件删除不需要的路径。
 
修改/root/.bashrc文件，删除相应环境变量选项，然后  
~~~bash
$ source /root/.bashrc
~~~
即可。
或者可以利用命令。如要删除 PATH 里的 /usr/local/del/bin:变量，则可直接在命令行里输入
~~~bash
$ export PATH=$(echo $PATH | sed 's/:\/usr\/local\/del\/bin:/:/g')
~~~
注意：＂／＂代表转意字符。

----

比如要把 /etc/apache/bin 目录添加到 PATH 中，方法有三：
- 1.
  ~~~bash
  $ PATH=$PATH:/etc/apache/bin
  ~~~
  使用这种方法,只对当前会话有效，也就是说每当登出或注销系统以后，PATH 设置就会失效。
 
- 2.
  ~~~bash
  $ emacs /etc/profile
  ~~~
  在适当位置添加 PATH=$PATH:/etc/apache/bin （注意：＝ 即等号两边不能有任何空格）  
  这种方法最好,除非你手动强制修改 PATH 的值,否则将不会被改变。

- 3.
  ~~~bash
  $ emacs ~/.bash_profile
  ~~~
  修改 PATH 行,把 /etc/apache/bin 添加进去，这种方法是针对用户起作用的。
 
**NOTE：**
想改变 PATH，必须重新登陆才能生效，以下方法可以简化工作：
如果修改了/etc/profile，  
那么编辑结束后执行
~~~bash
$ source profile  (source /etc/profile)
~~~
或 执行点命令
~~~bash
$ ./profile
~~~
PATH 的值就会立即生效了。  
这个方法的原理就是再执行一次 /etc/profile shell 脚本，注意如果用sh /etc/profile 是不行的，因为 sh 是在子 shell 进程中执行的，即使 PATH 改变了也不会反应到当前环境中，但是 source 是在当前 shell进程中执行的，所以我们能看到 PATH 的改变。

这样你就学会 Linux 系统下修改环境变量 PATH 路径的方法。

## 补充说明
工作环境设置文件
环境设置文件有两种：系统环境设置文件 和 个人环境设置文件：
 
1.系统中的用户工作环境设置文件：
~~~
登录环境设置文件：/etc/profile  
非登录环境设置文件：/etc/bashrc
~~~
2.用户个人设置的环境设置文件：
~~~
  登录环境设置文件: $HOME/.bash_profile   //这个是环境变量设置的地方
  非登录环境设置文件：$HOME/.bashrc       //这个是定义别名的地方
~~~

**登录环境：**指用户登录系统后的工作环境  
**非登录环境：**指用户再调用子shell时所使用的用户环境

----
本文转自：http://www.2cto.com/os/201211/165769.html 

