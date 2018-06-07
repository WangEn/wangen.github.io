---
title: Dedecms判断文章是否有缩略图没有就不显示默认图片
date: 2017-12-16 00:09:24
tags: 
	- DedeCMS
	- 缩略图
categories: DedeCMS
---
## 调用参考代码：
以下代码可直接复制使用
``` bash
[field:array runphp='yes']  
@me=(strpos(@me['litpic'],'defaultpic')?'':"<a href='{@me['arcurl']}' alt='{@me['title']}'><img src='{@me['litpic']}' alt='{@me['title']}'/></a>");  
[/field:array]
```
 
以上代码意思是，如果没有缩略图就显示为空白，有则显示
## 编译之后的代码如下：
``` bash
<a href='{@me['arcurl']}' alt='{@me['title']}'> <img src='{@me['litpic']}' alt='{@me['title']}'/></a>
```
## 后记
如果你想自定义没有缩略图的样式，则修改'defaultpic')?''问号后面的单引号里面的内容。
