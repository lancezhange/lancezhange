---
title: pandas 填充不完整时间序列
date: 2020-04-04 22:43:09
tags: pandas, time-series, 缺失值, 时间序列, python
category: tech
layout: single-column
---

最近在处理数据的时候遇到一个填充不完整时间序列的问题，这里记录一下解决方案。<!--more-->
假设数据如下

![](2020-04-04-22-56-52.png)

可以看到，Item_A 和 Item_B 各自的小时数据都不满 24 小时，下面我们进行补全。

思路： 利用 `reindex` 函数。

先定义一个帮助函数来获取完整 24 小时的日期索引

```python
from datetime import datetime, timedelta

def get_index(x):
    start = x['partition_date'].unique()[0]
    start_day = datetime.strptime(start, "%Y-%M-%d")
    end_day = start_day + timedelta(days=1)
    end = end_day.strftime('%Y-%M-%d')
    return pd.date_range(start, end, freq='1H')[:-1]

```

然后先构造 dh 作为时间类型的索引

```python

data["dh"] = data["partition_date"].map(str) + '-' + data["hour"].map(str)
data['dh'] = pd.to_datetime(data['dh'], format="%Y-%m-%d-%H")
data.set_index('dh', inplace=True)

```

然后对每个 item 的日期进行 24 小时补全

```python
result = data.groupby(['Item', 'partition_date']).apply(
    lambda x: x.reindex(get_index(x), method='nearest'))
```

最后将过程中产生的多级索引去除，并将 hour 等字段复位

```python
result = result.drop(['partition_date', 'Item', 'hour'], axis=1).reset_index()
result['hour'] = result.level_2.dt.hour
result.drop('level_2', axis=1, inplace=True)
result = result[['partition_date', 'hour', 'Item', 'value']]
```

填充后数据如下

![](2020-04-04-23-18-24.png)

可以看到，填充后，每个 item 的每个对应日期，都有 24 个小时的数值。

## 多余的话

最近又对 vscode 的 markdown 文档个性化渲染做了些修改，例如修改了背景颜色等。现在用 vscode 写起 markdown 简直不能太爽（可以说，这是最近半年以来我的工具链发生过的最美好的事情！）

下面是一个例子（看格式，不要关注内容）

![](2020-04-04-23-27-26.png)
