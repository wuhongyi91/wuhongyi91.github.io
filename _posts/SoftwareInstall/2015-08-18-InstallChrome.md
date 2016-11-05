---
layout: post
title: "Scientific Linux 自动安装谷歌浏览器 chrome"
date: 2015-08-18 14:07:05 +0800
comments: true
tags: [Linux,Software,Chrome]
---

&#160; &#160; &#160; &#160;Scientific Linux 6 下默认安装浏览器是 Firefox ，但其响应较慢，令人蛋疼。
<!--more-->
## 下载脚本
先下载自动安装脚本：http://chrome.richardlloyd.org.uk/install_chrome.sh

## 修改脚本
将其中的
~~~
http://omahaproxy.appspot.com
~~~
改为
~~~
https://dl.google.com/linux/direct/google-chrome-stable_current_x86_64.rpm
~~~

可以直接[下载](http://share.weiyun.com/0d5cdd1fd694e447c887a25ce3f22d57)我修改好的脚本。

## 安装
打开终端，依次执行
~~~ shell
chmod u+x install_chrome.sh  
./install_chrome.sh
~~~

----
&#160; &#160; &#160; &#160;执行以上命令会自动下载并安装最新版谷歌浏览器及相关依赖包。在终端下执行google-chrome就可以打开浏览器了。  
在系统左上角的 应用程序-Internet 里面也会有Google Chrome 启动项。




<br />
----
&#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160;**Update: 4. Last-Modified: 2015-08-18 14:15**
