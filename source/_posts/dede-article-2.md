---
title: DedeCMS织梦首页调用指定一篇文章body内容的方法
date: 2017-12-16 00:16:24
tags: 
    - DedeCMS
    - 织梦
    - body
categories: DedeCMS
---
> 我们常会碰到一些问题，比如DedeCMS网站在首页调用某一篇文档的正文内容，今天实例讲述了dedecms首页调用指定一篇文章body内容的方法。

分享给大家供大家参考。具体实现方法如下：
## 直接调用（包含样式等代码）
代码如下：
``` bash
{dede:arclist idlist='要调用文章的id' channelid='1' addfields='body'}
[field:body function='cn_substr(@me,330)'/]
{/dede:arclist}
```
## 过滤调用（只调用纯文本内容）
如果是要过滤掉文本中的html，使用如下：
``` bash
{dede:arclist idlist='要调用文章的id' channelid='1' addfields='body'}
[field:body function='cn_substr(html2text(@me),330)'/]
{/dede:arclist}
```
## 后记
idlist 是要调用文章的id, channelid 是这个内容模型id,  addfields 是要调用附加表里面的字段.

> 本文参考地址[来源](http://www.xiuzhanwang.com/dedecms_jq/765.html)
