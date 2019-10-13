---
layout: post
title:  "第三話－實體數據模型與LINQ"
date:   2019-10-14 17:08:07 +0800--
categories: [EF]
tags: [EF]  
---

# 第三話－實體數據模型與LINQ
---
#### 1.類似於`Select * From Product`

> LINQ－

`var result = from product in db.Product select product;`

> Lambda－

`var result = db.Product.Select(p=>p);`

  - 以上皆可利用`foreach(var p in result)`將某個字段逐一取出。
  - `Debug.WriteLine(p.Name);` 可以用於ASP.NET CORE 的視窗輸出

---
#### 2.使用自訂義欄位不返回Model，如`SALE_PRICE` 

> LINQ－
```c#
            var result = await db.Product.Select new
            { PRO_NAME = p.Name }.ToListAsync();
            return Ok(result);
```



> Lambda－
```c#
            var result = await db.Product.Select
            (p => new { PRO_NAME = p.Name }).ToListAsync();
            return Ok(result);
```

---

#### 3.使用封裝方法轉換類型

> Model－
```c#
public class SProduct{
    public string Pid{get; set;}
    public string CName {get; set;}
    public int Price {get; set;}
    public double SPrice {get;set;}
}
```
> Controler－
```c#
var result = from product in db.Product.ToList() select ToSprocut(product);


//封裝方法
static SProduct ToSproduct(Product product)
{
    return new Sproduct()
    {
        PID = product.Id.ToString().PadLeft(4,'0'),
        CName=product.Category+"_"+product.Name,
        Price=product.Price,
        Sprice=product.Price*0.8
    }
}
```
---

#### 4. 多重from

```c#
string[] weekMonth = {
    "JANUARY,FEBRUARY,MARCH,APRIL,MAY,JUNE,JULY,AUGUST,SEPTEMBER,OCTOBER,NOVEMBER,DECEMBER",
    "Monday,Tusday,Wednesday,Thursday,Friday,Saturday,Sunday"};
}
var result = from a in weekMonth
             from b in a.Split(',')
             select b.Substring(0,3);

var StringOutPut= "星期與月份英文縮寫:";
foreach(var d in result)
{
 StringOutPut2+=d
}


```
---

#### 5. Where


> 查找價錢介於80-160的產品

> LINQ－
```c#
var result = from p in db.Products
             where p.Price>=80 && p.Price <=160 
             select p;
```


> LAMBDA－
```c#
var result = db.Products.Where(p => p.Price >=80 && p.Price<=160 );
```

---
#### 6. 排序
|方法|說明|
|---|---|
|OrderBy|升序排序|
|OrderByDescending|降序排序|
|ThenBy|二次升序排序|
|ThenByDescending|二次降序排序|
|Reverse|反轉集合中的對象排列順序|

> LINQ－
```c#
var result = from p in db.Products
             orderby p.Price
             select p; 
```

> LAMBDA－       
```c#
var result = db.Prodcuts.OrderBy(p=>p.price).ThenBy(p=>p.Category)
```

#### 7. Join

> LINQ－
```c#
var result = from c in catrgories join b in books
             on c.Id equals b.CategoryId select new 
             {
                 BookCategory = c.Name, BookTitle= b.Name
             }
```

> LAMBDA－       

```c#
result = catrgories.Join(books, 
                c=>c.Id,
                b=>b.CategoryId (c,b) =>
new 
{
    BookCategory = c.Name , BookTitle = b.name
});
```

#### 8. To SQL
```c#
  var studentList =     db.Students
                        .SqlQuery("Select * from Students")
                        .ToList<Student>();
```

```c#
    var student = ctx.Students
                    .SqlQuery("Select * from Students where StudentId=@id", new SqlParameter("@id", 1))
                    .FirstOrDefault();
```
