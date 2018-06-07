---
title: dedecms织梦特定条件下,隐藏特定栏目的方法,通过css实现
date: 2018-01-20 21:54:52
tags: 
    - DedeCMS
    - 栏目
categories: DedeCMS
---
## 一、通过前台直接隐藏栏目

> 如果栏目在后台设置显示或者设置隐藏无效,前台怎么隐藏?

### CSS样式

定义用来隐藏栏目的CSS样式
``` bash
.hidden{display:nidden}
```
### HTML调用代码

因为栏目id为6的栏目是【关于我们】等非核心栏目,不想在导航上显示,但在后台又没有设置隐藏,设置隐藏了前台的其它栏目页就没法调用他了。所以,在导航条上要处理一下。

``` bash
{dede:channel type='top' row='10' currentstyle="<li class='active'><a href='~typelink~' ~rel~>~typename~</a></li>"}
    <li  class="[field:id runphp='yes'] if(@me=='6') @me = 'hidden'; else @me = '';[/field:id]" >
        <a href='[field:typeurl/]'>[field:typename/]</a>
    </li>
{/dede:channel}
```

## 二、前台单独显示隐藏栏目

> 如果ID为666的栏目在后台设置隐藏,而在前台想让他单独显示在导航条的右侧,怎么实现？代码如下所示：

``` bash
# //调用首页
<li {dede:field name=id runphp='yes'}(@me=='')?@me="class='active'":@me='';{/dede:field} >
    <a href="{dede:global.cfg_basehost/}">首页</a>
</li>
# //调用所有显示的栏目
{dede:channel type='top' row='10' currentstyle="<li class='active'><a href='~typelink~' ~rel~><span>~typename~</span></a></li>"}
<li>
    <a href='[field:typeurl/]' [field:rel/]><span>[field:typename/]</span></a>
</li>
{/dede:channel}
# //在右侧调用隐藏的栏目（ID:666）
 <li class="float-right {dede:field name=id runphp='yes'}(@me=='666')?@me='active':@me='';{/dede:field}">
 {dede:type typeid=666}
    <a href="[field:typeurl /]">[field:typename /]</a>
 {/dede:type}
 </li> 
```

### 写在最后

> 原文地址:http://www.dede58.com/a/dedejq/4271.html
