---
layout: post
title:  "補充二－存儲過程與數據衝突"
date:   2019-10-14 17:15:07 +0800--
categories: [EF]
tags: [EF]  
---

```c#
protected override void OnModelCreating(DbModelBuilder modelBuilder)
{
    modelBuilder.Entity<Product>().MapToStoredProcedures();
}
```

### Product_Insert
```sql
CREATE PROCEDURE [dbo].[Product_Insert]
@Name [navarchar](max),
@Price [int],
@Quantity [int]
AS BEGIN
INSERT [dbo].[Product]([Name],[Price],[Quantity]) VALUE(@Name,@Price,@Quantity)
DECLARE @Id int SELECT @Id=[Id]
FROM [dbo].[Product]
WHERE @@ROWCOUNT > 0 AND [Id]= scope_identity()
SELECT t0.[Id], t0.[Timestamp] FROM [dbo].[Product] AS t0
WHERE @@ROWCOUNT > 0 AND t0.[Id] = @Id 
```

### Product_Update
```sql
CREATE PROCEDURE [dbo].[Product_Update]
@Name [navarchar](max),
@Price [int],
@Quantity [int]
@Timestamp_Original [timestamp]
AS BEGIN
UPDATE [dbo].[Product]

SET [Name] = @Name ,[Price] =@Price,[Quantity]=@Quantity 
WHERE (([Id]=@Id) AND 
([Timestamp]=@Timestamp_Original OR [Timestamp] IS NULL AND @Timestamp_Original IS NULL)))

SELECT t0.[Timestamp]
FROM [dbo].[Product] AS t0
WHERE @@ROWCOUNT > 0 AND t0.[Id] = @Id
END

```