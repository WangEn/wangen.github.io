---
title: dedecms织梦列表页内容页栏目高亮和当前栏目调用二级三级栏目
date: 2017-12-16 12:00:39
tags: 
    - DedeCMS
    - 栏目
categories: DedeCMS
---
> 在处理企业站的时候，经常会用到在列表页或内容页调用二三级栏目，且需要跟随栏目切换高亮显示。

![channlecurrent](http://www.dedediy.com/uploads/allimg/channlecurrent.gif)
## 具体实现方法如下
### 1、打开 \include\taglib\channelartlist.lib.php 找到

``` bash 
$tpsql = " reid='$typeid' AND ispart<>2 AND ishidden<>1 ";
```

改成

``` bash
if($type=='son')
{
	$typeid = ( !empty($refObj->TypeLink->TypeInfos['id']) ?  GetTopid($refObj->TypeLink->TypeInfos['id']) : 0 );
	$tpsql = " reid='$typeid' AND ishidden<>1 ";
}
else
{
	$tpsql = " reid='$typeid' AND ispart<>2 AND ishidden<>1 ";
}
```

### 2、打开 \include\taglib\channel.lib.php 找到

``` bash
if($type=='son' && $reid!=0 && $totalRow==0)
```
改成

``` bash
if($type=='son' && $reid!=0 && $totalRow==0 && $noself=='')
```

### 3、去掉模板引擎禁用PHP标签
后台-系统-其它选项 模板引擎禁用标签 去掉php 
![去掉PHP标签禁用](http://www.dedediy.com/uploads/allimg/QQimg20170807131443.png)

## 前台模板文件中调用代码如下：

``` python
<ul>
{dede:php}
$GLOBALS['thisid'] = intval($refObj->Fields['typeid']);
$GLOBALS['reid'] = intval($refObj->Fields['reid']);
$GLOBALS['topid'] = intval($refObj->Fields['topid']);
{/dede:php}
{dede:channelartlist type=son}
    <li{dede:field.typeid runphp=yes}(@me==$GLOBALS['thisid']||@me==$GLOBALS['reid']||@me==$GLOBALS['topid'])? @me=' class="current"':@me='';{/dede:field.typeid}><a href='{dede:field.typeurl/}' >{dede:field.typename/}</a></li>
    <ul>
    {dede:channel type=son noself=yes}
    <li[field:id runphp=yes](@me==$GLOBALS['thisid'])? @me=' class="current2"':@me='';[/field:id]><a href='[field:typelink /]' title='[field:typename/]'>[field:typename/]</a></li>
    {/dede:channel}
    </ul>
{/dede:channelartlist}
</ul>
```
至此，全部完成。