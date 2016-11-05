---
layout: post
title: "吴鸿毅 CodeProject使用说明书"
date: 2016-01-14 22:29:53 +0800
comments: true
tags: [Linux,CodeProject]
---
<!-- CodeProjectManual.md --- 
;; 
;; Description: 
;; Author: Hongyi Wu(吴鸿毅)
;; Email: wuhongyi@qq.com 
;; Created: 四 1月 14 22:29:53 2016 (+0800)
;; Last-Updated: 六 7月 16 19:42:16 2016 (+0800)
;;           By: Hongyi Wu(吴鸿毅)
;;     Update #: 178
;; URL: http://wuhongyi.cn -->


# 吴鸿毅 CodeProject使用说明书

# 关于Geant4、Garfield、ROOT、C/C++、RadWare、NCURSES等程序包

## 吴鸿毅（wuhongyi@qq.com）

### 程序包创建于 2014 年 02 月 - 哈尔滨工程大学 - 31号楼

### 更新于 2016 年 03 月 23 日 - 北京大学 - 加速器楼

----

**为了便于程序包的维护和提高代码开发、利用效率，本程序包的思想是模拟与数据处理分离。虽然在某些时候并不能达到最优效果。整个程序包共划分为通用模块（GCC可执行）、ROOT模块、Geant4模块、Garfield模块、RadWare模块、数据处理模块等功能模块。**

程序包名称为 **CodeProject**，将该文件夹放于 **/home/UserNmae** 目录下即可。需要修改 **projectXXX** 文件夹内 **CMakeLists.txt** 文件中对该程序包路径的设置。

<!--more-->

```bash
set(CODE_DIR /home/UserNmae/CodeProject )   #设置工程文件路径
```

在 .bashrc 文件里添加以下内容：

```bash
export PATH=$PATH:/usr/lib64/mpich/bin      #G4 mpi并行需要调用mpich
source /opt/Geant4/geant41002/bin/geant4.sh #自行修改指向Geant4中geant4.sh文件
source /opt/root60212/bin/thisroot.sh  #自行修改指向ROOT中thisroot.sh文件
export GARFIELD_HOME=/opt/Garfield  #自行修改指向Garfield安装目录
export HEED_DATABASE=$GARFIELD_HOME/Heed/heed++/database/
source /home/UserNmae/CodeProject/RadWare53/.radware.bashrc  #不使用RadWare的不添加
export PATH=$PATH:/home/UserNmae/CodeProject/RadWare53/bin   #不使用RadWare的不添加
```

修改CodeProject/RadWare53/.radware.bashrc 文件第一行中**/home/UserNmae**内容(**不使用的话直接跳过**)：

```bash
export RADWARE_HOME=${RADWARE_HOME:-/home/UserNmae/CodeProject/RadWare53}
```

如果使用 mpi 并行的话还需要在/etc/hosts 加入 ip、主机名(**不使用的话直接跳过**)。即加入以下的第三行：

```
127.0.0.1   localhost localhost.localdomain localhost4 localhost4.localdomain4
::1         localhost localhost.localdomain localhost6 localhost6.localdomain6
例如 192.168.1.107 ScientificLinux
```

主机名替换为自己电脑的主机名，如果装机时使用默认则为 localhost.localomain。如果实在忘记了系统主机名，那就执行以下程序获取主机名：

```c
#include <sys/utsname.h>
#include <unistd.h>
#include <stdio.h>
#include<stdlib.h>
int main()
{
   char computer[256];
   struct utsname uts;

   if (gethostname(computer, 255) != 0 || uname(&uts) < 0)
   {
      fprintf(stderr, "Could not get host information\n");
     exit(1);
   }

  printf("Computer host name is %s\n", computer);
  printf("System is %s on %s hardware\n", uts.sysname, uts.machine);
  printf("Nodename is %s\n", uts.nodename);
  printf("Version is %s\n", uts.release, uts.version);

  exit(0);
}
```

----

## 程序包对系统的要求

**当前程序包适用于（测试过） Scientific Linux 6.6/6.7 Everything完整版、Scientific Linux 7.2（开发及生成工作站）。Scientific Linux 6的安装及软件配置请查看本文件夹内的ScientificLinux6文档，Scientific Linux 7.2的安装及软件配置请查看本文件夹内的ScientificLinux72文档。其它 Linux 系统（fedora、ubuntu）对应的软件安装将在以后给出。**

## 程序包对软件版本要求

- Scientific Linux 6.6/6.7 Scientific Linux 7.2
- cmake 3.4.1
- GCC 4.9.2
- Geant 4.10.01.p01及以上（测试到Geant 4.10.02/Geant 4.10.02.p01）
- ROOT 6.00及以上（测试到6.06.02）
- Garfield(2015年12月17日更新版本)

----

## 程序包目录

本程序包的优点是高度模块化，提供了几个常用工程模板，每次使用只需对该类型工程进行小修改，添加相应的功能即可，让每次的代码书写量尽可能达到最小，避免重复性工作。


程序包包含以下文件夹及文件(以下**待补充**说明该文件夹内文件没有罗列出来)：

- cmake
	- FindROOT.cmake
	- MacroCheckPackageLibs.cmake
	- MacroCheckPackageVersion.cmake
- G4_inc
	- G4mpi (example中提供的多机并行方案，还不完善)
		- G4MPIbatch.hh
		- G4MPImanager.hh
		- G4MPImessenger.hh
		- G4MPIrandomSeedGenerator.hh
		- G4MPIRunMerger.hh
		- G4MPIScorerMerger.hh
		- G4MPIsession.hh
		- G4MPIstatus.hh
		- G4UImpish.hh
		- G4VMPIseedGenerator.hh
		- G4VMPIsession.hh
	- DataWrite.hh  (txt输出测试用，已弃用)
	- wuActionData.hh  (貌似已弃用)
	- wuAssis.hh  (粒子源中用到)
	- wuBOptrChangeCrossSection.hh  (偏倚抽样中人为更改截面)
	- wuBOptrMultiParticleChangeCrossSection.hh  (偏倚抽样中人为更改截面)
	- wuBOptrMultiParticleForceCollision.hh  (偏倚抽样中强制碰撞)
	- wuDigi.hh  (暂时未用)
	- wuDigitizerModule.hh  (暂时未用)
	- wuEventActionLog.hh  (单线程下log输出，有时间须改造支持多线程)
	- wuEventActionSD.hh  (用来SD数据输出)
	- wuG4Authorize.hh  (权限验证)
	- wuG4DataControl.hh  (控制输出Branch信息)
	- wuG4LinkDef.h
	- wuHistogramRand.hh  (单一、连续谱抽样)
	- wuHit.hh  (有关SD输出)
	- wuKitDetectorOptical.hh  (材料光学属性、光学表面设置)
	- wuNeutronHPphysics.hh  (0-20MeV中子模型)
	- wuNeutronPhysics.hh  (0-100TeV中子模型)
	- wuOpticalPhysics.hh  (光学模型)
	- wuPrimaryGeneratorAction.hh  (粒子源)
	- wuPrimaryParticleInformation.hh  (未完成，记录粒子源)
	- wuPrimaryVertexInformation.hh  (未完成，记录粒子源)
	- wuRegionInformation.hh  (用来做区域设置)
	- wuRunAction.hh  (用来Step数据输出)
	- wuRunActionSD.hh  (用来SD数据输出)
	- wuRunActionTimer.hh  (用来计算运行时间以及发邮件通知)
	- wuRun.hh  (暂时未用)
	- wuScintillation.hh  (光学过程发光模型)
	- wuScoreWriter.hh  ()
	- wuSensitiveDetector.hh  (用来做SD设置)
	- wuSmearer.hh  (强制改变粒子能量、方向)
	- wuSteppingAction.hh  (用来Step数据输出)
	- wuTrackInformation.hh  (做跟踪标记)
	- wuTrajectory.hh  (有关径迹显示，暂时未用)
	- wuTrajectoryPoint.hh  (有关径迹显示，暂时未用)
	- wuVEventAction.hh  (为了支持多个EventAction同时调用)
	- wuVRunAction.hh  (为了支持多个RunAction同时调用)
	- wuVTrackingAction.hh  (为了支持多个TrackingAction同时调用)
- G4_lib
	- libwuG4mpi.so
	- libwuG4.so
- G4_src(不对外提供)
	- G4mpi
		- G4MPIbatch.cc
		- G4MPImanager.cc
		- G4MPImessenger.cc
		- G4MPIrandomSeedGenerator.cc
		- G4MPIRunMerger.cc
		- G4MPIScorerMerger.cc
		- G4MPIsession.cc
		- G4MPIstatus.cc
		- G4UImpish.cc
		- G4VMPIseedGenerator.cc
		- G4VMPIsession.cc
	- DataWrite.cc
	- wuActionData.cc
	- wuAssis.cc
	- wuBOptrChangeCrossSection.cc
	- wuBOptrMultiParticleChangeCrossSection.cc
	- wuBOptrMultiParticleForceCollision.cc
	- wuDigi.cc
	- wuDigitizerModule.cc
	- wuEventActionLog.cc
	- wuEventActionSD.cc
	- wuG4Authorize.cc
	- wuG4DataControl.cc
	- wuHistogramRand.cc
	- wuHit.cc
	- wuKitDetectorOptical.cc
	- wuNeutronHPphysics.cc
	- wuNeutronPhysics.cc
	- wuOpticalPhysics.cc
	- wuPrimaryGeneratorAction.cc
	- wuPrimaryParticleInformation.cc
	- wuPrimaryVertexInformation.cc
	- wuRegionInformation.cc
	- wuRunAction.cc
	- wuRunActionSD.cc
	- wuRunActionTimer.cc
	- wuRun.cc
	- wuScintillation.cc
	- wuScoreWriter.cc
	- wuSensitiveDetector.cc
	- wuSmearer.cc
	- wuSteppingAction.cc
	- wuTrackInformation.cc
	- wuTrajectory.cc
	- wuTrajectoryPoint.cc
	- wuVEventAction.cc
	- wuVRunAction.cc
	- wuVTrackingAction.cc
- Garfield
	- Examples
- inc
	- armadillo_bits(矩阵运算库)
	- Shark(数学库)
	- wu_CLHEP(该库来源于CLHEP简化)
	- AES.hh  (AES加密、解密)
	- armadillo  (引用它就会引用矩阵库)
	- Authorize.hh  (授权验证)
	- base64.hh  (base64编码)
	- CSmtp.hh  (用来发邮件)
	- md5.hh  (md5算法)
	- UEwasp67.hh  (有关水和水蒸汽性质参数查询)
	- UEwasp97.hh  (有关水和水蒸汽性质参数查询)
	- UEwasp.hh  (有关水和水蒸汽性质参数查询)
	- wuDataInterpolation.hh  (来自G4DataInterpolation.hh)
	- wuDataVector.hh  (来自G4DataVector.hh)
	- wuEmailNotice.hh  (用来自动发邮件)
	- wuFastVector.hh  (来自G4FastVector.hh)
	- wuFile.hh  (关于文件、文件夹操作)
	- wuGlobals.hh  (全局变量、函数等)
	- wu_HEP(有bug，已经弃用)
	- wuIntegrator.hh  (来自G4Integrator.hh)
	- wuMath.hh  (暂无内容)
	- wuMemory.hh  (有关动态数组空间的申请，来自lammps)
	- wuNcursesConsole.hh  (ncurses终端界面)
	- wuNumericalMethods.hh  (有关基本函数拟合、数据排序等)
	- wuPhysics2DVector.hh  (来自G4Physics2DVector.hh)
	- wuPhysicsLinearVector.hh  (来自G4PhysicsLinearVector.hh)
	- wuPhysicsOrderedFreeVector.hh  (来自G4PhysicsOrderedFreeVector.hh)
	- wuPhysicsVector.hh  (来自G4PhysicsVector.hh)
	- wuPolynomialSolver.hh  (来自G4PolynomialSolver.hh)
	- wuReadData.hh  (用来读取文本的字符串、数据等)
	- wuSimpleIntegration.hh  (来自G4SimpleIntegration.hh)
	- wuString.hh (来自G4String.hh)
	- wuSystem.hh  (关于终端操作)
	- wuTimes.hh  (有关计时、程序执行进度条)
- lib
	- libblas.so.3.2.1
	- liblapack.so.3.2.1
	- libshark.so.2.3.0
	- libwuAuth.so
	- libwuCLHEP.so
	- libwuGlobal.so
	- libwuSmtp.so
	- libwuWaterPro.so
- Manual
	- Garfield
	- Geant4
	- ROOT
	- CodeProjectManual.html
	- CodeProjectManual.md
	- CodeProjectManual.pdf (**整个程序包的说明书**)
	- ScientisicLinux6.html
	- ScientisicLinux6.md
	- ScientisicLinux6.pdf
	- ScientisicLinux72.html
	- ScientisicLinux72.md
	- ScientisicLinux72.pdf
- projectG4(Geant4单线程工程)
	- 待补充（这是一个存自定义ROOT数据结构示例。单线程当前用在存ROOT自定义数据结构上。预计4.10.03可能会修复多线程下G4与ROOT的冲突，到时候这个就改成自定义数据结构示例）
- projectG4MT(Geant4多线程工程)
	- Analysis
		- process.cc  (单线程数据处理程序，来自RootTemplet文件夹)
	- Authorize
		- Auth.wu
		- img
		- Log
		- README.html
		- README.md
		- README.pdf
		- RunNumber
		- UserInformation.txt
	- bin
	- build
	- include
		- wuActionInitialization.hh  (多线程框架)
		- wuBOptnSplitOrKillOnBoundary.hh (有关偏倚抽样)
		- wuBOptrGeometryBasedBiasing.hh (有关偏倚抽样)
		- wuDetectorConstruction.hh (几何体创建、光学属性设置、SD设置等)
		- wuElectroMagneticField.hh  (有关电磁场添加)
		- wuEMFieldBuilder.hh  (有关电磁场)
		- wuEventAction.hh  (暂时未用)
		- wuPhysicsList.hh  (物理过程)
		- wuStackingAction.hh  (有关堆栈)
		- wuTrackingAction.hh  (用来做跟踪标记设置)
		- wuWorkerInitialization.hh  (有关多线程的子线程)
	- README  (**Geant4模拟的使用说明**)
		- img
	    - Optical   (有关光学过程说明)
			- README.html
			- README.md
			- README.pdf
		- HowToCreateDetector.md  (**如何设置几何体**)
		- HowToRun.md  (**如何运行模拟程序**)
		- HowToUseFileFormatData.md  (**如何修改FormatData.txt文件**)
		- HowToUseFileOptical.md  (**如何修改Optical.txt文件**)
		- HowToUseFileReadData.md  (**如何修改ReadData.txt文件**)
	- src
		- wuActionInitialization.cc
		- wuBOptnSplitOrKillOnBoundary.cc
		- wuBOptrGeometryBasedBiasing.cc
		- wuDetectorConstruction.cc
		- wuElectroMagneticField.cc
		- wuEMFieldBuilder.cc
		- wuEventAction.cc
		- wuPhysicsList.cc
		- wuStackingAction.cc
		- wuTrackingAction.cc
		- wuWorkerInitialization.cc
	- cmake.sh
	- CMakeLists.txt
	- FormatData.in
	- init.mac
	- init_vis.mac
	- main.cc
	- Optical.txt
	- ReadData.txt
	- readHits.cc (多线程中暂时没用，用来存ROOT自定义数据结构d额)
	- vis.mac
	- wu.in
	- wu.out
- projectNCURSES(ncurses工程)
	- bin
	- build
	- include
	- src
	- CMakeLists.txt
	- main.cc
	- ReadData.txt
- projectROOT(ROOT工程)
	- bin
	- build
	- include
	- src
	- CMakeLists.txt
	- main.cc
	- ReadData.txt
	- rootlogoff.C
	- rootlogon.C
- projectRW(RadWare工程)
	- 待补充
- projectWU(C/C++工程)
	- bin
	- build
	- include
	- src
	- CMakeLists.txt
	- main.cc
	- ReadData.txt
- RadWare53(软件安装在这里)
	- bin
	- demo
	- doc
	- font
	- icc
	- .radware.bashrc
	- .radwarerc
- ROOT_inc
	- wuLinkDef.h
	- wuMainFrame.hh  (有关图形界面)
	- wuROOT.hh  (ROOT图片美化常用函数等)
	- wuROOTHistogramRand.hh  (谱分布的随机抽样)
- ROOT_src
	- wuMainFrame.cc
	- wuROOTHistogramRand.cc
- RootTemplet
	- MakeSelector
		- fastest  (单线程模板)
		- proof  (多线程模板)
		- simple  (最简单使用示例)
	- source  (待补充)
- ShellAndTemplate
	- wuMarkdownStyle.css
- SimulationPMT(模拟PMT信号输出，开发中)
	- bin
	- build
	- include
		- wuSimulationPMT.hh(2002模型已完成，2010模型未完成)
	- src
		- wuSimulationPMT.cc
	- Analysis.h
	- Analysis.C
	- CMakeLists.txt
	- main.cc
	- PMT.txt
	- ReadData.txt
	- rootlogoff.C
	- rootlogon.C	
- source(不对外提供)
	- Authorize(用来生成libwuAuth.so)
		- build
		- include
			- AES.hh
			- Authorize.hh
			- base64.hh
			- CSmtp.hh
			- md5.hh
		- lib
		- src
			- AES.cc
			- Authorize.cc
		- CMakeLists.txt
	- CLHEP(用来生成libwuCLHEP.so)
		- build
		- src(存放wu_CLHEP文件类所对应的.cc文件)
		- CMakeLists.txt
	- Geant4(用来生成libwuG4mpi.so、libwuG4.so)
		- build
		- CMakeLists.txt
	- Global(用来生成libwuGlobal.so)
		- build
		- CMakeLists.txt
	- Smtp(用来生成libwuSmtp.so)
		- build
		- include
			- base64.hh
			- CSmtp.hh
			- md5.hh
		- lib
		- src
			- base64.cc
			- CSmtp.cc
			- md5.cc
		- CMakeLists.txt
		- main.cc
	- WaterPro(用来生成libwuWaterPro.so)
		- build
		- src
			- UEwasp67.cc
			- UEwasp97.cc
			- UEwasp.cc
		- CMakeLists.txt
	- 授权文件生成VN(N为版本号)
		- ToUser
			- GetComputerParameters.cc
		- AES.cc
		- AES.hh
		- WriteAuthFile.cc
	- rw05.3.tgz(软件源码)
- src(不对外提供)
	- wuDataInterpolation.cc
	- wuDataVector.cc
	- wuEmailNotice.cc
	- wuFile.cc
	- wuIntegrator.cc
	- wuMath.cc
	- wuMemory.cc
	- wuNcursesConsole.cc
	- wuNumericalMethods.cc
	- wuPhysics2DVector.cc
	- wuPhysicsLinearVector.cc
	- wuPhysicsOrderedFreeVector.cc
	- wuPhysicsVector.cc
	- wuSimpleIntegration.cc
	- wuString.cc
	- wuSystem.cc
	- wuTimes.cc
- UsersGuide
	- armadillo
		- armadillo_nicta_2010.pdf
		- armadillos数学库使用例程.docx
		- example1.cpp
		- LAPACK函数介绍.doc
		- rcpp_armadillo_csda_2014.pdf
	- GSL
		- gsl-1.16.tar.gz
		- GSLReferenceManual.ps.gz
	- mpi
		- 高性能计算之并行编程技术(MPI并行程序设计).pdf
	- openMP
		- example(待补充)
		- OpenMP4.0.0.Examples.pdf
		- OpenMP4.0.0.pdf
		- OpenMP编程.ppt
		- OpenMp_并行程序设计.pdf
	- python(待补充)
	- RadWare
		- plot.txt
		- RadWareInstall.txt
	- Shark
		- examples(待补充)
	- WaterPro
		- main.cc
	- 技术储备(待补充)
		- clock
		- g4数据暂存
		- include
		- mpi
		- openMP
		- ROOT
		- string
		- thread
		- UDP
		- 打开文本多次写入
		- 调用系统命令
		- 断言
		- 获取目录下所有文件名
		- 静态变量
		- 判断文件是否存在
		- 文件操作
		- 文件夹的创建和删除
		- 压缩解压缩
		- 指针
		- 字符串与数字相互转换
		- duoxiancheng.cxx
		- sstream.cc

----

# 程序包各文件夹简要介绍

- cmake：该文件夹存放cmake的MODULE文件。比如有了 FindROOT.cmake 我们外部调用 ROOT 软件时候无需设置 ROOT 的路径，这样换用新版本的 ROOT 时候就不用改 CMakeLists.txt 文件了。
- G4_inc：该文件夹存放G4可重复利用代码模块。
- G4_lib：该文件夹存放G4_inc文件夹内类实现部分的动态链接库文件。
- G4_src(不对外提供)：该文件夹存放G4_inc文件夹内类实现部分文件。
- inc：该文件夹存放一些网上下载程序库、积累的可重复使用程序模块。
- lib：该文件夹存放inc文件夹内类实现部分的动态链接库文件。
- Manual：该文件夹存放CodeProject的使用手册及部分软件常用手册。
- projectG4(Geant4单线程工程)：复制到任意位置修改、添加自己代码。
- projectG4MT(Geant4多线程工程)：复制到任意位置修改、添加自己代码。
- projectROOT(ROOT工程)：复制到任意位置修改、添加自己代码。
- projectRW(RadWare工程)：复制到任意位置修改、添加自己代码。
- projectWU(C/C++工程)：复制到任意位置修改、添加自己代码。
- RadWare53(软件安装在这里)：RadWare软件安装位置
- ROOT_inc：：该文件夹存积累的可重复使用ROOT程序模块。
- ROOT_src(不对外提供)：该文件夹存放ROOT_inc文件夹内类实现部分文件。
- RootTemplet：该文件夹存放ROOT的一些模板。
- ShellAndTemplate：该文件夹存放脚本及各类非常规模板。
- SimulationPMT(模拟PMT输出)：该文件夹为光子到PMT输出的模拟及数字化插件模拟。
- source(不对外提供)：该文件夹内存放将各文件夹内源码编译成动态链接库的程序以及程序的授权使用相关内容。
- src(不对外提供)：该文件夹存放inc文件夹内类实现部分文件。
- UsersGuide：该文件夹存放网上程序库的使用教程、个人程序功能测试、技术储备等。

----

# 工程模板介绍

程序包主要提供以下几种工程：

- projectG4(Geant4单线程工程)**(待补充)**
- projectG4MT(Geant4多线程工程)
- projectROOT(ROOT工程)
- projectRW(RadWare工程)**(待补充)**
- projectWU(C/C++工程)

使用时将该工程复制到任意位置（**可修改名字，记得修改里面CMakeLists.txt设置的CodeProject文件夹路径**），修改、补充、添加自己相关内容的代码即可运行。

这里 Geant4 给出了单线程工程、多线程工程是因为当前的多线程下调用 ROOT 软件做数据存储存在问题，这里的多线程工程使用的是Geant4自带的简易 root、xml、csv 输出，只能存 Branch 结构数据。而 Geant4 调用 ROOT 软件可写任意自定义结构的数据输出存储，更利于数据处理。例如：一个 Event 存一个结构，方便多线程数据处理；继承 TObject 类可 ROOT 读取数据结构等等。所以可用单线程下调用 ROOT 软件，然后用脚本程序直接开多个任务代替多线程。



## projectG4MT(Geant4多线程工程)

以下是各文件夹、文件功能的简单介绍：

- Analysis：该文件夹内放置了文件 **process.cc**，是 ROOT 软件进行 TTree 结构数据进行 Int\_t MakeSelector(const char* selector = 0)、Long64\_t Process(const char* filename, Option_t* option = "", Long64\_t nentries = 1000000000, Long64\_t firstentry = 0) 数据处理的模板。可对 build 文件夹内模拟输出的 ROOT 文件进行批量分析处理。
- Authorize：中放置了 **Auth.wu**、**RunNumber**、**UserInformation.txt** 文件。**Auth.wu**文件为授权验证文件，该文件须由吴鸿毅(wuhongyi@qq.com)提供，里面包含授权机器信息、授权使用时间、授权使用次数、接收联网授权验证码邮箱等信息。**RunNumber**文件用来控制生成数据文件编号。**UserInformation.txt**文件用来写入接收提醒邮件的地址，在模拟结束时会自动向该邮件地址发送通知(**联网时才会发送成功**)。具体使用请查看当前文件夹下的README.pdf。
- bin：该文件夹可不使用。通过修改CMakeLists.txt文件可将编译生成文件放在该文件夹。如使用也需将复制到build的文件改成复制到本文件夹内。
- build：该文件夹是存放编译产生文件的。
- include：该文件夹是**存放使用者模拟需要添加、修改的类文件**。
- README：该文件夹存放该工程的使用说明。
- src：该文件夹是**存放使用者模拟需要添加、修改的类实现部分文件**。
- cmake.sh：执行该文件可代替编译该工程。
- CMakeLists.txt：该文件是程序编译所需要cmake工程模板。**记得修改指向CodeProject路径**。
- FormatData.in：该文件是用来控制数据输出的，第二列为输出的数据名称，不想输出的信息就将其对应的第一列I/D/C修改为#即可，具体使用请查看**README**文件夹内说明，**该文件需要懂得使用**。
- init.mac：该文件为模拟控制脚本，暂时不使用。
- init_vis.mac：该文件为模拟控制脚本，暂时不使用。
- main.cc：该文件为程序主函数，不需要任何修改。
- Optical.txt：该文件存放模拟光学光子过程需要的参数，**该文件需要懂得使用**。具体使用请查看**README**文件夹内说明。
- ReadData.txt：该文件存放模拟的输入信息，包括输出文件名、数据格式、线程数、粒子源信息、简单数据筛选等等，这是模拟的核心内容，**该文件需要懂得使用**。具体使用请查看**README**文件夹内说明。
- readHits.cc：多线程中，暂时没用
- vis.mac：图形显示时会被使用。
- wu.in：一种执行方式中使用，具体使用请查看**README**文件夹内说明。
- wu.out：一种执行方式中使用，具体使用请查看**README**文件夹内说明。

### 使用方法

将终端路径指向**build**文件夹，然后执行命令行：

```bash
$  cmake ..
$  make -jN   #N为线程数
$  ./wu vis.mac   #开启图形界面的模拟，主要用来看几何体
$  ./wu -l        #不开启几何体的模拟
```

更多更详细程序执行说明，请查看**README**文件夹内说明。

----

## projectNCURSES(ncurses工程)

以下是各文件夹、文件功能的简单介绍：

- bin：该文件夹可不使用。
- build：该文件夹是存放编译产生文件的。
- include：该文件夹是**存放使用者自己添加的类文件**。
- src：该文件夹是**存放使用者自己添加的类实现部分文件**。
- CMakeLists.txt：该文件是程序编译所需要cmake工程模板。
- main.cc：该文件为程序主函数，里面代码是ncurses终端界面的简单测试使用情况。
- ReadData.txt：该文件可用来存放程序运行的一些变化的输入参数，执行不同参数只需修改该文件而不需要重新编译，参数可通过 wuReadData.hh 提供的模板函数读取。

### 使用方法

将终端路径指向**build**文件夹，然后执行命令行：

```bash
$  cmake ..
$  make -jN   #N为线程数
$  ./wu       #执行
```

----

## projectROOT(ROOT工程)

该工程为 C/C++ 调用 ROOT 软件工程模板。支持 ROOT 交互界面、图形界面的。

以下是各文件夹、文件功能的简单介绍：

- bin：该文件夹可不使用。通过修改CMakeLists.txt文件可将编译生成文件放在该文件夹。如使用也需将复制到build的文件改成复制到本文件夹内。
- build：该文件夹是存放编译产生文件的。
- include：该文件夹是**存放使用者自己添加的类文件**。
- src：该文件夹是**存放使用者自己添加的类实现部分文件**。
- CMakeLists.txt：该文件是程序编译所需要cmake工程模板。
- main.cc：该文件为程序主函数，**自己的代码需要在这里添加**。
- ReadData.txt：该文件可用来存放程序运行的一些变化的输入参数，执行不同参数只需修改该文件而不需要重新编译，参数可通过 wuReadData.hh 提供的模板函数读取。
- rootlogoff.C：该文件为 ROOT 提供，ROOT 退出前时先执行它。
- rootlogon.C：该文件为 ROOT 提供，ROOT 启动时先执行它。可用来加载宏文件或者动态链接库文件等等。

### 使用方法

将终端路径指向**build**文件夹，然后执行命令行：

```bash
$  cmake ..
$  make -jN   #N为线程数
$  ./wu       #执行
$  .q         #退出
```

----

## projectWU(C/C++工程)

以下是各文件夹、文件功能的简单介绍：

- bin：该文件夹可不使用。通过修改CMakeLists.txt文件可将编译生成文件放在该文件夹。如使用也需将复制到build的文件改成复制到本文件夹内。
- build：该文件夹是存放编译产生文件的。
- include：该文件夹是**存放使用者自己添加的类文件**。
- src：该文件夹是**存放使用者自己添加的类实现部分文件**。
- CMakeLists.txt：该文件是程序编译所需要cmake工程模板。
- main.cc：该文件为程序主函数，**自己的代码需要在这里添加**。里面有一些类的使用、矩阵库、openMP并行、发通知邮件等的简单测试示例，使用时将其删除即可。
- ReadData.txt：该文件可用来存放程序运行的一些变化的输入参数，执行不同参数只需修改该文件而不需要重新编译，参数可通过 wuReadData.hh 提供的模板函数读取。

### 使用方法

将终端路径指向**build**文件夹，然后执行命令行：

```bash
$  cmake ..
$  make -jN   #N为线程数
$  cd ../bin  #到bin文件夹
$  ./wu       #执行
```

----

# 有关使用授权

Geant4工程的使用需要通过授权验证。授权大致可分为以下几种模式和其组合：

- 单机授权
- 多机授权，需要邮箱接收验证码
- 使用时间授权
- 使用次数授权

**单机授权**：用户通过 GCC 执行 GetComputerParameters.cc 程序，将会生成 加密参数.txt 文件，里面是用户电脑的用户名、CPU ID 信息。将该文件发给吴鸿毅（wuhongyi@qq.com），提供的 Auth.wu 文件中将会写入该信息。

**多机授权**：在 Auth.wu 文件中写入用户邮箱地址(单一邮箱授权，已开发)，或者直接读取 Authorize 文件夹内 UserInformation.txt 文件中用户填写的邮箱地址(多邮箱授权，暂未添加)。程序执行时邮箱将会受到验证码，将该验证码按提示输入即可完成验证。

**使用时间授权**：依靠读取电脑时间来验证时间是否在授权范围内总是可以通过修改电脑时间通过授权。所以程序判断程序执行时间是否在授权时间范围采用的方法是**联网获取格林威治时间**来比较。

**使用次数授权**：这个存在破解方法。给未授权用户增加使用障碍。

----

<!-- CodeProjectManual.md ends here -->
