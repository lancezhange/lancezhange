title: "Hello Echarts"
date: 2015-12-05 16:15:53
tags: [Echats]
category: [tech]
---
[Echarts](http://ecomfe.github.io/echarts/index.html), open sourced by Baidu, is a fantanstic javascript library for visualizing web data. It is easy to use, so let us give it a try.
<!--more-->

---

<div id="main" style="height:400px"></div>
<script src="http://echarts.baidu.com/build/dist/echarts.js"></script>
<script type="text/javascript">
    // configure for module loader
    require.config({
        paths: {
            echarts: 'http://echarts.baidu.com/build/dist'
        }
    });
    // use
    require(
        [
            'echarts',
            'echarts/chart/bar' // require the specific chart type
        ],
        function (ec) {
            // Initialize after dom ready
            var myChart = ec.init(document.getElementById('main'));
            var option = {
    tooltip : {
        trigger: 'axis'
    },
    toolbox: {
        show : true,
        feature : {
            mark : {show: true},
            dataView : {show: true, readOnly: false},
            magicType: {show: true, type: ['line', 'bar']},
            restore : {show: true},
            saveAsImage : {show: true}
        }
    },
    calculable : true,
    legend: {
        data:['蒸发量','降水量','平均温度']
    },
    xAxis : [
        {
            type : 'category',
            data : ['1月','2月','3月','4月','5月','6月','7月','8月','9月','10月','11月','12月']
        }
    ],
    yAxis : [
        {
            type : 'value',
            name : '水量',
            axisLabel : {
                formatter: '{value} ml'
            }
        },
        {
            type : 'value',
            name : '温度',
            axisLabel : {
                formatter: '{value} °C'
            }
        }
    ],
    series : [

        {
            name:'蒸发量',
            type:'bar',
            data:[2.0, 4.9, 7.0, 23.2, 25.6, 76.7, 135.6, 162.2, 32.6, 20.0, 6.4, 3.3]
        },
        {
            name:'降水量',
            type:'bar',
            data:[2.6, 5.9, 9.0, 26.4, 28.7, 70.7, 175.6, 182.2, 48.7, 18.8, 6.0, 2.3]
        },
        {
            name:'平均温度',
            type:'line',
            yAxisIndex: 1,
            data:[2.0, 2.2, 3.3, 4.5, 6.3, 10.2, 20.3, 23.4, 23.0, 16.5, 12.0, 6.2]
        }
    ]
};
            // Load data into the ECharts instance
            myChart.setOption(option);
        }
    );
</script>

---
Cool, huh?

Note: Above Chart is taken from [Echarts Examples Gallery](http://ecomfe.github.io/echarts/doc/example-en.html).

