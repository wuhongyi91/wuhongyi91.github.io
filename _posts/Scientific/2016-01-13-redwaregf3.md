---
layout: post
title: RedWare gf3 Commands Available
date: 2016-01-13 20:42:27 +0800
comments: true
tags: [Linux,Nuclearphysics]
---

RedWare 软件 gf3 基本使用命令。
<!--more-->

| Commands | Means |
| :--- | :--- |
| AF [S]    |     自动寻找拟合光标两次点击的区域，S为灵敏因子，数值越大找到的峰越多，所以往小的设置建议1-4。拟合采用最小二乘拟合。 Auto-Fit: iteratively peak-find and peak-fit for a selected. region of the spectrum [with peak-find sensitivity S]    After the automatic fitting has completed, the program redraws the spectrum, and displays the limit, peak positions, and fit. It also lists the fitted parameters, areas and energies etc. on the screen. If you choose to do so, you are then free to modify the fit, by for example deleting or adding peaks (DP, AP), and/or fixing or freeing parameters (FX, FR, RW etc.), and then re-fitting with the modified setup using the FT command. | 
| NF      |       在光标两次点击的区域内手动用光标标志峰位置然后拟合 set-up a new fit for an arbitrary number of peaks (<16)  and do fit |
| FT [N]  |      N=1-15 : set-up for N peaks and do fit |
|         |     [N=0] : do fit using present parameter values 
|         |     N=-1 :  recalculate initial parameter estimates and do fit 。 It is equivalent to the pair of commands RF and FT. |
| AP      |      add a new peak to the fit set-up。 If you have a fit set up, but wish to add a peak to this fit, use the command AP. You will be prompted for the position of the new peak. The width of the new peak will be set to its initial starting value, so if the widths are free, but relative widths fixed, you will be given the option of reseting all non-fixed widths to their initial values also. If the new peak is not higher in energy than the previous last peak, you may also select to re-order the peaks in order of increasing energy. |
| DP [N]   |     删除拟合结果中编号为N的峰 delete a peak [peak no. N] from the fit set-up 。Use this command to delete a peak from a fit you have set up, if you wish to have fewer peaks. If you do not provide the number of the peak you want deleted, the program will prompt you for it. |
| WM [N]   |     change weighting mode (i.e. data error bars) |
|          |    N=1/2/3 : weight with   fit / data / another spectrum |
| RF       |     reset free parameters to their original estimates |
| FX [P]   |     fix additional parameters [par. P] or change fixed value(s) |
| FR [P,Q..] |   free additional parameters [pars. P,Q..] |
| RP N      |    free (N=1) or fix (N=0) relative peak positions to present values |
| RW N     |     free (N=1) or fix (N=0) relative widths |
| SW       |     change starting FWHM parameters, etc. |
| MA [N]   |     change limits for fit and/or peak positions |
|          |      [N=1-17 : change marker no. N] |
| DM       |     display markers |
| DF       |     在图上显示最近一次拟合的线 display fit for present parameter values |
| LP       |     在终端显示最近一次拟合的结果 list parameter values |
| SA N     |     N=1-20 : store fitted centroids and areas as one of 20 data sets |
|          |    N=-1 :   write stored values to disk file gf2.sto |
| SP FN    |     read in new spectrum  (FN = file name or ID) |
|          |      [SP-1 asks for new initial estimates etc for R, Beta, Step] |
| SP/C     |     read in new spectrum from matrix, using Cursor to enter limits |
| S+       |     read next spectrum in a multi-spectrum file (useful in cmd files) |
| S-       |     read previous spectrum in a multi-spectrum file |
| DS [I,J[,K]] | 显示谱   [in Ith of J parts of screen]  |
|              |  [I,J = 1,0  or K = 1  :  display whole spectrum] |
| OV [I,J[,K]] | overlay spectrum (i.e. no erase) [I,J,K as for DS] |
| OV -1    |     overlay spectrum using same counts scale as previous sp. display |
| SS lo,hi |     display many spectra (spectrum IDs lo through hi) in many small sub-windows of the graphics window |
| OS lo,hi  |    display many spectra (spectrum IDs lo through hi) overlayed |
| SD [max]  |    sets up new-Spectrum auto-Display for whenever you read a new spectrum [and clears screen after max spectra] |
| X0 N     |     X-axis lower limit (chs) = N|
| NX N     |     N>0 : range of X-axis (chs) = N |
|          |    N=0 : autoscaling of X-axis to display entire spectrum |
| Y0 N     |     Y-axis lower limit (counts) = N |
| NY N     |     N>0 : range of y-axis (counts) = N |
|          |    N=0 : autoscaling of Y-axis to largest counts/ch. | 
|          |    N=-1/-2/-3 : linear / square-root / logarithmic Y-axis |
| DL lo,hi |     alternative way of specifying display limits,equivalent to  "X0 lo"  plus  "NX (hi-lo+1)" |
| LIN      |     alternative way of specifying y-axis mode, same as  "NY -1" |
| LOG      |     alternative way of specifying y-axis mode, same as  "NY -3" |
| XA       |     set both origin (X0) and range (NX) for X-axis |
| YA       |     set both origin (Y0) and range (NY) for Y-axis |
| EX       |     展开光标两次点击的区域 |
| MU       |     move display up in spectrum using cursor |
| MD       |     move display down in spectrum using cursor |
| RD [1]   |     clear and redraw graphics display, [redraw and display all channels of spectra] |
| CO [N1,N2...] | Change color map [to N1,N2...] |
| HC      |      generate hardcopy of graphics screen |
| PF       |     set up peak search to be done on sp. display(for display purposes only) |
| CR [N]   |     call cursor  (hit X to exit)  [ display cursor at ch. N ] |
| SU [I,J] |     SU 累计光标两次点击道址区域计数   SU I,J 累计道址I、J之间的计数 |
| SB       |     累计光标两次点击道址区域减去本底后的计数 sum between channels, subtracting background |
| SC       |     set counts per ch. using cursor |
| PK        |    gives quick and reasonably accurate peak energies and areas.using auto-background-find and peak integration |
| AS X     |     add another spectrum to current sp.    (X = mult. factor) |
| AC X     |     add X counts to each channel in spectrum |
| MS X     |     multiply spectrum by factor X |
| MS FN    |     multiply spectrum by another sp.       (FN = file name) |
| DV FN    |     divide spectrum by another spectrum    (FN = file name) |
| AG       |     adjust gain of spectrum |
| CT N     |     contract spectrum by factor N |
| BG       |     try to generate a BackGround spectrum for current spectrum |
| CA       |     autocalibrate from source spectra and .sou files |
| WS FN    |     write spectrum to disk                 (FN = file name) |
| DU FN    |     dump parameters, markers etc to disk   (FN = file name) |
| IN FN    |     indump parameters etc from disk        (FN = file name) |
| EC       |     define / change / delete  energy calibration |
| EC FN    |     take calibration from ENCAL par. file  (FN = file name) |
| CF FN    |     take commands from disk command file   (FN = file name) |
|          |      CF CHK : check whether to proceed with command file |
|          |      CF END : end of command file |
|          |      CF CON : continue with present command file |
|          |      CF LOG : log all typed input to create a new command file |
|          |           (Use CF END to close new file.) |
| LU [FN]  |     create look-up table file              (FN = file name) |
| SL [FN]  |     create slice input file                (FN = file name) |
| WI       |     add window(s) to look-up table or slice file using cursor |
| DW       |     display windows as they are presently defined |
| ME       |     显示图形菜单栏 go to menu mode (get commands from menus) |
| DE       |     divide sp. by det. effic. (from an EFFIT.sto file) |
| HE       |     type list of commands and other help topics |
| HE/P     |     send this output to the default printer |
| HE [Topic] |   on-line help [on command or Topic (2 or 3 letters needed)] |
| ST       |     停止并退出程序 |


<br />
----
&#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160;**Update: 1. Last-Modified: 2016-01-13 21:05**
