---
layout: post
title:  "第五話－數據關連與繼承"
date:   2019-10-14 17:10:07 +0800--
categories: [EF]
tags: [EF]  
---

# 第五話－數據關連與繼承

一對多的例子
### Model 

一端：
```c#
public class OrderMaster
{
    public int Id {get; set;}
    public DateTime OrderDate {get ; set; }

    public virtual ICollection<OrderDetail> OrderDetails {get;set;}
}
```
最後一行的`OrderDetail`是`ICollection`集合對象，表示與此`OrderDetail`有關的所有`OrderMaster`對象
EF會根據此句建立兩者維護的關聯設置。

多端
```c#
public class OrderDetail
{
    public int Id {get; set;}
    public string ProductName {get ; set; }
    public int Quantity {get; set;}
    public int Price {get; set;}

    public int OrderDetailId {get; set;} //指定FK
    public virtual OrderMaster OrderMaster {get; set;}　
}
```


調用

```c#
var result = from bat in db.OrderDetails join bah in
             db.Orders on bat.OrderId eqauls order.Id
             select new { bat,bah };
             // select new report 
             // {
             //   Id=bah.Id,
             //   CustomerName=bah.Name,
             //   OrderDate=bah.Date
             //   ProductName = bat.Name
             //   Price = bat.price
             // }
```



--- 

## 加載(Loading)

#### 調整virtual屬性
- Lazy Loading：需要的時候再加載數據
  ` context.Configuration.LazyLoadingEnabled= false`這樣便不會自動載入關聯數據。
- Eager Loading:直接載入（預設）

#### Include方法與對象加載

`var result = db.Customers.Include(OrderMasters.OrderDetails).Include(OrderMasters.OrderDetails.Product").Where(c=>c.Id<5)`
