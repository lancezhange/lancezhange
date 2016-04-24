title: "jmpress.js in hexo"
date: 2015-11-23 18:02:37
tags: [javascript]
category: [tips, hexo]
---

Impress.js 确实非常炫酷，但如果能够将它嵌入页面当中，就更好了。利用 jmpress.js, 我们就能做到这一点。<!--more-->

jmpress.js 是 jQuery 版的 impress.js. 只需要包含 jQuery 和 jmpress 库，并引用对应的 css 就能用了。就是如此简单。

这里我们展示一个例子（取自[jmpress.js官网](http://jmpressjs.github.io/docs/examples.html)）。

每张幻灯片都是可以点击的哦。


<link href="http://jmpressjs.github.io/jmpress.js/examples/tab-control/style.css" rel="stylesheet" type="text/css" />
<script type="text/javascript" src="http://apps.bdimg.com/libs/jquery/2.1.4/jquery.min.js"></script> <script type="text/javascript" src="http://jmpressjs.github.io/jmpress.js/dist/jmpress.all.js"></script>


<div class="nav" id="nav1" data-template="nav1">
    <section>
        <h1>page 1</h1>
        <p>this is page 1.</p>
    </section>

    <section>
        <h1>page 2</h1>
        <p>this is page 2.</p>
    </section>

    <section>
        <h1>page 3</h1>
        <p>this is pag 3.</p>
    </section>

    <section>
        <h1>page 4</h1>
        <p>this is page 4.</p>
    </section>

    <section>
        <h1>page 5</h1>
        <p>this is page 5.</p>
    </section>

    <section>
        <h1>page 6</h1>
        <p>this is page 6.</p>
    </section>

    <section>
        <h1>page 7</h1>
        <p>this is page 7.</p>
    </section>

    <section>
        <h1>page 8</h1>
        <p>this is page 8.</p>
    </section>

    <section>
        <h1>page 9</h1>
        <p>this is page 9.</p>
    </section>

    <section>
        <h1>page 10</h1>
        <p>this is page 10.</p>
    </section>
</div>

<script type="text/javascript">
$(function() {
    $.jmpress("template", "nav1", {
        children: function(idx) {
            var distanceX = 200;
            var distanceY = 170;
            var config = {
                scale: 0.2,
                y: -1,
                x: ((idx%5) - 2) * (1/2),
                rotateY: 360,
                secondary: {
                    "": "self",
                    rotateY: 0,
                    x: 0,
                    y: 0,
                    scale: 1
                }
            };
            if(idx % 2 != 0) {
                config.y = -config.y;
            }
            config.x = config.x * distanceX;
            config.y = config.y * distanceY;
            return config;
        }
    });
    $('#nav1').jmpress({
        stepSelector: "section",
        containerClass: "jmpress",
        hash: { use: false },
        fullscreen: false,
        duration: {
            defaultValue: 3000
        }
    });
});
</script>


是不是很酷哈。


