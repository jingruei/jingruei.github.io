---
layout: post
title:  "第四話－擴充方法"
date:   2019-10-14 17:09:07 +0800--
categories: [EF]
tags: [EF]  
---


# 第四話－擴充方法

`Count`,`Max`,`Min`,`Sum`



元素＊１００再總和
```c#
List<double> list = new List<double>
{1.1,2.2,3.3,4.4};

var result = list.Avarge(x=>x*100)
```

字符串平均長度
```c#
List<string> str = new List<string> 
{"yahoo","google","pchome","eBay","amazon"};
var result = list.Average(str => str.Length)
```

計算產品總數、產品平均價格、產品最高價、產品最低價格
```c#
var mCOUNT  = db.Products.Count();
var mAVERAGE= db.Products.Average(p=>p.Price);
var mMAX    = db.Products.Max(p=>Price);
var mMIN    = db.Product.Min(p=>Price);
var mMUM    = db.Product.Sum(p=>Price);

```

|方法|IF SOURCE 是空的|如果 SOURCE 只包含一個元素|如果 SOURCE 有多個元素 |
|---|---|---|---|
First|抛异常|返回该元素|返回第一个元素|
FirstOrDefault|返回default(TSource)|返回该元素|返回第一个元素|
Last|抛异常|返回该元素|返回最后一个元素
LastOrDefault|返回default(TSource)|返回该元素|返回最后一个元素|
Single|抛异常|返回该元素|抛异常|
SingleOrDefault|返回default(TSource)|返回该元素|抛异常|