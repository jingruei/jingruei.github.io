---
layout: post
title:  "補充一－多人衝突範例"
date:   2019-10-14 17:12:07 +0800--
categories: [EF]
tags: [EF]  
---

# Model
```c#
public class Product
{
    public int Id{get; set;}
    public int Price {get; set;}
    public int Quantity {get; set;}

    [Timestamp]
    public byte[] Timestamp {get;set;}
}
```

# 調用
```c#
using(KTStoreModel model = new KTStoreModel())
{
    Product p = model.Products.Where(p => p.Id ==1).First();
    long ts = BitConverter.ToInt64(product.Timestamp,0);
    Console.WriteLine("Quantity:{0} ,TimeStamp:{1} ",p.Quantity,ts);

    // TODO: 假設新值將數量設定為50
    Console.WriteLine("新值將數量設定為50");
    p.quantity = 50 ;
    model.SaveChanges();

    ts = BitConverter.ToInt64(p.Timestamp);
    Console.WriteLine("Quantity:{0} ,TimeStamp:{1} ",p.Quantity,ts)
}
```

# 結果如下

> Quantity:25 TimeStamp:8148982050750201856

> 新值將數量設定為50

> Quantity:50 TimeStamp:1235956622736490496

---


# 測試結果

```c#
using(KTStoreModel model = new KTStoreModel())
{
    Product p = model.Products.Where(p => p.Id ==1).First();
    long ts = BitConverter.ToInt64(product.Timestamp,0);
    Console.WriteLine("Quantity:{0} ,TimeStamp:{1} ",p.Quantity,ts);

    try
    {
    // TODO: 假設新值將數量設定為50 
    // Prase 在這之前直接去修改數據庫 
    Console.WriteLine("新值將數量設定為50");
    p.quantity = 50 ;
    model.SaveChanges();
    ts = BitConverter.ToInt64(p.Timestamp);
    Console.WriteLine("Quantity:{0} ,TimeStamp:{1} ",p.Quantity,ts)
    }

    catch (System.Data.Entity.Infrastrucute.DbUpdateConcurrencyException ex )
    {   // ResolveConcurrency(model,entry)


        DbEntityEntry entry = ex.Entries.Single();
        DbPropertyValues current = entry.CurrentValues;
        int quantity = current.GetValue<int>("Quantity");
        long timestamp=BitConverter.ToInt64(current.GetValue<byte[]>("Timestamp"),0)
        
        DbPropertyValues dbvalue = entry.GetDatabaseValues(); 
        int dbquantity = dbvalue.GetValue<int>("Quantity");
        long dbtimestamp=BitConverter.ToInt64(dbvalue.GetValue<byte[]>("Timestamp"),0)

        Console.WriteLine("DbUpdateConcurrencyException異常...");
        Console.WriteLine("當前實體的屬性值:Quantity:{0},TimeStamp:{1}",quantity,timestamp);
        Console.WriteLine("數據庫的值:Quantity:{0},TimeStamp:{1}",dbquantity,dbtimestamp)

    }
}
```
> Quantity:122  TimeStamp:1021754165459681280

(進去資料庫修改,Update product set Quantity=200)

> 新值將數量設定為50

> DbUpdateConcurrencyException異常...

> 當前實體的屬性值:Quantity:50 TimeStamp:1021754165459681280

> 數據庫中的值: Quantity:200 TimeStamp:-7934779593473392640

# 封裝一個方法(解決方案)

```c#
public static void ResolveConcurrency (KTStoreModel model , DbEntityEntry entry)
{
    Console.WriteLine("1.數據庫優先, 2.客戶端優先");
    int i = int.Prase( Console.ReadLine());
    if( i ==1)
    {
        entry.Reload();
        model.SaveChanges();
        Console.WriteLine("與數據庫同步完成");
    }
    else
    {
        entry.OriginalValues.SetValues(entry.GetDatabaseValues());
        model.SaveChanges();
        Console.WriteLine("已更新到數據庫");
    }
}
```
