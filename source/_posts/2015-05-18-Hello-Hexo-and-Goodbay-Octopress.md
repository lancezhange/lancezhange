title: "Hello Hexo and Goodbay Octopress"
date: 2015-05-18 09:30:48
tags:  [hexo, octopress]
category: [tips]
---
So, Hello Hexo, and Goodbay Octopress
<!--more-->
## why move from octopress to hexo
Speed is the main concern. Octopress can be very slow , while hexo is lightening fast.
Second reason is  that i like this pretty theme [Yilia](https://github.com/litten/hexo-theme-yilia). Many thanks to its  author litten. 

## What changed
#### mathjax 
Some math formulas in my old octopress posts could not be rendered properly. So i changed contents of the file `/themes/yilia/layout/_partial/mathjax.ejs` to following:

    <!-- mathjax config similar to math.stackexchange -->
    <script type="text/x-mathjax-config">
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
    </script>
    <script src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS_HTML" type="text/javascript"></script>
Characters like `_` and `*` must be manually escaped to get mathjax working properly.

#### D3, Impress.js, P5.js
Those amazing posts i wrote in octopress which used d3/impress/p5 could not work anymore. Currently i do not have much time to make them work in hexo(you known, final exams are coming!), but i will do this in future.
