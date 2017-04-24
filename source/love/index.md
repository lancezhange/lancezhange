---
layout: false
---

<html lang="zh">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>Love between Us</title>
    <link rel="stylesheet" href="./css/bootstrap.min.css">
    <link rel="stylesheet" type="text/css" href="./css/main.css">
</head>

<body>
    <audio src="./music.mp3" autoplay="true" loop="loop"></audio>
    <div id="back">
        <div class="mask"></div>
        <!-- 设置展示的图片，github pages不能有后台程序只能一条一条手动添加了 囧 -->
        <div id="carousel"  class="carousel slide carousel-fade carousel-position">
            <div class="carousel-inner" style="width: 100%;height: 100%;" id="background">
                <div class="item active" style="width: 100%;height: 100%;">
                    <div style="width: 100%;height: 100%;background:url(babe.jpg);background-size: cover;"></div>
                </div>
            </div>
        </div>
    </div>
    <div class="modal show" style="top:24%;">
        <div class="modal-dialog" style="opacity: .9">
            <div class="modal-content" style="opacity:.85">
                <div class="modal-header">
                    <h1 class="text-center" style="color: #A94442;font-family: 'JournalRegular', Arial, sans-serif;font-size: 7rem;">the times we together</h1>
                    <p class="text-center small-title">Johnny & Dawn</p>
                    <p class="text-center small-title">LOVING ON THE WAY</p>
                </div>
                <div class="modal-body text-center" style="line-height: 1.5rem;font-family: 'JournalRegular', Arial, sans-serif;font-size: 3rem;">
                    <p>
                        <span id="day" class="time-font"></span><span style="color:#A94442">/&nbsp;</span><span id="hour" class="time-font"></span><span style="color:#A94442">/&nbsp;</span><span id="minute" class="time-font"></span><span style="color:#A94442">/&nbsp;</span><span id="second" class="time-font"></span>
                    </p>
                    <p>
                        days/hours/min/sec
                    </p>
                    <p class="text-center" style="color:#A94442;font-size: 17px" id="say"></p>
                </div>
            </div>
        </div>
    </div>
    <script type="text/javascript" src="./js/jquery-1.12.2.min.js"></script>
    <script type="text/javascript" src="./js/bootstrap.min.js"></script>
    <script type="text/javascript" src="./js/count-time.js"></script>
    <script>
    $(function() {
        //设置起始日期
        countTime('2016/01/30 23:45', 'day', 'hour', 'minute', 'second');
        var days = $('#day').text();

        // 设置标题
        if (parseInt(days / 365) != 0) {
            $(document).attr("title", "在一起" + parseInt(days / 365) + "年,感谢相伴。");
        } else if (parseInt(days / 30) != 0) {
            $(document).attr("title", "在一起" + parseInt(days / 30) + "个月,感谢相伴。");
        } else
            $(document).attr("title", "在一起" + days + "天,感谢相伴。");

        //设置每一张图片对应的文字
        var says = new Array(
            "一路相伴,感谢有你"
        );


        var start = function() {
            var index = 0;
            var rate = 6000;
            $('#say').text(says[(index++) % says.length]);
         	var _play = function () {
         		$('#say').hide();
                $('#say').text(says[(index++) % says.length]);
                $('#say').fadeToggle();
                $('#carousel').carousel('next');
         	};
            setInterval(_play, rate);
        }();

    });
    </script>
</body>

</html>
