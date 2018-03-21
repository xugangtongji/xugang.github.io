---
layout: post
title: 机器学习data库：Pandas
categories: 机器学习
comments: false
description: 
keywords: Pandas
mathjax: true
---

![](http://p5iojc2zy.bkt.clouddn.com/_posts/_image/2018-03-19-19-35-17.jpg)

pandas 是一种列存数据分析 API。它是用于处理和分析输入数据的强大工具，很多机器学习框架都支持将 pandas 数据结构作为输入。

**学习目标：**

- 大致了解 *pandas* 库的 DataFrame 和 Series 数据结构
- 存取和处理 DataFrame 和 Series 中的数据
- 将 CSV 数据导入 pandas 库的 DataFrame
- 对 DataFrame 重建索引来随机打乱数据


*pandas* 中的主要数据结构被实现为以下两类：

- **DataFrame**，您可以将它想象成一个关系型数据表格，其中包含多个行和已命名的列。
- **Series**，它是单一列。DataFrame 中包含一个或多个 Series，每个 Series 均有一个名称。

```python
city_names = pd.Series(['San Francisco', 'San Jose', 'Sacramento'])
population = pd.Series([852469, 1015785, 485199])
pd.DataFrame({ 'City name': city_names, 'Population': population })

california_housing_dataframe = pd.read_csv("https://storage.googleapis.com/mledu-datasets/california_housing_train.csv", sep=",")
california_housing_dataframe.describe()   #显示数据的样本数、均值、标准偏差、最大值、最小值和各种分位数。
california_housing_dataframe.head()     #只显示前几行数据
```

<center>
![](http://p5iojc2zy.bkt.clouddn.com/_posts/_image/2018-03-19-19-56-38.jpg)
</center>

数据索引和修改操作类似List，Dict，详见[文档](http://pandas.pydata.org/pandas-docs/stable/indexing.html)

apply 是 pandas 库的一个很重要的函数，多和 groupby 函数一起用，也可以直接用于 DataFrame 和 Series 对象。主要用于数据聚合运算，可以很方便的对分组进行现有的运算和自定义的运算。

```PY
>>> cities['City name'].apply(lambda name: name.startswith('San')
>>> #对cities中的"City name"进行列操作，判断str是否以“san”开头(不是以/t为截止)
"startswith" same as "endswith"
```

以下代码向现有 DataFrame 添加了两个 Series：

```py
cities['Area square miles'] = pd.Series([46.87, 176.53, 97.92])
cities['Population density'] = cities['Population'] / cities['Area square miles
```

![](http://p5iojc2zy.bkt.clouddn.com/_posts/_image/2018-03-19-21-08-06.jpg)
重新横向排序，重建索引是一种随机排列 DataFrame 的绝佳方式。
```py
cities.reindex([2, 0, 1])
cities.reindex(np.random.permutation(cities.index))
```
> np.random.permutation(List，N，Dict，Cell) #随机排序

如果您的 reindex 输入数组包含原始 DataFrame 索引值中没有的值，reindex 会为此类“丢失的”索引添加新行，并在所有对应列中填充 NaN 值：

![](http://p5iojc2zy.bkt.clouddn.com/_posts/_image/2018-03-19-23-02-12.jpg)


