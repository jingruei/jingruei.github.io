---
layout: post
title:  "Entity Framework概觀"
date:   2019-10-08 01:06:07 +0800--
categories: [EF]
tags: [EF]  
---

# 第一話－Entity Framework概觀

- EF為.NET平台支持的**對象關聯映射**技術，以**實體數據模型**搭配LINQ支持對象化的數據操作。
- 相較SQL，結構化數據容易於應用程序整合，避免常見類型錯誤

---

## 對象關聯映射技術－ORM
- ORM(Object Relational Mapping)：將數據庫的數據表等內容映射到自動創建的數據模型類
- 根據數據對象的設置，利用物件導向技術來處理關係數據庫
- 針對連接數據庫產生需要的SQL語句來完成對應的數據處理操作，避免直接操作SQL語句
- 提升數據存取的速度

---

# Code First
一、創建Class
```c#
  public class Product
    {
        public int Id { get; set; }
        public string Name { get; set; }
        public string Category { get; set; }
        public int Price { get; set; }
    }
```

二、DbContext設置如下
```c#
    public class AppContext : DbContext
    {
        public AppContext(DbContextOptions<AppContext> options)
            : base(options)
        {
            
        }
        public virtual DbSet<Product> Product { get; set; }
    }
```
定義`AppContext`繼承自`DbContext`並繼承基類的構造函數傳入與數據庫的連線字符串。



三、輸出
創建連線數據庫相關的實例
調用`Select()`方法取出`Product`數據表內容並轉換成可列舉`IEnumerable<Product>`類型對象，透過循環將數據逐一取出。
```c#
AppContext db = new AppContext();
IEnumerable<Product> result = db.Product.Select(x=>x);
foreach (var p in result)
{
    Console.WriteLine = p.Id
}
```
---







