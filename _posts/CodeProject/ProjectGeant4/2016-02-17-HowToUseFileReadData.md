---
layout: post
title: "吴鸿毅 CodeProject - How To Use File ReadData(Geant4)"
date: 2016-02-17 12:38:55 +0800
comments: true
tags: [Geant4]
---
<!-- HowToUseFileReadData.md --- 
;; 
;; Description: 
;; Author: Hongyi Wu(吴鸿毅)
;; Email: wuhongyi@qq.com 
;; Created: 三 2月 17 12:38:55 2016 (+0800)
;; Last-Updated: 六 7月 16 20:10:07 2016 (+0800)
;;           By: Hongyi Wu(吴鸿毅)
;;     Update #: 70
;; URL: http://wuhongyi.cn -->

# ReadData.txt 文件内参数填写说明

## 吴鸿毅（wuhongyi@qq.com）

### 更新于 2016 年 02 月 17 日
### 更新于 2016 年 03 月 23 日 - 北京大学 - 加速器楼

----

ReadData.txt 文件存在以下两种参数。

~~~
ParameterName    (int/double/string)Data
~~~

第一个为变量名（不能修改），后面为我们需要修改的int/double/string型数据。

~~~
ParameterName    (int)Number  (int/double/string)Data1  (int/double/string)Data2 ...
~~~

第一个为变量名（不能修改），第二个为int型数值，指定其后面的int/double/string型数据个数，再后面则为int/double/string型数据。
<!--more-->

----

# 有关数据输出部分

~~~
FormatName  FormatData.in    # branch control
FileFormat  root            # 可选 root,xml,csv
FileName  wuData
OutputStyle  1  # 1：采用SD输出，文件名为下面的FileNameSD    0：采用step输出，文件名为下面的FileNameStep
FileNameSD  wuSDData
FileNameStep  wuStepData
~~~

- **FormatName**是用来控制想要输出的参数的，输出文件结构是一个变量一个Branch结构，当前只提供了 FormatData.in 一个选项，关于它的使用请查看**HowToUseFileFormatData.pdf**文档。FormatData.in 是一个相当完整的输出选项，提供了40多个可供输出的变量。但是其对于有大量次级产物的模拟并不适合，以后还会提供适合于光学光子模拟的输出参数控制，以及自定义类型的数据结构输出存储。
- **FileFormat**是用来控制输出数据文件格式的。提供 root、xml、csv 三种数据格式输出。
- **FileName**(暂时未用)。
- **OutputStyle**可填写参数0或者1。用来控制采用SD输出还是Step输出。采用SD输出则填写后面的FileNameSD、采用Step输出则填写后面的FileNameStep，以此来控制输出文件名。
- **FileNameSD**采用SD输出时的文件名。例如填入wuSDData，而FileFormat填root时，多线程下输出的数据文件名为wuSDData\_tN.root，其中N为线程编号。关于文件名带有Run Number的使用，请查看**Authorize/README.pdf**文档，例如输出的数据文件名为wuSDDataRxxxx\_tN.root，其中R为Run的缩写，xxxx为四位数字。
- **FileNameStep**采用Step输出时的文件名。具体同FileNameSD类似。

----

# 有关多线程线程使用个数

~~~
NumberOfThreads  4
~~~

- **NumberOfThreads**后面填写int型数据，该数据表示执行程序时将会调用的线程数。

----

# 关于程序结束邮件通知

~~~
EmailNotice  true      #true、false  程序运行完成是否邮件通知
EmailSendFile 3  FormatData.in Optical.txt ReadData.txt
~~~

- **EmailNotice**后面可填写true或者false。如果填写true，在联网正常的情况下将会收到提醒邮件。邮件内容包括模拟粒子数、程序模拟花费时间、计算机IP等信息。计算机IP是方便远程控制使用。
- **EmailSendFile**后面第一个为int型数据，之后为文件名。该文件为发邮件通知时候要发送的附件。

----

# 存储前对数据进行筛选

## following cuts performed in SD and Step

~~~
fEDepByParticle  0
FilterEdep  0    #unit MeV
FilterPwith  0 gamma alpha  e-    #priori to without
FilterPwithout  0 gamma
FilterInStep  -1    #-1: ignore >3: not record 1~3:diff algo
FilterOutStep  -1    #-1: ignore
~~~

- **fEDepByParticle**(暂时未用)。
- **FilterEdep**后面填写double型数据，其单位为MeV。表示该Hit(Step)能量沉积大于等于该数值才存储该条信息。**以后还会添加大于该数值才存储的功能**
- **FilterPwith**后面第一个参数为int型数据，之后为string型数据。假设int型数值N，表示其后面的N个string型参数有效。表示粒子名是这几个其中之一的该entry信息才储存。0时表示不开启该功能，开启该功能时FilterPwithout失效。
- **FilterPwithout**用法同FilterPwith。表示粒子名是这几个其中之一的该entry信息不储存。
- **FilterInStep**(不用)。。。参数为int型数据，负数表示忽略该功能，请勿修改。
- **FilterOutStep**(不用)。。。参数为int型数据，负数表示忽略该功能，请勿修改。

## following cuts performed in Step

~~~
FilterRegion  0     #0:ignore    1:只输出Region信息
~~~

- **FilterRegion**后面可选参数0或者1。当采用SD方式输出时，只会记录设置了SD的逻辑体。而对于采用Step方式输出时则记录所有的逻辑体，这在大多时候都是没必要的。所以添加此功能是用来筛选一定逻辑体内的数据输出的。使用该功能需要在创建探测器的时候对其逻辑体进行区域设置，详细说明请查看**HowToCreateDetector.pdf**文档。需要使用G4Region、wuRegionInformation类等。


----

# 有关程序执行log记录

~~~
SaveLogFile 3  FormatData.in Optical.txt ReadData.txt
~~~

- **SaveLogFile**后面第一个为int型数据，之后为文件名。该功能为程序启动授权验证通过后会在**Authorize/Log**文件夹内生成一个名字为当前**年月日时分秒**的文件夹，并将后面填写的文件及**Authorize/RunNumber**文件复制到该文件夹内。可用来追溯每一次模拟的相关参数。

以下三行功能暂时不提供，**等待改进支持多线程**，应该会开发跟踪某一线程执行情况来代替。

~~~
FileNameEventLog  writelog
SecMaxShow  900
EvtMaxChk  1
~~~

----

# 有关物理过程

## 通用设置

~~~
ParticleRangeCut  0.7   #mm
lowLimit  -1.     #eV,It will take effect on lowLimit>=0.0 && highLimit>=0.0
highLimit  -0.5    #GeV,It will take effect on lowLimit>=0.0 && highLimit>=0.0 
TrackingLimitParticle  0 opticalphoton
~~~

- **ParticleRangeCut**后面为double型数据，单位为mm。为截断阈值。
- **lowLimit**后面为double型数据，单位为eV，只有与highLimit同时大于等于0时候才开启功能。该功能用来限制产物能量区间。
- **highLimit**后面为double型数据，单位为GeV，只有与lowLimit同时大于等于0时候才开启功能。该功能用来限制产物能量区间。
- **TrackingLimitParticle**(暂时未用)。

## 0-4eV热中子

~~~
Thermal  0  # 1：调用0-4eV热中子弹散模型，0：不调用0-4eV热中子弹散模型
~~~

- **Thermal**后面可选参数0或者1。该参数只有在调用了wuNeutronHPphysics、wuNeutronPhysics模型时，对一些特殊材料有效。调用该模型还需要在创建探测器时对材料进行特殊设置。

----

# 有关电磁场

电磁场具体的使用请查看**HowToCreateDetector.pdf**文档

~~~
MinStep  0.001
StepperType  4
~~~

- **MinStep**后面为double型数据，太久没用已经忘记干啥了。。。
- **StepperType**后面为int型数据，太久没用已经忘记干啥了。。。

----

# 粒子源

~~~
EnableRandSeed  1      # >0 set random seed as time
NumbPrimaryPerEvt  1
~~~

- **EnableRandSeed**后面为int型数据，其数值大于0时，程序的随机种子与时间相关，反之则随机种子是固定的。
- **NumbPrimaryPerEvt**后面为不小于1的int型数据，该数值表示一个Event发射多少个粒子。

## parimary particle name(opticalphoton or neutron,triton, alpha,proton)

~~~
NamePrimary  neutron
IonZ  4
IonA  14
IonEstar  0.0
IonQ  4.
~~~

- 定义入射粒子种类。
- **NamePrimary**后面为string型参数，可填Geant4定义的任意基本粒子，或者填ion自定义带电粒子。
- **IonZ**后面为int型数据，NamePrimary为ion时生效。其数值表示质子数。
- **IonA**后面为int型数据，NamePrimary为ion时生效。其数值表示核子数。
- **IonEstar**后面为double型数据，NamePrimary为ion时生效。其数值表示激发能。
- **IonQ**后面为double型数据，NamePrimary为ion时生效。其数值表示所带电荷。

## kinetic energy of primary particle(MeV, photon 2.85 eV)

~~~
EkPrimary  1    2.0	 2.0615    2.7185	 
EkWeight   1    1.
~~~

- 定义入射粒子能谱。支持单能、连续谱。需要EkPrimary、EkWeight配合使用。除粒子为photon单位为eV，其它粒子时候单位均为MeV。
- **EkPrimary**后面第一个为int型数据，之后为double型数据。假设int型数值N，表示其后面的N个double型数据有效。当int型数值为1时为单能，大于1时为连续谱。
- **EkWeight**后面第一个为int型数据，之后为double型数据。对应EkPrimary每个能量点的能谱数值。依次直线连接点（EkPrimary，EkWeight），在其构成的能谱图上均匀抽样。

以下为连续能谱测试：

~~~
EkPrimary   8    2.0  2.1  2.3  2.4  2.5  2.7  2.8  3.0	 
EkWeight    8    1.0  2.0  1.0  3.0  1.0  5.0  2.0  7.0
~~~

![连续能谱测试效果](/img/G4RandomEnergy.png)

## primary particle position(mm),z position is given by program

~~~
xInitPrimary  0.0     #-5.0e-4
yInitPrimary  0.0
zInitPrimary  -550.0
~~~

- 定义入射粒子发射位置。其为World中的绝对坐标，单位为mm。
- **xInitPrimary**  入射位置的x轴坐标，为double型数据。
- **yInitPrimary**  入射位置的y轴坐标，为double型数据。
- **zInitPrimary**  入射位置的z轴坐标，为double型数据。

## primary particle position smearing range(mm)

~~~
xRangePrimary  0
yRangePrimary  0
zRangePrimary  0
~~~

- 定义入射粒子发射位置，在前面的绝对坐标中扩展成线源、矩形面源、长方体源、圆形面源、圆柱体源等。
- **xRangePrimary**、**yRangePrimary**、**zRangePrimary**分别为粒子源位置在x、y、z三个方向的取值区间长度，为double型数据，单位为mm。假设前面设置粒子源x轴位置为X，则实际粒子源x轴位置为在区间[X-xRangePrimary/2,X+xRangePrimary/2]随机选取。yRangePrimary、zRangePrimary同理。这样依靠xRangePrimary、yRangePrimary、zRangePrimary均不小于0，可组合出线源、矩形面源、长方体源情况。
- 如果**xRangePrimary小于0**的数值时，则**yRangePrimary**无效，此时粒子源的x、y坐标位置为以（X，Y）为原点，半径为 -xRangePrimary 的圆内均匀分布。Z坐标仍然同上。此时可组合出面向z轴的圆形面源、圆柱体源情况。

## primary direction rotation respect to X/Y/Z axis(unit:degree)

~~~
DirectRotX  0
DirectRotY  0
DirectRotZ  0
~~~

- 定义入射粒子发射方向。默认沿z轴正方向，旋转操作相对于(0,0,1)方向。
- **DirectRotX**、**DirectRotY**、**DirectRotZ**均为double型数据，单位为度。分别表示发射方向(0,0,1)沿x、y、z轴旋转该角度。

## primary direction range(unit:degree)

~~~
RangeThetaPri  0.
~~~

- 定义沿前面设置的发射方向 RangeThetaPri角度所张锥角内均匀发射。
- **RangeThetaPri**为double型数据，其取值范围为[0.0,180.0]。0.0表示沿前面设置方向，90.0表示前向均匀发射，180.0表示4$\pi$空间均匀发射。


----

# 光学参数设置

~~~
OpticalFile  Optical.txt
~~~

- 定义材料的光学属性、光学表面参数等。
- **OpticalFile** 后面为string型参数，为一文件名字，该文件里面存有光学材料属性参数、光学表面参数等。详细使用请查看**HowToUseFileOptical.pdf**文档。

----

<!-- HowToUseFileReadData.md ends here -->
