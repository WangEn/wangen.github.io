---
title: 使用HTML5 <video> 标签-调用视频的当前播放时间值
date: 2017-12-23 23:37:37
tags: 
     - HTML5
     - Video
     - 视频
categories: HTML5
---
### Video对象

Video 对象是 HTML5 中的新对象。

Video 对象表示 HTML < video > 元素。


### currentTime基础使用

在用HTML5技术处理视频时,设置(setting)和获取(getting)时间都是非常重要的知识点，尤其是对于记录播放时间实现续播的功能。在管理视频状态时，最重要的是要了解 ` currentTime `是个什么鬼,简而言之你可以通过这个属性获取当前播放到了哪个时间点（比如当前播放到了2分30秒，当然` currentTime ` 的单位是 ` 秒 ` (seconds)，所以我们获取到的值是 ` 150 `）.

``` bash
// https://www.youtube.com/watch?v=Cwkej79U3ek
console.log(video.currentTime);  // 25.431747
```
### 语法

返回(getter) currentTime 属性(数字值，表示当前播放的时间，以秒计)：

``` bash
videoObject.currentTime
```
设置 currentTime 属性：
``` bash
videoObject.currentTime=seconds
```
> currentTime 属性设置或返回视频播放的当前位置（以秒计）,当设置该属性时，播放会跳跃到指定的位置。

### 浏览器支持
所有主流浏览器都支持 currentTime 属性。
> 注意：Internet Explorer 8 或更早的浏览器不支持该属性。

` currentTime ` 既是 getter 又是 setter 属性, 所以可以直接设置 currentTime 值来控制播放进度:

``` bash
video.currentTime = 0; // Restart,初始化设置当前播放到了0秒
```
### 补充说明

API 接口很容易理解,而且是自解释的(self-explanatory)。你仍然需要处理“second”来指定时间,包括内在实际的和外在显示的(both inward and outward),但是秒(second)这个单位和你预期的一样公平,所以说这个API设计是非常巧妙的。

> 使用参考：小米空气净化器 http://www.mi.com/air/ (页面已撤销)

``` bash
<video id="videoIntro" class="video" data-video-name="intro" poster="http://c1.mifile.cn/f/i/2014/cn/goods/air/overall/video-main-poster.jpg" style="height: 600px; width: 800px; display: none;">
    <source type="video/mp4" src="http://c1.mifile.cn/f/i/2014/cn/goods/air/overall/video-intro.mp4?2014120901">
    <source type="video/webm" src="http://c1.mifile.cn/f/i/2014/cn/goods/air/overall/video-intro.webm?2014120901">
</video>
```
### 示例代码

``` bash
<!DOCTYPE html> 
<html> 
<head> 
<meta charset="utf-8"> 
<title>菜鸟教程(runoob.com)</title> 
</head> 
<body> 

<video id="myVideo" width="320" height="240" controls>
	<source src="http://www.runoob.com/try/demo_source/movie.mp4" type="video/mp4">
  	<source src="http://www.runoob.com/try/demo_source/movie.ogg" type="video/ogg">
  	您的浏览器不支持 video 标签。
</video><br>
<button onclick="getCurTime()" type="button">获取当前时间点</button>
<button onclick="setCurTime()" type="button">设置时间位置为5秒</button>
<script>
var x = document.getElementById("myVideo");
function getCurTime(){ 
	alert(x.currentTime);
} 
function setCurTime(){ 
	x.currentTime = 5;
} 
</script> 

</body> 
</html>
```

### 写在最后
其实关于HTML5-Video视频播放器，还有一款插件可以直接使用：[Video.js](http://videojs.com/)，使用方法和原生比较类似，不过目前中文文档还不多，大家有兴趣可以去了解下。

> 整理参考1：[译：获取并设置HTML5 Video的当前进度](http://blog.csdn.net/renfufei/article/details/44522887) - [原文地址](http://davidwalsh.name/html5-video-current-time)

> 整理参考2：[Video currentTime 属性](http://www.runoob.com/jsref/prop-video-currenttime.html)