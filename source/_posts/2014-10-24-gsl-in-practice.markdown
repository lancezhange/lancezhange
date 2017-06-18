---
title: "GSL 在 Ubuntu 下的安装和使用"
date: 2014-10-24 15:34
comments: true
categories: [tech]
layout: single-column
---

[GSL(GNU Scientific Library)](http://www.gnu.org/software/gsl/)作为三大科学计算库之一，除了涵盖基本的线性代数，微分方程，积分，随机数，组合数，方程求根，多项式求根，排序等，还有模拟退火，快速傅里叶变换，小波，插值，基本样条，最小二乘拟合，特殊函数等。<!--more-->下面介绍一下GSL的安装和使用。  

在 windwos 下是无法从源码编译安装的，为此，GNU 貌似还提供了一个特别为 cygwin 编译好的版本，不过我还是希望能从源码编译安装（其实是不想再深陷windows 下编译安装之波折痛苦），因此我这里选择在虚拟机安装的 ubuntu 12.04 上来编译安装GSL。

首先从官网下载到源代码（我用的版本是 gsl-1.9）压缩包，解压后进入目录，执行

```
      ./configure
      make
      make install
```

这个过程需要几分钟。这里还有一点需要注意的是，执行 `make install` 时，会自动将动态库和头文件分别拷贝到/usr/local/lib和 /usr/local/include 下面，但如果这两个目录没有写权限，就无法创建此二目录，导致安装失败，此时改用 'sudo make install'或者手动去赋予权限，便能解决此问题。

安装就此完成，下面来跑官网上的这个[样例](http://www.gnu.org/software/gsl/manual/html_node/An-Example-Program.html#An-Example-Program)

```
     #include <stdio.h>
     #include <gsl/gsl_sf_bessel.h>
     int main (void)
     {
       double x = 5.0;
       double y = gsl_sf_bessel_J0 (x);
       printf ("J0(%g) = %.18e\n", x, y);
       return 0;
      }
```

编译之(假设该文件存为 test.c)：

```
     gcc -lgsl -lgslcblas test.c -o test
```

然而，上面的语句并不能成功，出现错误提示信息：`undefined reference to gsl_sf_bessel_J0`，经过排查，发现是源文件和连接库的顺序颠倒，应该改为

```
     gcc test.c -lgsl -lgslcblas   test.c -o test   
```

这样就能顺利生成可执行文件了。

GSL 的更多例程用法，参考[官方指导手册](http://www.gnu.org/software/gsl/manual/html_node/), 更深入的了解还可以参考[GSL设计文档](http://www.gnu.org/software/gsl/design/gsl-design_toc.html)