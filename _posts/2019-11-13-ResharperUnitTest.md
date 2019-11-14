---
layout: post
title:  "Unit Test Use Resharper "
date:   2019-11-13 12:04:08 +0800--
categories: [C#]
tags: [C#]  
---
- `SHIFT+HOME` 選取當前行(尾)
- `SHIFT+END` 選取當前行(頭)
- `CTRL+W` 選擇當前行
- `CTRL+SHIFT+L` 刪除當前行
- `CTRL+M+M` 開關摺疊
- `CTRL+D` 複製貼上
- `CTRL+R+A` run all test

---

## ALT+ENTER技巧

`new calcuator = new Calcuator();`  <--- Alt+Enter 產生Class


```c#
            var calcuator = new Calcuator();
            var first = 1;
            var second = 2;
            calcuator.Plus(first, second);  //<--- Alt+Enter 產生方法
```

`calcuator.Plus(first, second);` <--- Alt+Enter 產生變數

> 運行後---->`var plus = calcuator.Plus(first, second);`

---

## 重構技巧

- `CTRL+R,CTRL+F`   選擇  `Field initializer` (提取欄位)
     - 效果`        private Calcuator _calcuator = new Calcuator();`

- `CTRL+R,CTRL+M` 提取方法
     - 效果
``` c#
        public void Add_positive_integer ()
        {
            PlusShouldBe();
        }

        private void PlusShouldBe()
        {
            var first = 1;
            var second = 2;
            var plus = _calcuator.Plus(first, second);
            Assert.AreEqual(3, plus);
        }
```

- `CTRL+R+CTRL+P `
提取相關參數