---
title: "More D3 Example -- Rounded Rectangles"
date: 2014-10-16 12:37
comments: true
categories: [tech]
tags: D3
layout: single-column
---
下面是另外一个好玩的D3作品，出自[这里](http://bl.ocks.org/mbostock/1123639)，enjoy!<!--more-->

<script src="http://d3js.org/d3.v3.min.js"></script> 
<div id="body1">
</div>

<script>
var mouse = [480, 250],
    count = 0;

var svg = d3.select("#body1").append("svg")
    .attr("width", 960)
    .attr("height", 500);

var g = svg.selectAll("g")
    .data(d3.range(25))
  .enter().append("g")
    .attr("transform", "translate(" + mouse + ")");

g.append("rect")
    .attr("rx", 6)
    .attr("ry", 6)
    .attr("x", -12.5)
    .attr("y", -12.5)
    .attr("width", 25)
    .attr("height", 25)
    .attr("transform", function(d, i) { return "scale(" + (1 - d / 25) * 20 + ")"; })
    .style("fill", d3.scale.category20c());

g.datum(function(d) {
  return {center: [0, 0], angle: 0};
});

svg.on("mousemove", function() {
  mouse = d3.mouse(this);
});

d3.timer(function() {
  count++;
  g.attr("transform", function(d, i) {
    d.center[0] += (mouse[0] - d.center[0]) / (i + 5);
    d.center[1] += (mouse[1] - d.center[1]) / (i + 5);
    d.angle += Math.sin((count + i) / 10) * 7;
    return "translate(" + d.center + ")rotate(" + d.angle + ")";
  });
});

</script>
