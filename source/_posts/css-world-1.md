---
title: 《CSS世界》笔记：一些CSS属性
date: 2018-06-07 13:59:57
tags: 
	- CSS
	- CSS世界
categories: CSS
---


## CSS属性
### visibility 

[所有主流浏览器都支持 visibility 属性](http://www.w3school.com.cn/cssref/pr_class_visibility.asp)。
**注释**：任何的版本的 Internet Explorer （包括 IE8）都不支持 "inherit" 和 "collapse" 属性值。

#### 定义和用法

visibility 属性规定元素是否可见，所有浏览器兼容的值：visible（默认值，元素是可见的）、hidden(元素是不可见的)
**提示**：即使不可见的元素也会占据页面上的空间。请使用 "display" 属性来创建不占据页面空间的不可见元素。

#### 有效用法
为了提高加载性能，常采用异步加载图片。（_《CSS世界》P49-50_）为了体验良好，往往会使用一张透明的图片占位，而这张占位图片也是多余的资源，直接使用以下方式：
``` html
<img>
```
配合CSS代码来实现同样的效果
``` css
img { visibility:hidden; }
img[src] { visibility: visible;}
/*
 *兼容Forefox 内联元素-替换元素
 */
img{
    width:200px;
    height:150px;
    display:inline-block;
}
```
注意，这里的`<img>`是直接没有src属性，不是`src=""`,当图片的src属性缺省时，图片不会有任何请求。


