---
title: 制作透明背景 PNG 图片和 GIF 动图的几种方法
date: 2017-12-17 00:51:42
tags: 
    - png
    - gif
    - Photoshop
    - PowerPoint
categories: Images
---
偶尔需要将图片的背景透明，渐渐掌握了几种较为简便的处理方法。写篇文章记录一下经验，免得自己哪天又忘了。
## PowerPoint → PNG
> 微软幻灯片工具 PowerPoint 也可以用来简单处理下图片，比如透明化背景。

1、打开 PowerPoint，新建一张幻灯片页面，然后把待处理的图片拖进去。
2、依次点击 **格式** 选项卡 - **颜色** 菜单 - **设置透明色** - 点选 **背景色**。这样背景就透明化了，再右键图片另存为 ```PNG 格式``` 即可。
3、 **删除背景** 功能类似，不过感觉没上面的方式快捷。

![PPT-PNG](https://raw.githubusercontent.com/WangEn/BlogBackup/master/blogimgs/PPT-PNG.jpg)

## PhotoShop → PNG
> 如果电脑上安了 PhotoShop，那我们可以更灵活地制作透明背景图片。
1、用 PS 打开待处理的静态图片，双击图层框右侧小锁标志解锁图层;
2、选用魔棒工具，调整容差为10左右(容差越大，选取的相似颜色越多)，勾选消除锯齿;
3、点选背景，按键盘 ```del```键删除之(按住 ```shift``` 键可以多选)，之后将图片另存为 PNG 格式即可。
![PS-PNG](https://raw.githubusercontent.com/WangEn/BlogBackup/master/blogimgs/PS-PNG.jpg)

## PhotoShop → GIF
> PhotoShop 用来透明化 GIF 动图背景也是很方便的。
1、用 PS 打开待处理的 GIF 动图，并确保时间轴窗口已显示(窗口菜单 - 勾选时间轴)；
2、全选时间轴中的图片(可利用 ```shift``` 键全选)，右键图片，勾选自动(跳过此步生产的动图会有重影)；
3、点击文件菜单 - 存储为 Web 所用格式...；
4、在颜色表中，先点选的小星点，再点击下方第一个按钮将背景透明，之后点击存储...保存 GIF 到目标位置即可。
![PS-GIF-1](https://raw.githubusercontent.com/WangEn/BlogBackup/master/blogimgs/PS-GIF-1.jpg)
![PS-GIF-2](https://raw.githubusercontent.com/WangEn/BlogBackup/master/blogimgs/PS-GIF-2.jpg)

## 一些成品
> 展示一些已经透明化背景的图片。
![Mihawk](https://raw.githubusercontent.com/WangEn/BlogBackup/master/blogimgs/Mihawk.gif)![food-boy](https://raw.githubusercontent.com/WangEn/BlogBackup/master/blogimgs/food-boy.gif)![bilibili](https://raw.githubusercontent.com/WangEn/BlogBackup/master/blogimgs/bilibili.gif)

#### 拓展阅读
> * 移动端图片格式调研 by ibireme on 2015/11/2: http://blog.ibireme.com/2015/11/02/mobile_image_benchmark/
> * 解决GIF动画图去背景后出现的重影 by 有烟飘过 on 2010/5/10: http://tieba.baidu.com/p/2106457600
> * 制作透明背景 PNG 图片和 GIF 动图的几种方法 by MOxFIVE on 2015-11-17: http://moxfive.xyz/2015/11/16/png-gif-transparency/