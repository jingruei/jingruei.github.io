---
layout: post
title:  "C#"
date:   2019-10-08 01:06:07 +0800--
categories: [C#]
tags: [C#]  
---

# C Sharp

# Event

* 事件:當滑鼠、鍵盤、表單仔入時等操作觸發的動作。

* Publisher : 引發事件的控件

* Subscriber：負責處理此事的物件


```c#
private void btnClick (object sender , EvenArgs e)
{
 
}
```



* `sender`：觸發此事件的物件 => `Button`
* `EvenArgs`：例如MouserUp是MouseEventArgs物件


- 事件擁有者 (myBtn)
- 事件響應者 (窗體本身 , `Event Handler`)
- 事件處理器(this.myBtn.Click)
- 訂閱關係(`this.myBtn.Click += new System.EventHandler(this.myBtn`)


```c#
public delegate void TimeToRaise(); //定義委託
private event TimeToRaise RingAlarm; //定義事件

Student s = new Student()
RingAlarm a= new TimeToRaise(s.WakeUp);
if (RingAlarm != null)
{
    RingAlarm();
}
```
