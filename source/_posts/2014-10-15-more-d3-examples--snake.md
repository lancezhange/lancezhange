---
layout: post
title: "More D3 Example -- Snake"
date: 2014-10-15 12:37
comments: true
categories: [tech]
tags: D3
layout: single-column
---
来看下面这个摆动的蛇的例子<!--more-->

<script src="http://d3js.org/d3.v3.min.js"></script> 
<div id="body1">

</div>

<script type="text/javascript"> 

var margin = {top: 40, right: 40, bottom: 40, left: 40},
    width = 960 - margin.left - margin.right,
    height = 500 - margin.top - margin.bottom;

var y = d3.scale.ordinal()
    .domain(d3.range(50))
    .rangePoints([0, height]);

var z = d3.scale.linear()
    .domain([10, 0])
    .range(["hsl(62,100%,90%)", "hsl(228,30%,20%)"])
    .interpolate(d3.interpolateHcl);

var svg = d3.select("#body1").append("svg")
    .attr("width", width + margin.left + margin.right)
    .attr("height", height + margin.top + margin.bottom)
  .append("g")
    .attr("transform", "translate(" + margin.left + "," + margin.top + ")");

svg.selectAll("circle")
    .data(y.domain())
  .enter().append("circle")
    .attr("r", 25)
    .attr("cx", 0)
    .attr("cy", y)
    .style("fill", function(d) { return z(Math.abs(d % 20 - 10)); })
  .transition()
    .duration(2500)
    .delay(function(d) { return d * 40; })
    .each(slide);

function slide() {
  var circle = d3.select(this);
  (function repeat() {
    circle = circle.transition()
        .attr("cx", width)
      .transition()
        .attr("cx", 0)
        .each("start", repeat);
  })();
}
</script>

如果觉得很酷，就快去学<a href="http://d3js.org/">D3</a> 吧。

注：忘记了当时从哪里抄来的代码，希望不要侵犯到别人的版权。
