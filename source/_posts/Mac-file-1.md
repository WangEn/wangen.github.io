---
title: Mac OS常用操作命令备忘
date: 2018-01-15 23:45:22
tags: 
    - Mac
    - 文件操作
    - 目录路径
categories: Mac
---
## 查看复制文件路径
Launchpad-【其他】中打开```终端```输入以下命令

``` bash
defaults write com.apple.finder _FXShowPosixPathInTitle -bool YES
```
之后，就能在Finder顶部看见完整的路径地址了
![Finder顶部路径](https://raw.githubusercontent.com/WangEn/BlogBackup/master/blogimgs/macfile101.jpg)
在Finder顶端的地址栏右键，还可以直接访问路径中的任意一层
![Finder右键路径](https://raw.githubusercontent.com/WangEn/BlogBackup/master/blogimgs/macfile102.jpg)
复制路径的方法：
1、推荐方案-『复制路径可以用 Option+Command+C』
2、右键文件-显示简介（或者使用快捷键cmd + i），复制位置信息，在文本中粘贴即可得到路径信息。
3、打开一个可以粘贴文字的应用（比如说终端，记事本），然后把要获取路径的【文件/文件夹】拖进去，看到的就是对应的绝对路径。

## 常用快捷键一览
![Mac OS X快捷键](https://raw.githubusercontent.com/WangEn/BlogBackup/master/blogimgs/macfile103.jpg)
