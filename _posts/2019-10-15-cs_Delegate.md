---
layout: post
title:  "C#－委派"
date:   2019-10-15 12:00:07 +0800--
categories: [C#]
tags: [C#]  
---

# C Sharp
# Delegate

* 委派: 事件的基礎，一個參考類別的方法或實例方法的物件。
可以利用委派物件在執行時決定呼叫的方法。

## 步驟

- 一、宣告委派型別
 `delegate int MyDelegate (int opd1, int opd2)`
- 二、建立委派可以呼叫的方法
```c#
class MyMath
{
    public static int Add(int a, int b)
    {
        return a+b;
    }
    public static int Sub(int a, int b)
    {
        return a-b;
    }

}
```
- 三、建立委派物件,呼叫參考方法
```c#
if (chkBox1.value.Checked)
{
MyDelegate handler = new MyDelegate(MyMath.add)
Console.WriteLine(handler(5,15));
}
else
{
MyDelegate handler = new MyDelegate(MyMath.sub)
Console.WriteLine(handler(5,15));
}
```

- 補充：多點傳送委派(Multicast)

```c#
public delegate void MyDele ( int a, int b);

class MyMath
{
    public string str;

    public static int Add(int a, int b)
    {
        return str += "加法："+a+b;
    }
    public static int Sub(int a, int b)
    {
        return a-b += "/減法："+a-b;
    }
    
}

//Button Click

MyMath math = new MyMath()
MyDele handler = new MyDele(math.Add);
handler += new MyDele(math.Sub);
Console.WriteLine(handler(5,15));
//會出現
// 加法:20/減法:-10

```

