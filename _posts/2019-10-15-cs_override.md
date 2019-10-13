---
layout: post
title:  "C#－方法的過載"
date:   2019-10-15 12:00:03 +0800--
categories: [C#]
tags: [C#]  
---

# C Sharp 

# Overload
* 過載：允許擁有兩個以上的同名方法，只要傳遞參數個數或資料型別不同

```c#
public class MyMath
{
    public Plus (int a, int b)
    {
        return a+b;
    }

    public Plus (int a, int b,int c)
    {
        return a+b+c;
    }
}
```

# Ploymorphism 
* 多型  
   * Static Binding：編譯階段就央訊息送往目標物件
   * Dynamic Dinding：執行階段才知道訊息送往的目標物件