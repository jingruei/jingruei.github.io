---
layout: post
title:  "第六話－數據編輯與維護"
date:   2019-10-14 17:11:07 +0800--
categories: [EF]
tags: [EF]  
---

# 第六話－數據編輯與維護

執行`SaveChange()`方法後，才會將對集和內容的更新操作反映到底層的數據庫，完成SQL更新操作。


|狀態|說明|
|---|---|
|Added|添加實體對象到實體集中，數據未添加進數據庫|
|Modified|實體對象已經存在實體數據集中，數據庫同時存在對應的數據，但某些實體對象屬性值以變更，未更新到數據庫中|
|Deleted|實體對象已經存在於實體數據集中，數據庫同時存在對應的數據，但是實體對象本身被標示為刪除|
|Unchanged|實體對象存在於數據集中，數據庫同時包含對應未曾更改的相同數據|
|Detached|實體對象已經不再數據集中|

> 當`SaveChange()`被調用時

> Added－狀態會被新實體對象的屬性數據更新到數據庫

> Modified－實體對象的屬性值會逐一更新到數據庫中對應的數據字段

> Deleted－將其中對應的實體對象數據刪除

![](https://i.ibb.co/L1VH03F/Status.png)


---

## 存儲過程

#### Model

```c#
public class Product
{
    public int Id {get;set;}
    public string Name {get;set;}
    public int Price {get;set;}
    public int SalePrice {get;set;}
}
```

#### Context
```c#
modelbuilder.Entity<Product>().MapToStoredProcedures();
```
會自動生成 
- Product_Delete 
- Product_Insert
- Product_Update

## Product_Insert
```SQL
CREATE PROCEDURE [dbo].[Product_Insert]
@Name [nvarchar](max),
@Price [int],
@SalePrice [int]
AS BEGIN
 INSERT [dbo].[Product]([Name],[Price],[SalePrice])
 VALUES(@Name,@Price,@Price*0.8)
 ...
END 
```
## Product_Update
```SQL
CREATE PROCEDURE [dbo].[Product_Insert]
@Name [nvarchar](max),
@Price [int],
@SalePrice [int]
AS BEGIN
 UPDATE [dbo].[Product]
 SET [Name]=@Name,[Price]=@Price,[SalePrice]=@Price*0.8
 WHERE ([Id]=@Id)
 ...
END 
```

---
# 數據變更與衝突
避免數據衝突做法是加入`byte[]`類型的`TimeStamp`屬性，對應到數據表中的`rowversion`類型字段。

### Model
```c#
public int Id {get; set;}
public string Name {get;set;}
public int Price {get;set;}
public int Quantity {get;set;}

[Timestamp]
public byte[] Timestamp {get;set;}
```
`Timestamp`屬性是`byte`類型，並且以`[Timestamp]`標示。

### 調用
```c#
long ts= BitConverter.ToInt64(product.Timestamp,0);
```
> 查找值
> 
> Quantity:25 TimeStamp:814898205070201856
> 新的值：35
> SaveChanges() 之後
> Quantity:35 TimpStamp:12325956622736490496


#### `Reload()`方法是以數據庫優先策略，數據對象當前值與數據庫同步。
```
entry.Reload();
model.SaveChanges();
```
#### 客戶優策略先以當前值覆蓋數據中的值.
```
entry.OriginalValues.SetValues(entry.GetDatabaseValues());
model.SaveChanges();
```

#### ConcurrencyCheck
設置`[ConcurrencyCheck]`，如此一來當任何一項產品被更新的時候，SALEPRICE字段將會受到監控。

> 實體數據載入編輯期間，卻已經被其他應用程序執行了更新操作，會從額引發SalePrice字段的數據變更衝突
### Model
```c#
public int Id {get; set;}
public string Name {get;set;}
public int Price {get;set;}

[ConcurrencyCheck]
public int SalePrice {get;set;}
```
---

### 事務處理
```c#
using (KTSToreContext context = new KTStoreContext())
{
using (var transaction = context.Database.BeginTransation())
{
    try
    {
        var result = context.Products;
        foreach (var p in result)
        {
            product.SPrice=(int) (result.Price*0.5);
        }
        context.SaveChanges();
        transaction.Commit();
        //顯示更新成功

    }
    catch (Exception ex)
    {
        transaction.Rollback();
        //顯示更新失敗 
    }
}
```
