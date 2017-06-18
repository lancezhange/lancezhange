---
layout: post
title: "R Package With Fortran Code"
date: 2014-03-23 21:16
comments: true
categories: [tech]
tags: [R, Fortran]
---
R中编写函数是很方便的，所以一般情况下，如果对速度要求不是很高或者数据量不大，光用R就够了。但如果是计算密集型，各种循环嵌套你侬我侬又无法用apply家族向量化处理时，考虑用Fortran 或者 C 来提高代码速度是明智的选择。<!--more-->这种速度的提升是很可观的，我自己就做过一个实验，R的代码跑了一个多小时还出不了结果，用 Fortran 写成动态加载库调用，仅仅用了5分钟不到！  

在R中调用 Fortran 代码其实也不麻烦，只需将 Fortran 代码做成动态加载库，然后用 dyn.load() 函数载入，并用 .Fortran() 函数来调用动态库中的子例子程序。   
在R包中的 Fotran 代码也必须是动态加载库，置于 src 文件夹下，然后在 namespace 文件中用
useDynLib() 函数注册一下，再用 R 函数来封装即可。   

### 以下是几个需要注意的问题：  
1.  R包中能够调用的 Fortran 代码只能是子例子程序（subroutine），而不能是函数（function）！   
2.  R包中用到的子例子程序，其结果变量一定也要写到形参列表中！当用 .Fortran()函数调用时，它返回的是各个'形参'的结果，所以，如果你不把结果变量写到形参列表，就无法得到返回值！（这在R的官方文档 [writing R Extension](http://cran.r-project.org/doc/manuals/R-exts.html) 中写的很清楚，但这份文档实在包罗万象而显得太杂，导致我没有仔细看，当初遇到这个问题，深觉怪异，就去 stackoverflow 问，被人批了一顿，虽然不爽，但人家说的确实很对，从此收获一个教训，这是题外话）  
3.  子例子程序中间过程中要用到的耗费内存较大的变量也要写到形参列表中，否则R会在运行的时候直接奔溃！一个可能的解释是这样的：R在调用 Fortran 代码的时候，为子例子程序分配栈区，而栈一般是很小的，所以内存消耗太大的话就不够用了；但将变量写到形参列表中的话，R就会为这些变量分配堆区。我是在这个[邮件列表](https://stat.ethz.ch/pipermail/r-devel/2005-September/034570.html)上看到这个解决方法的。  

暂时就这些。     