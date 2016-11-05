---
layout: post
title: "LaTex表格内单元格内容换行"
date: 2015-11-26 23:22:22 +0800
comments: true
tags: [Linux,Software,Latex]
---

&#160; &#160; &#160; &#160;如何同时让表格同一行一个单元格的文字能垂直居中？比如说文字超长超出页面范围需要分行显示。
<!--more-->
~~~
\newcommand{\tabincell}[2]{\begin{tabular}{@{}#1@{}}#2\end{tabular}}%放在导言区
%然后使用 \tabincell{c}{} 就可以在表格中自动换行
%比如这么用
\begin{tabular}{|c|c|}
\hline
1 & the first line \\
\hline
2 & \tabincell{c}{haha\\ heihei\\zeze} \\
\hline
\end{tabular}
~~~

以下为一例子，可直接存为.tex文件编译运行:
~~~
\documentclass[a4paper,12pt]{article}
\begin{document}
\begin{table}
\newcommand{\tabincell}[2]{\begin{tabular}{@{}#1@{}}#2\end{tabular}}
  \centering
  \begin{tabular}{|c|c|c|}\hline
1 & \tabincell{c}{the first line \\ the next\\the next\\ last} & \tabincell{c}{one \\ one}\\\hline
2 & \tabincell{c}{hello\\ aha\\ ok \\yes \\en} & \tabincell{c}{two \\ two \\ two} \\\hline
\end{tabular}
  \caption{longtitle}
\end{table}
\end{document}
~~~

----
法一：
~~~
\begin{tabular}{m{5cm}} 
~~~
法二：
~~~
\begin{tabular}{p{0.9\columnwidth}}
~~~
法三：multirow 宏包
~~~
\multirow{nrows}[bigstructs]{width}[fixup]{text}  
~~~
nrows 设定所占用的行数。bigstructs  此为可选项，主要是在你使用了 bigstruct 宏包时使用。width  设定该栏文本的宽度。如果想让 LaTeX 自行决定文本的宽度，则用 * 即可。fixup 此为可选项，主要用来调整文本的垂直位置。text 所要排版的文本，可用 \\ 来强迫换行。

----
&#160; &#160; &#160; &#160;内容摘自网上。

<br />
----
&#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160;**Update: 1. Last-Modified: 2015-11-26 23:27**
