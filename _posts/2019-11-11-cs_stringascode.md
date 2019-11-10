---
layout: post
title:  "C# String ad Code"
date:   2019-11-11 12:04:08 +0800--
categories: [C#]
tags: [C#]  
---

Using the Roslyn scripting API :
https://github.com/dotnet/roslyn/wiki/Scripting-API-Samples
```c#
// add NuGet package 'Microsoft.CodeAnalysis.Scripting'
using Microsoft.CodeAnalysis.CSharp.Scripting;

await CSharpScript.EvaluateAsync("System.Math.Pow(2, 4)") // returns 16
```

You can also run any piece of code:
```c#
var script = await CSharpScript.RunAsync(@"
                class MyClass
                { 
                    public void Print() => System.Console.WriteLine(1);
                }")
And reference the code that was generated in previous runs:

await script.ContinueWithAsync("new MyClass().Print();");
```