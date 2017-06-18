title: "Hexo博客主题更换：从 Yilia 到 Maupassant"
date: 2017-04-24 01:11:05
tags: [tool, hexo]
category: tech
comments: true
layout: single-column
---

博客从 Yilia 主题更换为了 Maupassant(莫泊桑). 因为觉得后者更为简洁。记录一下更换过程中遇到的一些问题。
<!--more-->

### 修改
1. 模板引擎，Yilia 用的是ejs，而 maupassant 用的是 jade(因为商标冲突的原因，已经改名为 Pug), 用如下语句安装
```
npm install hexo-render-pug --save
```

2. Yilia 用 stylus 作为CSS预处理器，使得CSS比较模块化的感觉，而 maopassant 就比较原始了，只有一个CSS文件style.scss，不过，极简嘛，没毛病。

3. 数学公式 
之前凡是有数学公式的都要在yml部分添加`mathjax: true`才能被正确渲染。另外，将对应的jade文件中如下部分
```
script(type='text/x-mathjax-config').
  MathJax.Hub.Config({
    tex2jax: {inlineMath: [['$','$'], ['\\(','\\)']]}
    });

script(type='text/javascript', src='https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-MML-AM_CHTML', async)
```
  替换为
```
script(type='text/x-mathjax-config').
  MathJax.Hub.Config({
  jax: ["input/TeX", "output/HTML-CSS"],
  tex2jax: {
    inlineMath: [ ['$', '$'] ],
    displayMath: [ ['$$', '$$']],
    processEscapes: true,
    skipTags: ['script', 'noscript', 'style', 'textarea', 'pre', 'code']
  },
  messageStyle: "none"
  });
script(type='text/javascript', src='http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS_HTML', async)
```

4. 行间距和段落间距
行间距和段落间距太大是这个主题最明显的缺点。
修改 `post-content` 对应的部分即可。 

5. 列表元素的上下边距也有点大
修改对应的 `ul, ol` 部分即可。
注意有序列表*适当缩进*以使序号连贯。

6. 代码高亮
之前用缩进标识代码块进而代码高亮，现在不行了，必须用三个撇号包围才行。

7. 引用
修改 `blockquote:before,.stressed-quote:before`(这里 before 其实指的就是前面引号这一部分) 中的color属性来更改引号的颜色，并在 `blockquote,.stressed`部分添加如下字体设置 
```
font-family: times;
    font-style: oblique;
    font-size: 16px;
    fort-weight: bold;
```
  这样之后，中文的引文会美观一些，但英文的还是怪怪的。

8. 评论
之前用的是多说，不过多说6月份就要歇菜了，所以换成了网易云跟帖，需要每篇文章中显式开启`comments: true`。注意本地测试的时候可能加载不上，推到线上方能看到。


### 最后
相比 Yilia, maupassant 的文章格式类型除了page, 新添加了 `layout: single-column` 单栏页面，比较适合文字密集型的长文。此外，`toc: true` 添加文章大纲也很有用。

希望借着这个更加简洁的主题，我能够重拾之前写博客的习惯，用文字记录下更多的东西，无论是生活中的杂感，还是技术类的分享。