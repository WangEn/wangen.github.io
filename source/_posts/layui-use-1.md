---
title: layui use 定义js外部引用函数
date: 2018-02-26 21:39:55
tags: 
    - layui
categories: layui
---
layui.use 加载layui.define 定义的模块，当外部 js 或 onclick调用 use 内部函数时，需要在 use 中定义 window 函数供外部引用 ，如下：


``` javascript
layui.use(['layer','form'],function(){
   var layer = layui.layer,
       form = layer.form();
   
   var Test = function(){
      //不能被外部引用
      console.log("call Test");
   }


   window.Hello = function(){
       //可以被外部引用
       console.log("call hello");
   }

   Test();  //执行成功
   Hello();  //执行成功
});



$(function(){
    Hello();  //可以调用
    Test();   //提供未找到 Test
})
```


注：需要引用 layui.all.js或者layui.js

## 写在最后

>转载来源：[layui use 定义js外部引用函数](http://blog.csdn.net/xcmonline/article/details/75647144)