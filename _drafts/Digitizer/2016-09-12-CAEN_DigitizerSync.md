---
layout: post
title: "CAEN Digitizer Synchronization"
date: 2016-09-12 14:26:20 +0800
comments: true
tags: [CAEN,Digitizer]
---
<!-- CAEN_DigitizerSync.md --- 
;; 
;; Description: 
;; Author: Hongyi Wu(吴鸿毅)
;; Email: wuhongyi@qq.com 
;; Created: 一 9月 12 14:26:20 2016 (+0800)
;; Last-Updated: 五 11月  4 20:50:17 2016 (+0800)
;;           By: Hongyi Wu(吴鸿毅)
;;     Update #: 7
;; URL: http://wuhongyi.cn -->

光纤直接连接插件，红色端在上，黑色端在下
<!-- more -->

# External Trigger Propagation



# Independent Channel Triggers

**黑色光纤端**连接为第一个插件，**红色光纤端**连接为最后一个插件。


This section will describe the synchronization of two boards with independent channel triggers (logic OR of the channel autotriggers and independent external triggers). The aim is to align the start command and then let the boards trigger independently. Triggers won’t be propagated, but the run start will be propagated through the daisy chain TRG OUT and the start input (S IN).

The boards are connected as in the follows:
 - Connect Master CLK OUT to Slave CLK IN using A317 cable.
 - Connect the TRG OUT of the Master to S-IN of the Slave.



----

N boards with independent triggers generated as OR of the local channel triggers

SETUP:  
daisy chain SIN-TRGOUT to propagate the start of run

START OF RUN: 
 - All boards: set start mode = start on SIN high level; 
 - All boards: set Trg Mask = Channels self trigger
 - All boards: set propagation of SIN to TRG-OUT
 - All boards armed to start
 - Send SW Start to 1st board 
 - The RUN signal is propagate through the daisy chain SIN-TRGOUT and start all the boards. Delay can be compensated by means of RUN_DELAY register
 - Once started, each board is triggered by the OR of the channel self-triggers

 - NOTE1: Start and Stop can also be controlled by an external signal going into SIN of the 1st board
 - NOTE2: the stop is also synchronous:  when the 1st board is stopped (by SW command), the stop is propagated to the other boards through the daisy chain.
 - NOTE3: in this configuration it is also possible to trigger the boards with individual external triggers (one per board) going into TRGIN



# Trigger from External Logic Unit





<!-- CAEN_DigitizerSync.md ends here -->
