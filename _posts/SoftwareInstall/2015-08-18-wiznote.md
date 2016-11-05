---
layout: post
title: "Scientific Linux 安装 wiznote(为知笔记)"
date: 2015-08-18 23:23:22 +0800
comments: true
tags: [Linux,Software,wiznote]
---

&#160; &#160; &#160; &#160;网络笔记很多，但是绝大多数只有 **windows** 客户端，极少有开发 **Linux** 客户端的，然而即便是有客户端，不支持 **LATEX** 数学公式输入的在线笔记还是满足不了很多人的需求。为知笔记能够支持 <font color=red>markdown</font> 语法，也为经常需要在笔记中插入数学公式的带来了福音。
<!--more-->
## 源代码
为知笔记其代码也托管在 github ，可以通过：https://github.com/WizTeam 下载其源代码。

## 快速安装
针对不同的系统官网也给出了安装方法，
官网提供安装方法：
~~~shell
yum-config-manager --add-repo=https://copr.fedoraproject.org/coprs/mosquito/myrepo/repo/epel-$(rpm -E %?rhel)/mosquito-myrepo-epel-$(rpm -E %?rhel).repo 
yum install epel-release 
yum localinstall http://li.nux.ro/download/nux/dextop/el$(rpm -E %rhel)/x86_64/nux-dextop-release-0-2.el$(rpm -E %rhel).nux.noarch.rpm http://download1.rpmfusion.org/nonfree/el/updates/$(rpm -E %rhel)/x86_64/rpmfusion-nonfree-release-$(rpm -E %rhel)-1.noarch.rpm http://download1.rpmfusion.org/free/el/updates/$(rpm -E %rhel)/x86_64/rpmfusion-free-release-$(rpm -E %rhel)-1.noarch.rpm 
yum install wiznote      # Stable version  
yum install wiznote-beta      # Development version
~~~

对 Scientific Linux 6 只需要在终端中依次执行以下三行即可（需要 ROOT 权限）：
~~~shell
# yum-config-manager --add-repo=https://copr.fedoraproject.org/coprs/mosquito/myrepo/repo/epel-$(rpm -E %?rhel)/mosquito-myrepo-epel-$(rpm -E %?rhel).repo 
# yum install epel-release 
# yum install wiznote
~~~

----
&#160; &#160; &#160; &#160;Linux 版客户端功能相对 windows 版功能更新较慢，但是也在逐渐完善中。


<br />
----
&#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160;**Update: 4. Last-Modified: 2015-10-01 15:03**
