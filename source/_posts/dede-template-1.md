---
title: DedeCMS网站批量更新所有目录模板文件
date: 2017-12-16 00:23:03
tags: 
    - DedeCMS
    - Template
categories: DedeCMS
---
> 通过MySQL命令来批量修改DedeCMS使用的模板文件，建议使用PHPMyAdmin等数据库管理工具打开操作，建议先备份数据库到本地

## 修改栏目频道页
``` bash
update dede_arctype set tempindex = replace(tempindex,"{style}/page.htm","{style}/index_article.htm")
```
## 修改栏目列表页
```bash
update dede_arctype set templist = replace(templist,"{style}/page.htm","{style}/list_article.htm")
```
## 修改栏目内容页
```bash
update dede_arctype set temparticle = replace(temparticle,"{style}/page.htm","{style}/article_article.htm")
```

## 修改指定ID栏目的列表页
```bash
update dede_arctype set templist = replace(templist,"{style}/page.htm","{style}/list_article.htm") where id in(1,2,3,4,5,6)
```
## 后记
以上所有命令都通用，在前三种后面加上where id in(ID1,ID2,ID3,ID4,ID5,……)，里面写入你打算修改的频道/栏目/文章ID，可批量修改指定ID的模板文件。