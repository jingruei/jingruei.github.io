---
layout: post
title:  "C#－例外處理"
date:   2019-10-15 12:00:02 +0800--
categories: [C#]
tags: [C#]  
---

# C Sharp

# Exception 
* 例外處理：指程序執行時發生不正常的執行創太，產生的例外物件

```c#
int x = 10;
int y = 0
try 
{
    x=x/y;
}
catch (DivideByZeoExption ex)
{
    Console.WriteLine(ex.ToString()); //程式錯誤,System.DivideByZeroException嘗試以除與.........

}

finally {
    Console.WriteLine(x);
    Console.WriteLine(y);

    // 最後印出x,y.

}
```

|區塊|說明|
|---|---|
|try|放置可能出錯的區段|
|catch|捕捉異常,`ex.ToString()`可以獲得錯誤訊息|
|finally|無論例外是否產生都會執行此句。|

|例外|說明|
|---|---|
|ArithmeticException|數學運算錯誤|
|ArgumentException|參數相關|
|ArrayTypeMismatch|陣列不符合型別相關|
|indexOutOfRangeException|陣列索引超過邊界|
|NullReferenceException|物件為ＮＵＬＬ|
|OutOfMemoryException|記憶體不足|

# 拋出異常

`throw new ArithmeticException("值不得為0");`

```c#
Button Click....
{
    int times = Convert.ToInt32(txtTimes.Text);
    try
    {
        if (times>10)
        throw new MyException("不得超過十次");

    }
    catch (MyException ex)
    {
        Messagebox.Show(ex.ToString()); //程式錯誤:MyException:......於.....行26...
    }
}
```