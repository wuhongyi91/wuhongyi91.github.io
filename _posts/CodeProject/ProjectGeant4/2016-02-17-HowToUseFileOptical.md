---
layout: post
title: "吴鸿毅 CodeProject - How To Use File Optical(Geant4)"
date: 2016-02-17 12:39:21 +0800
comments: true
tags: [Geant4]
---
<!-- HowToUseFileOptical.md --- 
;; 
;; Description: 
;; Author: Hongyi Wu(吴鸿毅)
;; Email: wuhongyi@qq.com 
;; Created: 三 2月 17 12:39:21 2016 (+0800)
;; Last-Updated: 六 7月 16 20:06:49 2016 (+0800)
;;           By: Hongyi Wu(吴鸿毅)
;;     Update #: 7
;; URL: http://wuhongyi.cn -->

# Optical.txt 文件内参数填写说明

## 吴鸿毅（wuhongyi@qq.com）

### 更新于 2016 年 02 月 17 日 - 北京大学 - 加速器楼

----

Optical.txt 文件包含光学过程的一些相关参数、材料的光学属性、光学表面参数等。

----

# 有关物理过程

## Cerenkov

~~~
MaxNumPhotonsPerStep   100       #默认100
MaxBetaChangePerStep   10.0      #默认10.0
~~~

- **MaxNumPhotonsPerStep**
- **MaxBetaChangePerStep**
<!--more-->
## Scintillation

~~~
ScintillationByParticleType  false  #true or false 

YieldFactor       0.65     #[0,1.0] scintillation yield factor

ExcitationRatio   1.0            #scintillation excitation ratio

FiniteRiseTime    true           #true or false 
~~~

- **ScintillationByParticleType**  光产生是否与粒子类型有关，and deposited energy in case of non-linear light emission in scintillators
- **YieldFactor**  光产生与粒子类型无关时(ScintillationByParticleType=false)。
- **ExcitationRatio**  FASTCOMPONENT、SLOWCOMPONENT成分都有时，如果 (ExcitationRatio=1.0或ExcitationRatio=0.0)，快成分所占比例为min(yieldRatio,1.0)；如果（0<ExcitationRatio<1），快成分所占比例为 min(ExcitationRatio,1.0)。
- **FiniteRiseTime**  是否考虑快（慢）成分上升时间;如若考虑材料得添加FAST/SLOWSCINTILLATIONRISETIME属性



## OpWLS

~~~
UseTimeProfile   delta    #delta或exponential
~~~

- **UseTimeProfile** 可填写delta或者exponential。

----

## 材料属性设置


----

## 光学表面参数

----



<!-- HowToUseFileOptical.md ends here -->
