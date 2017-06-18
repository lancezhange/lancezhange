title: "MXNet 的安装和初试"
date: 2015-11-20 15:33:37
tags: [MxNet]
category: [tech]
layout: single-column
---
MXNet是一款开源的深度学习计算平台(yet another one, but a good one)，是分布式机器学习通用工具包[DMLC](http://dmlc.ml/) 的重要组成部分。MXNet具有轻量级、可移植性高、轻松支持分布式并行，显存利用率高、代码清晰、文档丰富全面等特点。<!--more-->

MXNet 的编译安装，除了[官方文档](http://mxnet.readthedocs.org/en/latest/build.html)之外，还可以参考 phunter_lau [所写的教程]( http://www.52cs.org/?p=639). 下面补充一些我遇到的问题。

我先是直接用 `make -j4` 编译了 CPU 版本，并安装了相应的 python 和 R 的扩展。但在用 `make rpkg ` 编译R包的时候，总是告诉我找不到 *DESCRIPTION* 文件（该文件用来对包的情况做一个整体说明，是每个R包都必须有的），但是这个文件明明在那儿！后来经过排查发现了原因。我们知道，一般我们打开R之后会用 `setwd()` 把工作目录切换到我们常用的某个目录下去，由于每次手动切换比较麻烦，所以我直接在 * /usr/lib/R/etc/Rprofile.site * 文件中写入了如下语句

```r
    .First <- function() {
        options(width=160)
        setwd("/home/lancezhange/programFactory/R")
        library(Rdym)
    }
```

`.First()` 函数会被R优先执行，因此每次打开R之后就不用再手动跳转目录了。难怪 R 无法找到当前目录下的 *DESCRIPTION*，工作目录都不在这儿了。所以，只要将这里的语句暂时注释掉，就可以顺利生成R包了，然后安装即可。顺便提一下，上面语句中加载的包 *Rdym* 是干什么用的呢。我们知道，R中的包和函数一箩筐，有些函数和变量的名字啥的起得特别烦人，一不小心就写错了，有时候还不知道自己哪里写错了，*Rdym* 包就是用来在你输错之后提示你一个比较相近的正确备选（利用字符串的编辑距离给出的），比如如果我输入的是``sharpiro.test()，它会返回

```
sharpiro.test()
Error: could not find function "sharpiro.test"
Did you mean:  shapiro.test  ?
```

对我这种经常拼写不正确的人确实很有帮助。

言归正传。后来又想编译一份 GPU 版的 MXNet，于是按照上述 phunter_lau 的教程中的做法，拷贝 *config.mk* 文件，并在其中修改相应的配置之后编译，但是爆出如下错误：

```
make: *** [build/ndarray/ndarray_function_gpu.o] Error 127
```

好像是因为找不到 cuda。由于我在安装 cuda 时已经把它加到系统的环境变量里了，所以没在*config.mk* 中显式给出 cuda 的路径，但不知道为什么 MXNet 居然找不到，不过没关系，加上 `use_cuda_path =  /usr/local/cuda-7.0 `就能顺利编译了。接着重新安装一下R包和 python 包就好了。

然后就能顺利跑通 mnist 例子了。值得一提的是，如果本地有 mnist 数据文件，就不用再下一遍了，直接将解压后的四个文件放到 examples/mnist/data 目录下，注意 data 目录要自己手动建立；没有的话就不用管了，`python mlp.py` 执行即可。下面是打印的部分日志

```
INFO:root:Epoch[17] Time cost=1.731
INFO:root:Epoch[17] Validation-accuracy=0.977300
INFO:root:Epoch[18] Train-accuracy=0.993417
INFO:root:Epoch[18] Time cost=1.743
INFO:root:Epoch[18] Validation-accuracy=0.978500
INFO:root:Epoch[19] Train-accuracy=0.994367
INFO:root:Epoch[19] Time cost=1.744
INFO:root:Epoch[19] Validation-accuracy=0.974200
​```

然后在 *mlp.py* 中修改一下，使用 GPU 看看效果。然而这时候爆出了错误：

>Please compile with CUDA enabled

好像这是由于我在编译GPU版本之前就编译过一遍（CPU 版本），但重新编译之前没有用 `make clean` 清理编译环境造成的，不过这好办，只需要先 `make clean`, 然后重新来一遍就好了。下面是用 GPU 的结果

```
INFO:root:Epoch[18] Train-accuracy=0.993950
INFO:root:Epoch[18] Time cost=0.540
INFO:root:Epoch[18] Validation-accuracy=0.974400
INFO:root:Epoch[19] Train-accuracy=0.994667
INFO:root:Epoch[19] Time cost=0.534
INFO:root:Epoch[19] Validation-accuracy=0.974800
```

可以看到，GPU 的加速还是很明显的。



