title: "hexo 中的幻灯片"
date: 2015-11-24 09:39:43
tags: [hexo]
category: [tech]
layout: single-column
---


如果能在博客中插入幻灯片，想必是极好的。

第一种方式是嵌入式幻灯片，这可以像我们之前引入 jmpress.js 那样实现，不过那有点麻烦，直接用 `iframe` 其实就足够了。<!--more-->

比如，下面将我的 About 页面嵌入（点击之后，方向键即可翻页）

---

<iframe src="http://www.lancezhange.com/about/#/welcome？" height="400px" width="90%" align="center"></iframe>

仅仅用到如下代码而已
```
<iframe src="http://www.lancezhange.com/about/#/welcome？" height="400px" width="90%" align="center"></iframe>
```

因此推荐这种方式。但前提是你的幻灯片已经托管在了其他合适的地方。

---

第二种方式是全页面式幻灯片.

我们可以先写好 html 文件，然后只要不让 hexo 渲染它，而是直接拷贝过去就好了。但这样不仅麻烦，而且也有许多问题，比如，这样的幻灯片如何出现在摘要列表当中呢？ 当然，可以将 html 幻灯片作为新的页面生成，然后写一篇新的文章，里面包含指向该新页面的链接，正如[这里所做的](http://yeejlan.github.io/2014/07/10/host-blog-on-github-using-hexo/)那样, 新页面不会出现在摘要列表当中，自然也就没有了摘要页面错乱的问题，不失为一种解决办法，但有点治标不治本的感觉。

更好的解决方案是，直接定义一种新的 layout, 例如称为 slide (和 post, page 无异)，`hexo new slide [slides_name]` 即可生成模板，因为支持 markdown 语法，所以直接用 markdown 制作，制作完成后，用 hexo 生成 html 页面（完美复制 RStudio 中 markdown 制作幻灯片的丝滑享受）；我们可以自定义对该 layout 的渲染方式，以及相应的展现方式等，这样就完美了。

目前还没找到现成的代码，等我有时间自己写一个吧。





