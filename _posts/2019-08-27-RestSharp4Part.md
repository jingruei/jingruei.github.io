---
layout: post
title:  "RestSharp 四部曲"
date:   2019-10-11 01:06:07 +0800--
categories: [RestSharp]
tags: [RestSharp]  
---

# 介紹 API Testing with RestSharp

# 入門安裝
1. RestSharp install . 
2. SpecFlow.
3. SpecFlow.Assist.Dynamic

---

# Part 1 Basic GET 操作

```c#
var client = new RestClient("https://localhost:5000/")

var request = new Request("posts/{postid}",Method.GET);
request.AddUrlSegment("postid",1);

var content = client.Execute(request).Content;

```
- 為搜尋id為1的文章
- client有有Execute... ExcuteAsGet...  很多方法，可以使用


```c#
var client = new RestClient("https://localhost:5000/")

```
# Part 2. 反序列化＆序列化 Deserialization
將Content轉化為有用的資訊。



## 一、不使用其餘第三方的方法進行反序列化

```c#
var client = new RestClient("https://localhost:5000/")

var request = new Request("posts/{postid}",Method.GET);
request.AddUrlSegment("postid",1);

var content = client.Execute(request);

var deserialize = new JsonDeserializer();

var output= desrial.Deserialize<Dictionary<string,string>>(response);

var result = output["author"]; 
```


## 二、使用JSON.NET進行反序列化

```c#
var client = new RestClient("https://localhost:5000/")

var request = new Request("posts/{postid}",Method.GET);
request.AddUrlSegment("postid",1);
var content = client.Execute(request);

JObject obs = JObject.Parse(response.Content);
var result = obs[author].ToString();
```

## Others. 
安裝NUnit。移除VisualStudioTest...方便進行測試
![](https://i.ibb.co/RvHTHb4/Untitled.png)

```c#
[TestFixture]
public class UniTest1
{
    [Test]
    public void TestMethod1()
    {
        .
        .
        .
        Assert.That(result,Is.EqualTo("iewihc"),"Error:作者不正確");
    }
}
```

--- 

# Part 3. POST with body params using anonymous and type class

## 一、傳送新(自訂義)對象
```c#
var client = new RestClient("https://localhost:5000/")

request.Request.Format =DataFormat.Json;
request.AddBody(new {name = "Sam"});
var request = new Request("posts/{postid}/profile",Method.POST);

request.AddUrlSegment("postid",1);
var content = client.Execute(request);

//var deserialize = new JsonDeserializer();
//var output= desrial.Deserialize<Dictionary<string,string>>(response);
//var result = output["name"]; 
```

## 二、傳送類別對象
```c#
var client = new RestClient("https://localhost:5000/")
var request = new Request("posts",Method.POST);

request.Request.Format =DataFormat.Json;
request.AddBody(new Posts() { id = 13,author="iewihc", Title="zn"});

var response = clinet.Execute(request);

//var deserialize = new JsonDeserializer();
//var output= desrial.Deserialize<Dictionary<string,string>>(response);
//var result = output["author"]; 
```

# Part 4  Generic and Async Execute in RestSharp.

## Part A
`var response .client.Execute(request);`

`var response .client.Execute<Posts>(request);`


### 同步＆非同步

```c#
private async Task<IRestResponse<T>> ExecuteAsyncRequest<T>(RestClient client, IRestRequest request) where T:class,new()
{
var taskCompleteionSource = new TaskCompleteionSource<IRestResponse<T>>();
client.ExecuteAsync<T>(request,restReponse =>
{
    if(restReponse.ErrorExecption !=null)
    {
        const string message = "Error retrieing response."
        throw new ApplicationExecption(message,restResponse,ErrorExecption)
    }
    taskCompletionSource.SetResult(restResponse);
});
return await taskCompletionSource.Task;
}
```
使用
```c#

var client = new RestClient("https://localhost:5000/")
request.Request.Format =DataFormat.Json;
request.AddBody(new Posts() { id = 13,author="iewihc", Title="zn"});
var request = new Request("posts",Method.POST);

request.AddUrlSegment("postid",1);
var content = client.ExecuteAsyncRequest(client, request)<Posts>.GetAwaiter().GetResult();

//var deserialize = new JsonDeserializer();
//var output= desrial.Deserialize<Dictionary<string,string>>(response);
//var result = output["name"]; 
```