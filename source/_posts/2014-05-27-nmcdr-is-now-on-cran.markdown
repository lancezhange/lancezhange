---
title: "R Package nmcdr is Now on CRAN"
date: 2014-05-27 07:40
comments: true
categories: [tech]
tags: [R]
layout: single-column
---

变点(change-point)理论是探测序列的潜在分布是否发生显著变化以及确定变化发生时刻的统计理论，自提出之日起，对它的研究文献可谓汗牛充栋，并且已经有了一些不错的成果，包括参数和非参数的方法、线性回归的方法，也有人将它纳入贝叶斯理论的的框架中去讨论。由于具有实际应用价值，变点理论和方法在机器学习、模式识别、质量控制、信号处理等领域都获得了广泛的应用<!--more-->。  

简单来说，变点（在有些文献中也称为‘断点’或者‘分割点’）模型适用于有序的随机数据序列，它试图将序列分成不同的区段，使得同一区段的数据大致遵循相同的分布，而不同区段之间的数据尽可能遵循不同的分布。不同区段的分界点就称作变点。  
按照变点个数的不同，变点问题可以分为单变点和多变点；针对一维数据和高维数据，也有不同的处理方法。   
作为统计分析领域最受欢迎的软件，R 中已经有许多软件包可以用来作变点分析，包括但不限于 bcp, cpm, ecp, changepoint 等。此外，根据[邹长亮](http://sms.nankai.edu.cn/teacher/szdw/xs004/51.html)等牛人的[一篇牛叉论文](http://www.e-publications.org/ims/submission/AOS/user/submissionFile/17525?confirm=abd32a18)中提出的牛叉的非参数方法编写的R包 [nmcdr](http://cran.r-project.org/web/packages/nmcdr/index.html)(目前版本 0.3.0 )也已经新鲜出炉，可于各大 CRAN 镜像站点下载使用。  

nmcdr 包中，最主要的函数就是 nmcd：  

```    
     nmcd(x, kmax, cpp, ncp, n)   
```

该函数用以对一维数据做变点分析，其中，参数 x 是数据序列，该参数也是唯一必须赋值的参数，其他几个参数都有默认的值，一般情况下不用调节，具体细节请参见该包的说明文档。下面我们来看一个例子。

```  
    # 这里假设你已经安装了需要用到的各个包      
    library(nmcdr)   
    data(Blocks)  
```

nmcdr 包自带的 Blocks 数据是一个人为构造的模拟数据，它包含11个真正的变点，其位置分别为 100， 130， 150， 230， 250, 400， 440， 650， 760， 780， 810. 我们来对它做变点检测：   
    
```
    t <- nmcd(Blocks)   
```

可以看到，nmcd 函数检测除了11个变点，分别为 102， 132， 158， 231， 251， 401， 441， 651， 761， 781， 811. 和真正的变点相比，差距微乎其微（当然，有许多评价变点估计优劣的准则，这里我们不去讨论它，只从直观上来感受），效果令人满意。    
对结果 t, 可以画图：  

```    
    plot(t)    
```

{% img http://pic.yupoo.com/lancezhange_v/DMOspifF/SoeNk.png %}  

此外，还可以返回包括 BIC 值在内的详细信息：  

```    
    summary(t)  
```

前面我们说过，变点方法在实际问题中也有很多应用，下面我们来看一个很简单的例子，虽然略显粗糙，但却能让我们直观地感受到变点方法的实用性。 
我们考虑图像处理领域中具有基础重要性的图像边缘检测问题，当然，这里我们只针对2D灰度图。所谓边缘，其实就是对比度较高，即亮度发生剧烈改变的点的集合。由于 nmcdr 包只能做一维数据的变点检测，所以我们将每一行（当然，每一列，或者每一对角线都行）作为一个序列来检测变点（确实很粗糙对吧），这些变点的位置应该大致就是边缘所在了。下面我们对 R 包 EBImage 中自带的一张图片做个演示，直接上代码：  

```    
    library(EBImage)   
    # vignette("EBImage-introduction")  
    library(nmcdr)  
    library(foreach) # 为了提高计算效率，采用并行计算  
    library(doParallel)  
    
    cl <- makeCluster(4)   # 创建一个具有4个节点的集群  
    registerDoParallel(cl)    
    
    # 读入图片
    f = system.file("images", "lena.png", package="EBImage")  
    lena = readImage(f)  
    # 用 display(lena) 来查看图片  
```

这张图片在图像处理领域真是随处可见啊  
{% img http://pic.yupoo.com/lancezhange_v/DMOsbku3/cQcrm.png %}      

```    
    mlena = imageData(lena)  
    dim(mlena)  
```

可以看到，该图像为一个 512 X 512 的灰度图，每一元素的值都为 0-1 之间的一个值，表示亮度  

```
    # 按照每一行数据来检测变点  
    rmlena = foreach(i = 1:nrow(mlena), .combine = rbind, .packages="nmcdr")   %dopar% {  
    t = nmcd(mlena[i,])$cpp  
    mlena[i,t]=1     # 边缘置白  
    mlena[i,-t]=0    # 背景置黑    
    mlena[i,]  
    }  
    # 将得到的数据存为图片  
    writeImage(rmlena,"lenaEdgeDetectionRowChangePoint.png")  
```

最终得到的结果如图：  
{% asset_img http://pic.yupoo.com/lancezhange_v/DMOspL8a/m1yem.png %}   
由于没有进行任何参数调节，也没有采取任何预处理措施，所以做到这样的结果已经可以让人满意了。 

