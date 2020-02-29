---
title: python和highcharts制作图表
tags:
  - highcharts
  - python
date: 2014-05-06 19:44:34
---

有时候我们需要制作一些图表，单独使用python可以制作出但是不是特别好看，今天我们就用Django和highcharts神器来做出好看的图标，如想要详细了解highcharts请关注如下网站：[highcharts官网](http://www.highcharts.com/)  [highcharts中文网](http://www.hcharts.cn/index.php)


## 前端准备


```
<html>
<head></head>
<body>
<div id="container" style="height: 300px"></div>
</body>
<script type="text/javascript">
$(function () {
    $('#container').highcharts({
        chart: {
            type: 'column'
        },
        title: {
            text: 'ip统计'
        },
        xAxis: {
            categories: {{ip_time}}  }, //ip_time数据为：[20140501, 20140502, 20140503, 20140504, 20140505]
        yAxis: {
            min: 0,
            title: {
                text: 'ip数量 (ip)'
            }
        },
        plotOptions: {
            column: {
                pointPadding: 0.2,
                borderWidth: 0
            }
        },
        series: [{
            name: 'ip',
            data: {{ip_conut}}  //ip_conut数据为：[853, 821, 829, 1048, 1014]

        }]
    });
});
</script>
</html>
```

## 后端views实现


```
def ip(request):
if request.method == 'GET':
    ip_time = [20140501, 20140502, 20140503, 20140504, 20140505]
    ip_conut = [853, 821, 829, 1048, 1014]
    return render_to_response("app_detail.html", RequestContext(request, {'ip_time': ip_time, 'ip_conut': ip_conut}))

```
这里的数据只是个例子，更直观的展示出效果，真正项目中可能需要你在数据库中取出数据，自己折腾成list，我们看下效果：

![出图](http://opslinux.qiniudn.com/639887B8-0262-4186-9D9E-68481F846620.png)


