---
layout: post
title:  "C#－REF,OUT"
date:   2019-10-15 12:00:03 +0800--
categories: [C#]
tags: [C#]  
---

# C Sharp REF,OUT

- 實質型別 (Value Type) : 即變數的記憶體(Stack)空間裡存放的為『value』。
  - int,float,char
    - 
- 參考型別 (Reference Type) :變數的記憶體空間(Stack)裡存放的為『Heap記憶體位址』。
  - Class,String,Interface,delegate,object

1. Default: variables are passed *BY VAL* to methods. 
   OUT,REF helps to pass variales *BY REF*

預設值>'OutSideVar'複製一份到'InsideVar' 

1. DEFAULT : 輸出為`void{}`外的結果，使用一開始被初始的值

2. REF : 共用同一份變數

3. OUT : 輸出為`void{}`裡面的結果

![](https://github.com/iewihc/CodeDocument/blob/master/Images/c1.jpg?raw=true)

![](/images/c2.jpg)

![](/images/c1.jpg)
