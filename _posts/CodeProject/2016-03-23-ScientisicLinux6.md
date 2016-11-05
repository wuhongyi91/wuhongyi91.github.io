---
layout: post
title: "Scientific Linux 6 常用软件安装"
date: 2016-03-23 16:52:33 +0800
comments: true
tags: [Linux,Software]
---
<!-- ScientisicLinux6.md --- 
;; 
;; Description: 
;; Author: Hongyi Wu(吴鸿毅)
;; Email: wuhongyi@qq.com 
;; Created: 三 3月 23 16:52:33 2016 (+0800)
;; Last-Updated: 六 7月 16 19:41:22 2016 (+0800)
;;           By: Hongyi Wu(吴鸿毅)
;;     Update #: 18
;; URL: http://wuhongyi.cn -->


# Scientific Linux 6 常用软件安装

## 吴鸿毅（wuhongyi@qq.com）

### 更新于 2016 年 3 月 23 日 - 北京大学 - 加速器楼

***

-------

**以下终端命令中，没有$开头的表示需要root权限。**

###  Scientific Linux 6 Everything完整版安装

- 将光盘 DVD1 放进光驱，等待进入安装界面。
- **待补充**。
<!--more-->

----

## 软件的安装方法

请按照以下先后顺序依次安装。

### yum安装所需库

- pythia8(程序安装非必须，可不安装)
- ruby(程序安装非必须，可不安装)
- llvm(程序安装非必须，可不安装)
- fftw3(程序安装非必须，ROOT中roofit进行快速傅立叶变换需要)
- xerces(G4需要)

依次对应执行以下命令行：

```bash
yum install pythia8*
yum install ruby.x86_64 ruby-libs.x86_64
yum install llvm.x86_64 llvm-devel.x86_64 llvm-libs.x86_64
yum install fftw.x86_64 fftw-devel.x86_64
yum install xerces-c.x86_64 xerces-c-devel.x86_64
```

### GCC

*获取源码*：

可以网上下载，也可在终端中输入以下：

```bash
$ wget http://ftp.gnu.org/gnu/gcc/gcc-4.9.2/gcc-4.9.2.tar.gz
```

自动下载。

*编译安装*：

```bash
yum install glibc-static libstdc++-static //安装基础库,想要安装gcc 4.8及以上版本，你需要先安装C标准库和头文件，以及旧版本的c++编译器。
$ tar -zxvf gcc-4.9.2.tar.gz  //解压缩
$ cd gcc-4.9.2     //进入解压缩出来的文件夹
$ ./contrib/download_prerequisites //自动编译下载所需文件
$ cd ..
$ mkdir build_gcc492 //在gcc-4.9.2同一目录下新建文件夹
$ cd build_gcc492
$ ../gcc-4.9.2/configure --prefix=/usr --enable-checking=release
	--enable-languages=c,c++,fortran --disable-multilib
$ make -jN
make install
```

### cmake

*获取源码*：

可以去官网下载 cmake-3.4.1.tar.gz


*编译安装*：

```bash
$ tar -zxvf cmake-3.4.1.tar.gz  //解压缩
$ cd cmake-3.4.1    //进入解压缩出来的文件夹
$ ./configure --prefix=/usr
$ make -jN
make install
```


### xerces

如果使用 yum 安装了就不需要进行当前步骤。当前步骤是 yum 故障时候的一种安装方法。

*获取源码*：

可以去官网下载 xerces-c-3.1.1.tar.gz


*编译安装*：

```bash
$ tar -zxvf xerces-c-3.1.1.tar.gz  //解压缩
$ cd xerces-c-3.1.1    //进入解压缩出来的文件夹
$ ./configure --prefix=/usr
$ make -jN
make install
```

### ROOT

建议安装在/opt文件夹下。

官网下载 root_v6.02.12.source.tar.gz


*编译安装*：

```bash
cp /PathTo/root_v6.02.12.source.tar.gz /opt
cd /opt
tar -zxvf root_v6.02.12.source.tar.gz
cd root-6.02.12
./configure --all
make -jN
```


### Geant4

建议安装在/opt文件夹下，在该文件夹内新建文件夹Geant4。

官网下载  geant4.10.02.tar.gz 及相应数据包。

*编译安装*：

```bash
mkdir /opt/Geant4
cp /PathTo/geant4.10.02.tar.gz /opt/Geant4
cd /opt/Geant4
tar -zxvf /PathTo/geant4.10.02.tar.gz
mkdir buildgeant41002
cd buildgeant41002
cmake -DCMAKE_INSTALL_PREFIX=/opt/Geant4/geant41002/
      -DGEANT4_USE_SYSTEM_EXPAT=OFF -DGEANT4_USE_OPENGL_X11=ON
	  -DGEANT4_USE_RAYTRACER_X11=ON -DGEANT4_USE_QT=ON
	  -DGEANT4_USE_RAYTRACER_X11=ON -DGEANT4_BUILD_MULTITHREADED=ON
	  -DGEANT4_USE_GDML:BOOL=ON /opt/Geant4/geant4.10.02/
make -jN
make install
```

完成以上步骤即将Geant4安装到/opt/Geant4/geant41002/。在/opt/Geant4/geant41002/share/Geant4-10.2.0中新建文件夹data，然后将下载的数据解压缩放在该data文件夹内。


### Garfield

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

<!-- ScientisicLinux6.md ends here -->
