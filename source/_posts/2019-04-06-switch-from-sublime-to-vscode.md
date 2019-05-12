---
title: 从 sublime 到 vscode
date: 2019-04-06 11:29:10
tags: vscode, sublime
category: life
comments: true
---

使用 sublime 已经好有几年了，不得不说，sublime 的确是一个性感的编辑器。但是有几个痛点却也一直没能解决。

<!--more-->

第一个痛点：markdown 预览问题。

虽然可以通过插件实现浏览器中预览，但是无法在同一页面中实时预览。

第二个痛点：截图无法粘贴。

不能快速截图直接粘贴。

相比之下，vscode 不存在上述两个痛点。针对预览问题，甚至可以选择 github 等不同风格的预览；针对图片问题，可以通过配置，图片粘贴-保存-加到 markdown 中一气呵成，无需多余操作。甚至可以通过*vs-picgo*等插件实现一键上传图床。
另外，vscode 的插件市场也特别丰富。

因此决定从 sublime 切换到 vscode，整个切换的过程非常平滑，只在 hexo 博客文章渲染数学公式的时候，发现对特殊符号，hexo 使用的 markdown parser 会先解决转义，然后交给 mathjax 渲染，因此之前的文章中，都对特殊符号做了转义；而 vscode 在采用 markdown preview enhanced 插件之后，不会做转义处理，导致之前文章中的公式预览出现问题。最后的解决办法是，
输入 `markdown preview enhanced: extend parser` 搜索出 parser.js, 修改如下

```js
onWillParseMarkdown: function(markdown) {
    return new Promise((resolve, reject)=> {
      markdown = markdown.replace(
        /\{%\s*asset_img\s*(.*)\s*%\}/g,
        (whole, content) => (`![](${content})`)
      ).replace(
        /\\([\_\{\[\}\\\*\]])/g,(whole, content) => (content)
      ).replace(
        /\{%\s*img\s*(.*)\s*%\}/g,
        (whole, content) => (`![](${content})`)
      )
      return resolve(markdown)
    })
```

也就是，先将特殊字符前面的转义斜杠去掉。

暂时还没发现有其他任何问题。

by the way, 楼下的花儿开了，真漂亮。

![](https://i.loli.net/2019/04/06/5ca825147487f.jpg)
