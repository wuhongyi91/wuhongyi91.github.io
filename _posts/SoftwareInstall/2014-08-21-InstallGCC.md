---
layout: post
title: "Scientific Linux 安装 GCC"
date: 2014-8-21 15:09:14 +0800
comments: true
tags: [Linux,Software,GCC]
---

&#160; &#160; &#160; &#160;Scientific Linux 默认 gcc 版本较低，并不支持 C++11 标准。需要使用较新的版本就得自己编译、安装。
<!--more-->
## 下载源码
获取源码，可以网上下载，也可在终端中输入以下
~~~
$ wget http://ftp.gnu.org/gnu/gcc/gcc-4.8.1/gcc-4.8.1.tar.gz
~~~
自动下载。（其中 4.8.1 换成想要的相应版本）

## 安装
编译安装：
~~~
yum install glibc-static libstdc++-static //安装基础库,想要安装gcc 4.8及以上版本，你需要先安装C标准库和头文件，以及旧版本的c++编译器。
tar xzf gcc-4.8.1.tar.gz  //解压缩
cd gcc-4.8.1     //进入解压缩出来的文件夹
./contrib/download_prerequisites //自动编译下载所需文件
cd ..
mkdir build_gcc4.8 //在gcc-4.8.1同一目录下新建文件夹
cd build_gcc4.8    
../gcc-4.8.1/configure --enable-checking=release --enable-languages=c,c++,fortran --disable-multilib
make -j4
make install
~~~

以上命令中
~~~
../gcc-4.8.1/configure --enable-checking=release --enable-languages=c,c++,fortran --disable-multilib
~~~
是默认安装在 /usr/local/ 中，这样还得进行以下操作：
将/usr/local/lib64/目录下面的libstdc++.so.6和libstdc++.so.6.0.18（18这个数值不同版本不同，如果安装高版本找最大的）拷贝到/usr/lib64/目录下面：
~~~
cp /usr/local/lib64/libstdc++.so.6 /usr/lib64/
cp /usr/local/lib64/libstdc++.so.6.0.18 /usr/lib64/
~~~
删除libstdc++.so.6旧的链接，建立新的链接，同时删除libstdc++.so.6.0.13：
~~~
ln -sf /usr/lib64/libstdc++.so.6.0.18 /usr/lib64/libstdc++.so.6
rm -f /usr/lib64/libstdc++.so.6.0.13
~~~
不过目前还不能使用新版本的gcc，因为新版的可执行文件还没加到命令的搜索路径中。在这里我为新版的gcc和g++命令分别建立了一个软链接。进入/usr/bin目录后，键入如下命令建立软链接。 ln -s /usr/local/gcc-4.5.0/bin/gcc gcc45 , ln -s /usr/local/gcc-4.5.0/bin/g++ g++45 这样我使用新版本gcc的时候就可以用gcc45和g++45命令，同时也可使用原来的gcc编译程序。当然这里也可以直接将/usr/bin目录下gcc，g++命令重新链接到新版本的gcc可执行文件。


## 覆盖/usr安装
如果想覆盖系统的版本，按照以下方法安装即可：
~~~
../gcc-4.9.2/configure --prefix=/usr --enable-checking=release --enable-languages=c,c++,fortran --disable-multilib
~~~
或者
~~~
../gcc-4.9.2/configure --prefix=/usr --enable-checking=release --enable-languages=c,c++,fortran --disable-multilib --enable-libgcj-debug --with-stabs
~~~

查看：
~~~
strings /usr/lib64/libstdc++.so.6 | grep GLIBCXX
~~~

<br />
----
&#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160;**Update: 13. Last-Modified: 2015-4-09 13:00**
