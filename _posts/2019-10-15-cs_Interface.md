---
layout: post
title:  "C#－介面"
date:   2019-10-15 12:00:04 +0800--
categories: [C#]
tags: [C#]  
---

# C Sharp

# Interface
定義不同類別之間的一致行為。

> 有三個類：Book , CD , Toy
> 需要同一個GetPrice()，取得價格的方法
> 就可以實作介面IPrice


- 介面中沒有建構子、靜態成員...必須是抽象方法。

# 宣告介面
```c#
interface IArea
{
    void Area ();
}
interface IInfo
{
    string Info();
}
```

# 實作介面
```c#
class Rectangle : IArea , IInfo
{
public int H;
public int W;
public Rectangle (int h , int w)
{

}
public double Area()
{
    retrun ( H*W);
}
public string Info ()
{
    return "長方形的寬" + H + "長方形的高"+ W;
}

}
```

#調用介面
```c#
Rectangle r = new Rectangle(10,20);
Console.WriteLine(r.Info();
Console.WriteLine(r.Area();
```


---

# Abstract Class
抽象類別：不能建立物件的類別，只能允許類別繼承他。
```c#
public abstract class Shape
{
    public int X;
    public int Y;
    public Shape (int x, int y);
}
```

# sealed

密封類別，類別無法被繼承。
- 保護原因:防止子類改寫。
- 設計原因:設計需求。
