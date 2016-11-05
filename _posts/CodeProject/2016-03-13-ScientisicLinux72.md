---
layout: post
title: "Scientific Linux 7 常用软件安装"
date: 2016-03-13 21:42:38 +0800
comments: true
tags: [Linux,Software]
---
<!-- ScientisicLinux72.md --- 
;; 
;; Description: 
;; Author: Hongyi Wu(吴鸿毅)
;; Email: wuhongyi@qq.com 
;; Created: 日 3月 13 21:42:38 2016 (+0800)
;; Last-Updated: 六 7月 16 19:48:06 2016 (+0800)
;;           By: Hongyi Wu(吴鸿毅)
;;     Update #: 103
;; URL: http://wuhongyi.cn -->


# Scientific Linux 7 常用软件安装

## 吴鸿毅（wuhongyi@qq.com）

### 更新于 2016 年 3 月 23 日 - 北京大学 - 加速器楼

***

**以下终端命令中，没有$开头的表示需要root权限。**

-------

### Scientific Linux 7 安装
<!--more-->
![安装初始界面](/img/ScientificLinux7Install1.jpg)

进入以上安装界面，需要重点设置的是**软件选择(S)**、**安装位置(D)**两个位置。

![软件选择](/img/ScientificLinux7Install2.jpg)

进入**软件选择(S)**选项，这里选**开发及生成工作站**，右边已选环境的附加选项**全部打钩**。

![安装位置](/img/ScientificLinux7Install3.jpg)

进入**安装位置(D)**选项，这里选**我要配置分区**。

![标准分区](/img/ScientificLinux7Install4.jpg)

这里新挂载点将使用以下分区方案：**标准分区**。

![文件系统](/img/ScientificLinux7Install5.jpg)

创建/、/boot、/home时候，文件系统选**ext4**，然后点击右下的**更新设置**。


安装完成之后，第一次开机时候会跳出一些引导，出来语言选择时候，务必选择**汉语(Intelligent Pinyin) 拼**选项，不要选**汉语 zh**选项。

----

安装好系统之后：

系统自带python2.7.5。

将我的yum配置文件替换系统中的配置文件。**以下安装默认是采用我配置的yum库。**

----

自带emacs24.3.1与我的.emacs配置有冲突（开多窗口失效），所以自己安装最新24.5版本（暂时还没发现没法修复的bug）。

依赖的库：

```bash
yum install giflib-devel.x86_64  giflib-utils.x86_64
```

```bash
$ tar -zxvf emacs-24.5.tar.gz
$ cd emacs-24.5
$ ./configure --prefix=/usr
$ make -j4
make install
```

----

系统自带gcc4.8.5，基本够用，为了ROOT能够支持C++14属性，我还是安装gcc4.9.2。

依赖的库：

```bash
yum install glibc-static libstdc++-static //安装基础库,想要安装gcc 4.8及以上版本，你需要先安装C标准库和头文件，以及旧版本的c++编译器。
```

```bash
$ tar -zxvf gcc-4.9.2.tar.gz
$ cd gcc-4.9.2
$ ./contrib/download_prerequisites //自动编译下载所需文件
$./configure --prefix=/usr --mandir=/usr/share/man --infodir=/usr/share/info --enable-checking=release  --enable-languages=c,c++,fortran --disable-multilib
$ make -j4
make install
```

----

系统自带cmake版本2.8.11，Geant4.10.02 要求 cmake 版本大于3.0。所以还是安装最新的3.4.1版本。

```bash
$ tar -zxvf cmake-3.4.1.tar.gz
$ cd cmake-3.4.1
$ ./configure --prefix=/usr
$ make -j4
make install
```

----

# 常用必需

```bash
#多机并行（有空再测试G4的多机并行）
yum install mpich.x86_64 mpich-devel.x86_64

#汉化包，缺少将导致有些软件为英文界面，例如kate
yum install kde-l10n-Chinese.noarch  kde-l10n-Chinese-Traditional.noarch

#screen
yum install screen.x86_64
```

# 额外功能（根据需要选择安装）

```bash
#压缩解压缩.rar文件
yum install rar.x86_64

#kate编辑器
yum install kate.x86_64 kate-devel.x86_64 kate-libs.x86_64 kate-part.x86_64 ghc-highlighting-kate-devel.x86_64 ghc-highlighting-kate.x86_64

#打开ntfs格式硬盘
yum install findntfs.x86_64 ntfs-3g.x86_64 ntfs-3g-devel.x86_64

#flash播放
yum install flash-plugin.x86_64

#桌面录制
yum install recordmydesktop.x86_64 gtk-recordmydesktop.noarch

#mp3格式文件转wav格式
yum install mpg123.x86_64 mpg123-devel.x86_64 perl-Audio-Play-MPG123.noarch

#播放mp3
yum install libmad.x86_64 libmad-devel.x86_64

#clang编译器
yum install clang.x86_64 clang-devel.x86_64

#hdf5
yum install hdf5.x86_64  hdf5-devel.x86_64 hdf5-mpich.x86_64 hdf5-mpich-devel.x86_64  hdf5-openmpi.x86_64 hdf5-openmpi-devel.x86_64

#Hexo个人网站制作
yum install npm.noarch  nodejs.x86_64 
npm install -g hexo

#加快显示网页的latex数学公式
yum install mathjax.noarch

#markdown转pdf
yum install pandoc.x86_64
yum pandoc-pdf.x86_64 #这个不需要

#翻译软件，配置后可选词翻译
yum install goldendict.x86_64 goldendict-docs.noarch

#gnuplot
yum install gnuplot.x86_64 gnuplot-common-4.6.2-3.el7.x86_64

#GDB调试时查看gcc问题(无需装)
yum install glibc-utils.x86_64
yum install glibc-debuginfo.x86_64 glibc-debuginfo-common.x86_64  #须打开sl7-other.repo中的[sl-debuginfo]
```


----

# ROOT

```bash
yum install fftw.x86_64 fftw-devel.x86_64 fftw-libs.x86_64
yum install gsl.x86_64 gsl-devel.x86_64
yum install graphviz.x86_64 graphviz-devel.x86_64
yum install ruby.x86_64 ruby-devel.x86_64 ruby-libs.x86_64
yum install expect.x86_64 expect-devel.x86_64
yum install davix.x86_64 davix-devel.x86_64
yum install unuran.x86_64 unuran-devel.x86_64
yum install avahi-compat-libdns_sd.x86_64 avahi-compat-libdns_sd-devel.x86_64
yum install ftgl.x86_64 ftgl-devel.x86_64
yum install glew.x86_64 glew-devel.x86_64
yum install mysql++.x86_64 mysql++-devel.x86_64
yum install cfitsio.x86_64 cfitsio-devel.x86_64
yum install libxml2*
yum install binutils-devel.x86_64
yum install pythia8.x86_64 pythia8-devel.x86_64
yum install redhat-lsb.x86_64
```

```bash
./configure --all
make -j4
```

Enabled support for asimage, astiff, bonjour, builtin_afterimage, builtin_llvm, explicitlink, fftw3, fitsio, gviz, gdml, genvector, http, ldap, mathmore, memstat, minuit2, mysql, odbc, opengl, pgsql, pythia8, python, roofit, search_usrlocal, shadowpw, shared, sqlite, ssl, table, tmva, unuran, vc, vdt, x11, xft, xml.


----

# Geant4

```bash
yum install xerces-c.x86_64  xerces-c-devel.x86_64
```

```bash
mkdir buildgeant41002p01
cd buildgeant41002p01/
cmake -DCMAKE_INSTALL_PREFIX=/opt/Geant4/geant41002p01  -DGEANT4_USE_SYSTEM_EXPAT=OFF -DGEANT4_USE_OPENGL_X11=ON -DGEANT4_USE_RAYTRACER_X11=ON -DGEANT4_USE_QT=ON -DGEANT4_USE_RAYTRACER_X11=ON -DGEANT4_BUILD_MULTITHREADED=ON -DGEANT4_USE_GDML:BOOL=ON  -DGEANT4_BUILD_TLS_MODEL=global-dynamic   /opt/Geant4/geant4.10.02.p01
make -j4
make install
```


VGM >http://ivana.home.cern.ch/ivana/VGM.html
如果装有VGM，则可添加
```bash
-DGeant4VMC_USE_VGM=ON
```

----

# Garfield

按照UserGuide.pdf中的方式下载到源码，我将其压缩成Garfield.tar.gz文件了。

以下我将动态连接库、静态连接库两种都安装了，方便使用。

```bash
cp Garfield.tar.gz /opt/
cd /opt/
tar -zxvf Garfield.tar.gz
mkdir buildGarfield
cd buildGarfield
cmake -DCMAKE_INSTALL_PREFIX=/opt/Garfield /opt/Garfield/
make -j4
make install
cd /opt/Garfield
make -j4
```

----

# geant4_vmc

Geant4 VMC, as well as ROOT and Geant4, have moved to the C++11 standard. The latest versions of all three packages use C++11 by default: Geant4 VMC 3.3, Geant4 10.2 and ROOT 6. When mixing other versions of Geant4 and ROOT together, the same standard must be used for both packages. See below how the override the default setting when needed.

Geant4 VMC 3.3 built against ROOT 5.x requires ROOT built with C++11 (not default for this ROOT version), set via:

```bash
-Dcxx11=ON
```
option, when ROOT is built using CMake, or

```bash
--enable-cxx11
```
option, when ROOT is built using configure script. 


需要将程序包中cmake文件夹内FindGarfield.cmake中的

```bash
find_library(Garfield_LIBRARIES NAMES Garfield
	         HINTS ${Garfield_DIR}/lib ${Garfield__LIB_DIR}
             HINTS $ENV{GARFIELD_HOME}/Library)
			 ```
			 
替换为：

```bash
find_library(Garfield_LIBRARIES NAMES Garfield
	         HINTS ${Garfield_DIR}/lib ${Garfield__LIB_DIR}
             HINTS $ENV{GARFIELD_HOME}/lib)
```

```bash
cp geant4_vmc.tar.gz /opt/VMC
cd /opt/VMC
tar -zxvf geant4_vmc.tar.gz
mkdir buildgeant4vmc
mkdir geant4vmc
cd buildgeant4vmc/
cmake -DCMAKE_INSTALL_PREFIX=/opt/VMC/geant4vmc/ /opt/VMC/geant4_vmc/
make -j4
make install
```

----

# 以下是一些其它用途的软件

----

# 安装谷歌浏览器 chrome

~~~
先下载自动安装脚本：http://chrome.richardlloyd.org.uk/install_chrome.sh

然后将其中的
http://omahaproxy.appspot.com
改为
https://dl.google.com/linux/direct/google-chrome-stable_current_x86_64.rpm

打开终端，依次执行
chmod u+x install_chrome.sh
./install_chrome.sh
~~~

它会自动下载并安装最新版谷歌浏览器及相关依赖包。在终端下执行google-chrome就可以打开浏览器了。
在系统左上角的 应用程序-Internet 里面也会有Google Chrome 启动项。

----

# 将mp3文件转为wav格式文件

前面已经提供了yum的安装方法了。

The -w option will convert an .mp3 file to .wav file. The syntax is:

```bash
mpg123 -w output.wav input.mp3
```

----

# 安装texlive

先卸载系统中的texlive

```bash
yum remove texlive
yum remove texlive-*
```

下载地址：http://mirrors.ustc.edu.cn/CTAN/systems/texlive/Images/texlive2015.iso

我要将镜像文件挂载到 /opt/texlive 文件夹，所以先建好文件夹 /opt/texlive

具体安装：

```bash
mkdir /opt/texlive
mount -o loop texlive2015.iso  /opt/texlive
cd /opt/texlive
./install-tl
#出现选项后，输入 I 直接安装（也可以更改选项）。
```

环境变量

在~/.bashrc中加入如下语句：

```bash
# TeX Live 2014
export MANPATH=${MANPATH}:/usr/local/texlive/2015/texmf-dist/doc/man
export INFOPATH=${INFOPATH}:/usr/local/texlive/2015/texmf-dist/doc/info
export PATH=${PATH}:/usr/local/texlive/2015/bin/x86_64-linux
```

取消挂载镜像

```bash
umount /opt/texlive
```

### 特别说明

插入太多ROOT生成的tex格式图片容易出现以下问题。

~~~
TeX capacity exceeded, sorry [main memory size=5000000]
如果报以上错误，则在下面文件添加以下几行
/usr/local/texlive/2015/texmf.cnf

main_memory = 1024000000 % words of inimemory available; also applies to inimf&mp 
extra_mem_top = 100000000 % extra high memory for chars, tokens, etc. 
extra_mem_bot = 100000000 % extra low memory for boxes, glue, breakpoints, etc
save_size = 1500000 % for saving values outside current group 
stack_size = 1500000 % simultaneous input sources 

将
/usr/local/texlive/2015/texmf-dist/web2c/texmf.cnf 
main_memory 数值也调大。

texhash 刷新 lsr 数据库
# fmtutil-sys --all
~~~


----


----

# 修复装完双系统中只有linux启动项丢失windows启动项的方法

```bash
su
fdisk -l
```

查看C盘的设备号，比如**/dev/sda2**

```bash
su
emacs /etc/grub.d/40_custom
```

添加以下几行

```
menuentry 'Windows 10 (on /dev/sda2)' --class windows {
    insmod part_msdos
    insmod ntfs
    set root='hd0,msdos2'
    chainloader +1
}
```

其中

```
set root='hd0,msdos2'
0表示第一个硬盘，2为刚才看到的sda2
```

```bash
su
grub2-mkconfig -o /boot/grub2/grub.cfg
```

即将信息添加到/boot/grub2/grub.cfg中

----


<!-- ScientisicLinux72.md ends here -->
