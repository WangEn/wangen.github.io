---
title: DedeCMS栏目添加自定义字段，增加栏目上传缩略图功能
date: 2017-12-16 11:28:35
tags: 
  - DedeCMS
  - 栏目
categories: DedeCMS
---
> 我们用织梦制作时，点击进入每个栏目时，都会有“关于我们”，“新闻中心”，“产品展示”等提示性的图片，单独调用这些图片比较麻烦，我们可以修改程序，实现每个栏目都有上传栏目缩略图的功能，就方便多了。

## 修改方法如下：
### 第一步：执行SQL命令为数据库的栏目表结构添加一个字段

``` bash
alter table `dede_arctype` add `typeimg` char(100) NOT NULL default '';
```
### 第二步：修改涉及到文件：

- [x] dede/catalog_add.php 
- [x] dede/catalog_edit.php
- [x] dede/templets/catalog_add.htm
- [x] dede/templets/catalog_edit.htm
#### 1、打开dede/catalog_add.php

查找
``` bash
$queryTemplate = "INSERT INTO `#@__arctype`
```
定义添加新变量字段typeimg,将
``` bash
(reid,topid,sortrank,typename,typedir,
```
修改为
``` bash
(reid,topid,sortrank,typename,typedir,typeimg,
```
将
``` bash
('~reid~','~topid~','~rank~','~typename~','~typedir~',
```
修改为
``` bash
('~reid~','~topid~','~rank~','~typename~','~typedir~','~typeimg~',
```
#### 2、打开dede/catalog_edit.php

查找(一般是Ctrl+F快捷键)
``` bash
$upquery = "Update 
```
在其下面新加一行
``` bash
typeimg='$typeimg',
```


#### 3、打开dede/templets/catalog_add.htm
修改后台的页面模板，查找
``` bash
<tr>
   <td height="26" style="padding-left:10px;">列表命名规则：</td>
   <td>
      <input name="namerule2" type="text" id="namerule2" value="{typedir}/list_{tid}_{page}.html"  class="pubinputs"  style="width:250px" />
      <img src="images/help.gif" alt="帮助" width="16" height="16" border="0" style="cursor:pointer" onClick="ShowHide('helpvar3')"/></td>
</tr>
```
在其下面增加以下内容
``` bash
<tr>
  <td height="65" style="padding-left:10px;">栏目图片：</td>
  <td>
     <input name="typeimg" type="text" style="width:250px" id="typeimg" class="alltxt" value="" />
 <input type="button" name="set9" value="浏览... "class="coolbg np" style="width:60px" onClick="SelectImage('form1.typeimg','');" />
   </td>
</tr>
```
并在文件的<head></head>里面增加以下内容
``` bash
<script language='javascript' src="js/main.js"></script>
```
#### 4、打开dede/templets/catalog_edit.htm
在第3步文件（同上catalog_add.htm）同样的位置插入修改：
``` bash
<tr>
   <td height="26" style="padding-left:10px;">列表命名规则：</td>
   <td>
      <input name="namerule2" type="text" id="namerule2" value="{typedir}/list_{tid}_{page}.html"  class="pubinputs"  style="width:250px" />
      <img src="images/help.gif" alt="帮助" width="16" height="16" border="0" style="cursor:pointer" onClick="ShowHide('helpvar3')"/></td>
</tr>
```
在其下面加入
``` bash
<tr>
  <td height="65" style="padding-left:10px;">栏目图片：</td>
  <td>
     <input name="typeimg" type="text" style="width:250px" id="typeimg" class="alltxt" value="<?php echo $myrow['typeimg']?>" />
 <input type="button" name="set9" value="浏览... "class="coolbg np" style="width:60px" onClick="SelectImage('form1.typeimg','');" />
   </td>
</tr>
```
> 备注说明：下面这句会调用出已添加的路片路径。
> <?php echo $myrow['typeimg']?> 
和第三步一样，在文件的<head></head>里面增加以下内容
``` bash
<script language='javascript' src="js/main.js"></script>
```
### 在前台栏目模板中调用自定义缩略图
> 在模版里用：{dede:field.typeimg /} 是调不出数据的，所以改成SQL调用。
原来是这样的：
``` bash
{dede:channel type='top' row='13'}
     <li><a href='[field:typeurl/]' [field:rel/]>[field:typeimg/]</a></li>
{/dede:channel}
```
在这里面加上[field:typeimg]是调不出来的，栏目缩略图就是通过循环出来的，而循环不出来则意义不大，所以改成了如下：
``` bash
{dede:sql sql="SELECT typename,typedir,typeimg FROM dede_arctype"}
    <li><a href="[field:typedir/]">[field:typeimg/]</a></li>
{/dede:sql}
```
这样就顺利的调出来了,当然如果你要调用子ID的话，只要加上相应的条件ID调用就可以了。
> 添加或修改图片时在DedeCMS后台-《栏目管理》-高级选项中操作管理
### 以下内容个人尚未实例测试
如果想同时在文章内容页调用自定义添加的缩略图
打开\include\arc.archives.class.php
查找
``` bash
if($this->ChannelUnit->ChannelInfos['issystem']!=-1)
```
将
``` bash
$query = "Select arc.*,tp.reid,tp.typedir,ch.addtable
                 from `ant_archives` arc
                          left join ant_arctype tp on tp.id=arc.typeid
                           left join ant_channeltype as ch on arc.channel = ch.id
                           where arc.id='$aid' ";
                 $this->Fields = $this->dsql->GetOne($query);
```
替换为
``` bash
$query = "Select arc.*,tp.reid,tp.typedir,tp.typeimg,ch.addtable
                 from `ant_archives` arc
                          left join ant_arctype tp on tp.id=arc.typeid
                           left join ant_channeltype as ch on arc.channel = ch.id
                           where arc.id='$aid' ";
                 $this->Fields = $this->dsql->GetOne($query);
```
即可。
如果需要在内容页中使用栏目缩略图的朋友，可以去试一下。
