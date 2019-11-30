---
layout: post
title:  "UI 測試 "
date:   2019-11-19 12:04:08 +0800--
categories: [WPF]
tags: [WPF]  
---

## 自動化點擊UI

#### `WinAppDriver.exe`以及安裝
- appium 是一個框架
     - `Windows Application Driver`
        - 用於打開應用、操作應用、關閉應用、返回結果

---
#### 一、Session class created.
```c#
private const string WindowsAppDriverUrl="http://127.0.0.1:4723/"
private const string MyAppId=@"D:\Corder\xxx\bin\Debug\LC.exe"

protected static WindowsDriver<WindowsElement> session;

public static void SetUp()
{
    if (session ==null)
    {
    DesiredCapabilities appCap = new DesiredCapabilities();
    appCap.SetCapability("app",MyAppId);
    appCap.SetCapability("deviceName","WindowsPC")
    session = new WindowsDriver<WindowsElement>(new Uri(WindowsAppDriverUrl),appCap);

    session.Manage().Timeouts().ImplicitlyWait.(TimeSpan.FromSeconds(1,5));

    }
}   

public static void TrarDown()
{

}

```




```c#
public void Add()
{

}
public void Sub()
{
    
}
public void Div()
{
    
}
```