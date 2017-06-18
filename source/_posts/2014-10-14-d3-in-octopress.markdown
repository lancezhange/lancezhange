---
layout: post
title: "D3 in Octopress"
date: 2014-10-14 12:37
comments: true
categories: [tech]
tags: D3
layout: single-column
---

[D3.js(Data-Driven Documents)](http://d3js.org/) 是 javascript 的一个库，用以做基于浏览器的数据可视化。<!--more-->D3 功能强大，能够做出非常炫酷的可视化作品，语法也相当简洁易学。因此，倘若能在 Octopress 博客中加入D3，想必是极好的。

如何做呢，其实也简单，下载 D3, 将 **d3.v3.js** 放到 **source/javascripts** 目录下，
然后在 **source/_includes/head.html** 中，在加载 octopress.js 的那一行之后，加上下面这一句

```    
    <script src="{{ root_url }}/javascripts/d3.v3.js" type ="text/javascript"> </script>
```

这样就不用每次手动加入D3的头部依赖了。然后要做的就是在文章中加入D3代码，如下
    
```        
        <scipt type ="text/javascript">
        // D3 codes 写在这里 
        </script> 
```

不过这里有一点需要特别注意的地方：D3中的

```
    d3.select("body")
```

是一条常见的语句，但在博客中最好不要这样用，因为这里 body 指的是最后生成的 html 的 body,而 rake generate 生成的 html 文档中，body 部分还加入了许多东西，比如侧边栏啊什么的，所以，最好自己写一个如下的 div

```
  <div id='#body1'>
  </div> 
```


然后用

```
    d3.select("#body1")
```

这样，按照 id 选取，就能把 D3 能够影响的范围控制在具有该 id 的 div 之中。  

下面看个例子吧。

<script src="http://d3js.org/d3.v3.min.js"></script> 

<style> 

.link {  
stroke: #666;
opacity: 0.6;
stroke-width: 1.5px; 
} 

.node circle { 
stroke: #fff; 
opacity: 0.6;
stroke-width: 1.5px; 
} 

text { 
font: 10px serif; 
pointer-events: none; 
} 

</style> 

<div id="body2">              
     可以拖动的点，用鼠标拖住点移动，或者点击/双击，看看会发生什么-----   
    
</div>


<script type="text/javascript"> 
var links = [ { "source" : "A", "target" : "B" }, { "source" : "A", "target" : "C" }, { "source" : "A", "target" : "D" }, { "source" : "A", "target" : "J" }, { "source" : "B", "target" : "E" }, { "source" : "B", "target" : "F" }, { "source" : "C", "target" : "G" }, { "source" : "C", "target" : "H" }, { "source" : "D", "target" : "I" } ] ; 
 var width = 700 
 height = 300 ; 
 
var nodes = {}

// Compute the distinct nodes from the links.
links.forEach(function(link) {
link.source = nodes[link.source] || 
(nodes[link.source] = {name: link.source});
link.target = nodes[link.target] || 
(nodes[link.target] = {name: link.target});
link.value = +link.value;
});

var color = d3.scale.category20();

var force = d3.layout.force() 
.nodes(d3.values(nodes)) 
.links(links) 
.size([width, height]) 
.linkDistance(80) 
.charge(-400) 
.on("tick", tick) 
.start(); 

var svg = d3.select("#body2").append("svg") 
.attr("width", width) 
.attr("height", height); 

var link = svg.selectAll(".link") 
.data(force.links()) 
.enter().append("line") 
.attr("class", "link"); 

var node = svg.selectAll(".node") 
.data(force.nodes()) 
.enter().append("g") 
.attr("class", "node") 
.on("mouseover", mouseover) 
.on("mouseout", mouseout) 
.on("click", click)
.on("dblclick", dblclick)
.call(force.drag); 

node.append("circle") 
.attr("r", 8)
.style("fill", function(d) { return color(d.value); });

node.append("text") 
.attr("x", 12) 
.attr("dy", ".35em") 
.style("fill", "steelblue")
.text(function(d) { return d.name; }); 

function tick() { 
link 
.attr("x1", function(d) { return d.source.x; }) 
.attr("y1", function(d) { return d.source.y; }) 
.attr("x2", function(d) { return d.target.x; }) 
.attr("y2", function(d) { return d.target.y; }); 

node 
.attr("transform", function(d) { return "translate(" + d.x + "," + d.y + ")"; }); 
} 

function mouseover() { 
d3.select(this).select("circle").transition() 
.duration(750) 
.attr("r", 16); 
} 

function mouseout() { 
d3.select(this).select("circle").transition() 
.duration(750) 
.attr("r", 8); 
} 
// action to take on mouse click
function click() {
d3.select(this).select("text").transition()
.duration(750)
.attr("x", 22)
.style("stroke-width", ".5px")
.style("fill", "#E34A33")
.style("font", "20px serif");
d3.select(this).select("circle").transition()
.duration(750)
.style("fill", "#E34A33")
.attr("r", 16)
}

// action to take on mouse double click
function dblclick() {
d3.select(this).select("circle").transition()
.duration(750)
.attr("r", 6)
.style("fill", "#E34A33");
d3.select(this).select("text").transition()
.duration(750)
.attr("x", 12)
.style("stroke", "none")
.style("fill", "#E34A33")
.style("stroke", "none")
.style("font", "10px serif");
}
</script>

<style type="text/css">
 circle {
  fill: #000;
  stroke: #000;
  stroke-width: 1.5px;
}
</style>

怎么样，还不错吧，反正我是醉了。

### 后记  
不学D3很久了，上面例子是抄的别人的代码。以后会慢慢把 D3 等有趣的东西捡起来，并用它们做些有趣的东西出来。


## 更新 2015-05-21
博客框架切换到 hexo 后，由于 hexo 也不会处理 `<script`等 html 标签，所以这篇文章中的例子还是能够如常展示；然而，由于一些我暂时还未解决的问题，原本文章中一次展示了两个例子，现在只能有一个，只好将第二个滑动的蛇的例子放到下一篇中去了。此外，D3.js 也是链接到官方库，而不是使用本地资源。