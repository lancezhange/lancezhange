---
layout: post
title: "Windows-怎样让一个窗口始终保持在最前端"
date: 2013-10-02 11:52
comments: true
categories: [tips]
---
今天一个同学过来问我，在Windows上如何将指定的窗口始终保持在最前端？<!--more-->虽然我并不知道如何去做，但感觉这应该不是什么难事，稍微 google 一下，就有了解决之法：    
1.下载一款叫 Autohotkey 的小软件  
   [点击下载](http://l.autohotkey.net/AutoHotkey_L_Install.exe)  
2.随便在哪儿建立一个新的文件（以 .ahk 作为文件名后缀），输入如下一行代码：  
     ^SPACE::  Winset, Alwaysontop, , A    

   然后保存文件。  
3.双击2中所建立的文件（没有什么反应是正常的，不要着急）。  
4.在所欲保持在最前端的窗口上， 点击热键： `Ctrl Space`. 这就完成了。  
5. 若要取消保持最前，再点击一次 `Ctrl Space` 即可。 
