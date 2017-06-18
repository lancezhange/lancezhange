---
title: "用统计方法求积分"
date: 2014-10-30 19:53
comments: true
categories: tech
tags: math
keywords: 积分, 蒙特卡洛，统计
mathjax: true
layout: single-column
---

来看下面这个有趣的积分题目  
试用统计学方法证明  
<!--more-->
$$
\begin{align}
& \int_0^a\int_0^a\cdots\int_0^a max(x_1, x_2, \cdots, x_n)^{1-n}\, dx_1\, dx_2\,\cdots dx_n \\\
& = n\times a \tag {1} \label {1}
\end{align}
$$


注意观察到，\eqref{1}式这个多重积分，其各重积分的上下限都是相同的，而且被积函数的形式可以让人联想到最大次序统计量，题目中又提示了用统计学的方法，因此，我们想到，如果将$x_i(i=1,...n)$看做是$(0,a)$区间上独立的均匀分布随机变量，按照佚名统计学家公式，我们就可以将式\eqref{1}转化成一个新的随机变量的期望，而那个期望如果容易求算的话，问题就解决了（顺便提一下，如果新的期望不容易求算，可以用产生随机数的方法估计期望，这其实就是蒙特卡洛求积分的方法了）。

$$
\begin{align}
\eta: = max(x_1, x_2, \cdots, x_n)^{1-n} \qquad \qquad \qquad  \\\
其中， x_i(i=1,...n)为(0,a)区间上n个独立的均匀分布随机变量，则  \\\
E(\eta) = \int_0^a\int_0^a\cdots\int_0^a\[max(x_1,x_2,\cdots,x_n)^{1-n}\](\frac 1 a)^n \, dx_1\, dx_2\,\cdots dx_n \\\
\Rightarrow \eqref{1}式 = a^n \times E(\eta) \quad \qquad \qquad \qquad \qquad \qquad
\end{align}
$$

下面我们只需要求$E(\eta)$即可。设$\eta$的分布函数为G，密度函数为f

$$
\begin{align}
G(x)  = p(\eta \lt x) \\\
= p(x(n)^{1-n} \lt x) \\\
= p(x(n) \lt x^{\frac 1 {1-n}}) \\\
\Rightarrow f(x) = \frac n {a^n \times (1-n)} \times x^{\frac {2n-1} {1-n}}
\end{align}
$$

有了密度函数，求期望就是小菜一碟了，这里略去。易求得，$E(\eta) = \frac n {a^{n-1}}$，于是，$I = a^n * E(\eta) = n\times a$，证毕。

我们也可以用前面提过的蒙特卡洛方法求一下，验证一下结果，取 n =3, a =2, R代码如下

```r
    p = function(a=2,k=100000){
        n=3
        a1 = runif(k,0,a)
        a2 = runif(k,0,a)
        a3 = runif(k,0,a)
        ma = pmax(pmax(a1,a2), a3)
        eta = 1/(ma**2)
        meta = mean(eta) # eta 期望的估计
        meta*(a**n)      # 积分值的估计
    }
```

运行上述代码，按照默认参数，得到的结果应该和6差不多，说明结果无误。

下面再说一下一种错误但又极具诱惑力的解法，其想法是，最大值取任何一个都是等可能的，所以，不妨设

$$
max(x_1, x_2,\cdots, x_n)=x_n
$$

然后注意到，$x_n$取值的上下限仍然不变，但其他的就只能在 $(0,x_n)$的范围中取值，因此，原来的积分变为

$$
\begin{align}
\int_0^a\int_0^{x_n}\cdots\int_0^{x_n} {x_n}^{1-n}\, dx_1\, dx_2\,\cdots dx_n \tag{2} \label{2}
\end{align}
$$

\eqref{2}式积分容易求得为a，因此，答案为a. 这显然是错误的，但是，错在何处？  
这种想法错在，\eqref{2}式相当于先产生一个(0,a)上的均匀分布随机数$x_n$，然后就将其作为最大值，这个最大值$\gt \frac a 2$ 的概率显然是0.5，而原来的式子是要产生n个随机数，然后取其最大值，这个最大值$\gt \frac a 2$的概率为$1- (\frac 1 2)^n$，当n很大的时候，该值明显大于0.5，甚至趋于1，可见所谓的"不妨设"，其实是有妨碍的（因此，以后见到不妨设之类的语句，也要有所辨别，不能尽信），原题中的条件已然被改变了。

通过这道题，可以体会到佚名统计学家公式的强大之处，也可以加深对蒙特卡洛方法的理解，所以实在是一道不错的题目。据说是一道人大考研题。


