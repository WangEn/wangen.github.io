---
title: 使用Firefox火狐浏览器模拟手机等多种浏览器
date: 2017-12-19 23:58:31
tags:
    - Firefox
    - 组件
categories: Browser
---

## 下载旧版本Firefox浏览器
官方下载地址：http://ftp.mozilla.org/pub/firefox/releases/
因为最新更新的Firefox57版本使用了新引擎，之前的绝大部分扩展组件都不再支持，所以我们要选用适合自己电脑系统的旧版本Firefox浏览器下载作为开发使用。
我先用的是[Firefox 53.0 64位版本](http://ftp.mozilla.org/pub/firefox/releases/53.0/win64/)（win10 64位系统环境下），仅作为参考
> 安装完成后，打开浏览器在【工具】-【选项】-【高级】-【更新】中选择“不检查更新”

## 安装附加组件User Agent Switcher
【工具】-【附加组件】-【搜索】User Agent Switcher，安装后重启浏览器
> 点击菜单栏的【工具】下拉，选择"iPhone3.0"选项即可在火狐浏览器模拟手机浏览器了

## 扩展UA列表伪装更多平台版本
1、工具-default user agent-User Agent Switcher-options，打开User Agent Switcher选项卡，点击右下角网页链接，就能找到最常用的UA列表了
2、打开techpatterns网页，下载网页为你提供的.xml文件地址。如果觉得此步骤麻烦，点击以下下载地址进入下载。
下载地址：https://techpatterns.com/forums/about304.html
点击 ``` Download via Save As File Dialogue ```将xml文件下载到本地。
3、打开 【User Agent Switcher】组件，在该组件面板【选项】中导入下载的xml文件后重启浏览器即可。导入下载好的.xml文件，即可体验更多的平台模拟了。

