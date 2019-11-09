---
layout: post
title:  "C#－強型別中玩動態型別"
date:   2019-10-15 12:00:08 +0800--
categories: [C#]
tags: [C#]  
---

## Anonymous Types
首先我先用 Anonymous Types (匿名類型) 作為開場，C# 允許將一組唯讀屬性封裝成一個物件，而不需要事先明確定義類型。

```c#
var v = new 
{ 
    Amount = 108, 
    Message = "Hello" 
};  
```

## Why Anonymous Types
在傳統思維的 C# 中，如果你要將一組屬性封裝成物件，你必須這樣做：

```c#
class MyProps
{
    public int Amount;
    public string Message;
}
```
定義完畢後，才可以使用它：

```c#
MyProps props = new MyProps() 
{
    Amount = 108,
    Message = "Hello"
};
```

而 Anonymous Types 就是這一切的語法糖，讓你用更精簡的語法完成一樣的事情，更多細節可以參考 C# 匿名類型。

混合招式
C# 如此飄逸自由的語法糖，到底要怎麼使用呢? 最經典的場合就是 Option or Setting，假設我們有一個 function 會進行訂票：

```c#
// 訂票
// setting: 用來設定的物件
static void booking(object setting) {
    // 期望有 setting.people = 人數
    // 期望有 setting.date = 時間
}
```
我們期望這樣呼叫：
```c#
var setting = new
{
    people = 1,
    date = DateTime.Now
};
booking(setting);
```

## Type.GetProperty 方法
關鍵就在於 C# 不像是 javascript 或其他動態語言，所有的型別、屬性成員與方法都必須要事先定義，而在 Anonymous Types 中卻沒有這樣做，因此無法像是這樣呼叫：

```c#
static void booking(object setting) {
    int people = setting.people;
    DateTime date = setting.date;
}
```
既然預期 `setting` 會有 `people` 與 `date` 兩個成員，但是卻因為 Anonymous Types 只會在編譯階段將型別與成員產生，所以若要取出這個成員，就只能使用 `Type.GetProperty()`：

```c#
static void booking(object setting) 
{
    int people = 1;
    try
    {
        var prop = setting.GetType().GetProperty("people");
        people = prop.CanRead ? (int)prop.GetValue(setting, null) : 1;
    }
    catch(Exception) {
    }
}
```
## 封裝 Type.GetProperty
為了讓語法更加精簡，我們可以封裝這個取值的方法，並且協議如果取不到值，一律回傳 `null`:

```c#
static T getProp<T>(string key, object obj) {
    try
    {
        var prop = obj.GetType().GetProperty(key);
        if (prop.CanRead)
            return (T)prop.GetValue(obj, null);
    }
    catch (Exception){ }
    return default(T);
}
```
這裡的 `getProp` 被設計成泛型的目的，是為了讓**取值與型態轉換**一氣呵成。如果無法取到指定 `key` 的 Property 或是無法轉型則會返回使用者定義型態的預設值 `default(T)`。舉例來說 string 預設是 null、int 預設是 0 以此類推...。

## 精簡版 booking() 方法
有了上面的 `getProp<T>()` 方法後，取得 Property 就更方便了：

```c#
static void booking(object setting) {
    int people = getProp<int?>("people", setting) ?? 1; 
    DateTime date = getProp<DateTime?>("date", setting) ?? DateTime.Now;
}
```
>   備註：上面的語法用了大量的語法糖
    首先是 int? 是 Nullable<int> 的縮寫，表是一個 Nullable 的 int，在 C# 中有許多類型不允許為 NULL，如果需要指定 int 為 NULL，可以使用 Nullable-types
    另外 ?? 是 C# 中的 null conditional operator，它的作用是，如果 ?? 的左邊運算元為 NULL 時，它就會使用右邊的運算元來指派，否則就用左邊。非常適合用來指派預設數值。

## 將 Anonymous Types 的所有成員指定到 泛型 裡

為什麼要將 Anonymous Types 指定到泛型裡? 先說個來由來作為開頭。
在 javascript 裡，資料大多由 JSON 格式儲存，而 JSON 格式又相容於 javascript 語言，當今許多 API 也大多都以 JSON 做為資料交換的格式。但如果要用 C# 來處裡 JSON 資料就非常的麻煩，我們可能會立刻想到 Json.NET 套件。但其實它就是在幫你把動態資料 (Anonymous Types) 塞到你指定的類別 (泛型) 裡。掌握這個技巧，就能夠在撰寫 C# 時，讓程式碼更加優雅。

## 取得所有成員 Type.GetProperties()
使用 `GetProperties() `就可以將所有成員取出

```c#
foreach (var prop in setting.GetType().GetProperties())
{
    if (prop.CanRead)
        Console.WriteLine(prop.GetValue(setting, null));
}
```
## Assign Value
既然可以取出 Properties 當然也可以 assign 數值進去，我們將 Assign Value 封裝成一個方法：

```c#
static void assignValue<T>(ref T obj, string key, object value)
{
    try
    {
        var prop = obj.GetType().GetProperty(key);
        if (prop.CanWrite)
            prop.SetValue(obj, value);         
    }
    catch (Exception){}     
}
```
> 備註：這裡的 `obj` 使用 `ref` 修飾子， 表示要被 assign 的物件會以**傳址** 的方式傳入，而非一般的傳值。如此達到 assign 的效果。

## Object Assign
假設有一個訂票的設定物件為 `ModelBookingInfo` 定義如下：

```c#
class ModelBookingInfo
{
    public int people { get; set; }
    public DateTime date { get; set; }
}
```
將欲 assign 的動態資料，透過 `Type.GetProperties()` 取得所有成員，並逐一呼叫 `assignValue<T>()` 方法，將數值逐一寫入：

```c#
ModelBookingInfo info = new ModelBookingInfo();

foreach (var prop in setting.GetType().GetProperties())
{
    if (prop.CanRead)
        assignValue(ref info, prop.Name, prop.GetValue(setting, null));
}

Console.WriteLine(info.people);
Console.WriteLine(info.date.ToString("MM/dd"));
```
輸出結果：
```c#
100
9/24
```

## Conclusion
以上就是 C# Anonymous Types 與 Class 的 Properties 的應用介紹，目的就是希望在面對動態資料時，還是可以透過 C# 既有的強行別機制，讓語法還是能優雅的撰寫。