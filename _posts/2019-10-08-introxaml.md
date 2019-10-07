---
layout: post
title:  "XAML介紹"
date:   2019-10-08 12:56:07 +0800--
categories: [WPF]
tags: [XAML, WPF]  
---

# XAML

zammel
XAML(Extensible Application Markup Language) 

- 是一種標記語言 (markup language)
    - 可實例化.net 對象
- 可構造WPF應用程序的基本版面、按鈕、控件

#### XAML Basics

- 每個元素都映射到.NET 類的一個實例
`<Button>`等同在.NET 創建一實例
- 嵌套(Nest)
嵌套是一種表示＂包含(containment—in)＂的方法
- 可通過attribute設置每個類的property

---
####頂級元素(toplevel element)
一個XAML文檔只能有一個頂級元素

- Window
- Page(可導航)
- Application (資源 和 startup settings)

#### XAML Namespaces

```xaml#
xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
```
- `xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"`
包含了所有WPF類，構建使用者介面的控件
- `xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"` 
元素必須使用前綴x來指定，例如`<x:ElementName>`

1. `partial class`  編譯時會合併XAML.cs.、XAML
2. `InitialzeComponent()` 調用System.Windows.Application類中的LoadComponent()方法，大概是提取BAML並使用它來構建XAML的對象、創建對象的屬性、關聯的事件處理

