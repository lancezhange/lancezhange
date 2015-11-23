title: "hexo 中使用 impress.js"
date: 2015-11-23 16:49:39
tags: [javascript]
category: [tips, hexo]
---

今天才发现自己用 impress.js 做的 about 页面挂了，这里记录下修复的过程，对 Hexo 中使用 impress.js 有需求的童鞋可以看看。<!--more-->

基本上，之前的 about 页面挂了的原因是指向 *impress-demo.css* 和 *impress.js* 的链接失效。所以修复其实很简单，将这些资源放到本地即可。不过有几点需要注意：

- 在 .md 文件中， 指向本地的 *impress.js* 总是无法正确渲染，总是出现 impress.js 那恼人的提示
> Your browser <b>doesn’t support the features required</b> by impress.js, so you are presented with a simplified version of this presentation.

排查了多次，也没找出什么原因，最后只好指向[百度静态资源公共库](http://cdn.code.baidu.com/)提供的地址。

- `.md` 文档的 yaml 头部写法

> `---`
  layout: false
  `---`

- `.md` 中直接上正常的 impress.js 幻灯片代码就行，不过，由于 .md 文档在被渲染的时候，将换行替换为*`<br>`*，导致最后生成的 html 文档中各种换行，影响到实际效果，因此需要去除代码中的换行，这也不是什么难事，用 sublime 的快捷键 `ctrl j` 就能连起来，很方便。顺便说一下，利用 sublime 的插件 **HTML-CSS-JS-Prettify** 就能还原并美化代码。

效果参见修复后的 [About 页面](http://www.lancezhange.com/about/#/welcome)。说明一下，本来 *Use a spacebar or arrow keys to navigate(使用空格或方向键继续)*这句话是在无反应一段时间后动态给出的提示，但这个效果也是一直出不来，因此我直接硬写入了（囧）。



