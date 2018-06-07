---
title: Layui使用layer的一些问题
date: 2018-06-07 14:02:04
tags: 
	- layui
	- layer
categories: layui
---

## 问题一：父页面传值时报错

### 问题描述

```
//通过layer给父页面传值
parent.$('#parentIframe').text('我被改变了');
报Uncaught TypeError: parent.$ is not a function(…) 错误
```

### 解决方法

#### 方法一：在父页面引用jquery

``` html
<script type="text/javascript" src="https://cdn.bootcss.com/jquery/3.2.1/jquery.min.js"></script>
```


#### 方法二： $改写为layui.jquery

之前可能报错的写法

```javascript
parent.$('#testText').text('我被改变了');
parent.$("#memberList tr").each(function(){});
```

修改后的写法
```javascript
parent.layui.jquery('#testText').text('我被改变了');
parent.layui.jquery("#memberList tr").each(function(){});
```


