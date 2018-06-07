---
title: 视频播放插件Video.js一些API的使用介绍
date: 2017-12-23 23:37:17
tags: 
     - HTML5
     - Video
     - Video.js
     - 视频
categories: HTML5
---
### 插件基本说明
Video.js 是一个通用的在网页上嵌入视频播放器的 JS 库，Video.js 自动检测浏览器对 HTML5 的支持情况，如果不支持 HTML5 则自动使用 Flash 播放器。（要支持ie低版本请下载5.4.3版 ）
github地址： https://github.com/videojs/video.js ，可以自行选择版本标签点击下载。
 > V6.6.0版本下载地址：[点击下载](https://codeload.github.com/videojs/video.js/zip/v6.6.0)
 > V5.4.3旧版本下载地址：[下载地址](https://codeload.github.com/videojs/video.js/zip/v5.4.3)
 
### 使用方法

#### 静态调用

> 引入文件

在页面中引用video-js.css样式文件和video.js
``` bash
<link href="video-js.css" rel="stylesheet" type="text/css">
<script src="video.js"></script>
```
设置flash路径，Video.js会在不支持html5的浏览中使用flash播放视频文件
``` bash
<script>
    videojs.options.flash.swf = "video-js.swf";
</script>
```
> HTML调用代码

poster="**"设置播放初始图片。
< source src="" >可设置使用三种视频格式，根据所需要格式选择对应的。

``` bash
<video id="example_video_1" class="video-js vjs-default-skin" controls preload="none" width="640" height="264" poster="http://video-js.zencoder.com/oceans-clip.png" data-setup="{}">
    <source src="http://视频地址格式1.mp4" type='video/mp4' />
    <source src="http://视频地址格式2.webm" type='video/webm' />
    <source src="http://视频地址格式3.ogv" type='video/ogg' />
    <track kind="captions" src="demo.captions.vtt" srclang="en" label="English"></track>
    <!-- Tracks need an ending tag thanks to IE9 -->
    <track kind="subtitles" src="demo.captions.vtt" srclang="en" label="English"></track>
    <!-- Tracks need an ending tag thanks to IE9 -->
</video>
```
> JavaScript设置调用 

通过JavaScript标签设置相关属性，比如设置`自动播放`，是将如下代码加到html中代码后面
``` bash
<script type="text/javascript">
    var myPlayer = videojs('example_video_1');
    videojs("example_video_1").ready(function(){
        var myPlayer = this;
        myPlayer.play();
    });
</script>
```
> 额外样式自定义

默认情况下，大的播放按钮是被定为在左上角的，这样就不会覆盖视频内容。如果你想让这个播放按钮居中，你可以给你的 video 标签添加额外的 `vjs-big-play-centered` 样式，比如：

``` bash
<video id="example_video_1" class="video-js vjs-default-skin vjs-big-play-centered" controls preload="auto" width="640" height="264" poster="http://video-js.zencoder.com/oceans-clip.png" data-setup='{"example_option":true}'>
  ...
</video>
```
如果你还对播放按钮样式不满意可重新定义`.video-js .vjs-big-play-button`{/*继续将这里的样式重写*/}。

#### 动态加载使用

如果你的 web 页面或者应用是动态加载video标签的（ajax，appendChild，等等）,这样在页面加载后这个元素是不存在的，那么你会想要手动设置播放器而不是依靠`data-setup`属性。要做到这一点，首先将`data-setup` 属性从 video 标签中移除掉，这样在播放器初始化的时候就不会混乱了。接下来，运行下面的 JavaScript ，有时在 Video.js 加载后，有时是在 video 标签被加载进 DOM 后:
``` bash
videojs("example_video_1", {}, function(){
  // Player (this) is initialized and ready.
});
```
videojs 方法中的第一个参数是你的 video 标签的 ID，用你自己的代替。
第二个参数是一个选项对象。它允许你像设置`data-setup`属性一样设置额外的选项。
第三个参数是一个'ready'回调。一旦Video.js初始化完成后，就会触发这个回调。

当然，你也可以传入一个元素本身的引用来代替元素ID：
``` bash
videojs(document.getElementById('example_video_1'), {}, function() {
  // This is functionally the same as the previous example.
});

videojs(document.getElementsByClassName('awesome_video_class')[0], {}, function() {
  // You can grab an element by class if you'd like, just make sure
  // if it's an array that you pick one (here we chose the first).
});
```
> 如果您无法播放内容，您得确保使用了正确的格式，你的 HTTP 服务器可能无法提供正确的`MIME类型`的内容

### 一些常用的API属性
获取video实例：
``` bash
var videoObj = videojs(“videoId”);
```
初始化回调ready：
``` bash
myPlayer.ready(function(){
    //在回调函数中，this代表当前播放器，
    //可以调用方法，也可以绑定事件。
})
```
播放：
``` bash
myPlayer.play();
```
暂停：
``` bash
myPlayer.pause();
```
获取播放进度（当前播放时间）：
``` bash
var whereYouAt = myPlayer.currentTime();
```
设置播放进度：
``` bash
myPlayer.currentTime(120);
```
视频持续时间，加载完成视频才可以知道视频时长，且在flash情况下无效:
``` bash
var howLongIsThis = myPlayer.duration();
```
缓冲，就是返回下载了多少：
``` bash
var whatHasBeenBuffered = myPlayer.buffered();
```
百分比的缓冲：
``` bash
var howMuchIsDownloaded = myPlayer.bufferedPercent();
```
声音大小（0-1之间）：
``` bash
var howLoudIsIt = myPlayer.volume();
```
设置声音大小：
``` bash
myPlayer.volume(0.5);
```
获取/设置视频的宽度：
``` bash
var howWideIsIt = myPlayer.width(); //getter width
myPlayer.width(640); //setter width
var howTallIsIt = myPlayer.height(); //getter height
myPlayer.height(480); //setter height
```
一步到位，设置视频大小（宽度高度）：
``` bash
myPlayer.size(640,480);
```
设置进入全屏/退出全屏(HTML5播放器下有效)：
``` bash
myPlayer.enterFullScreen();
myPlayer.exitFullScreen();
//http://docs.videojs.com/html5#enterFullScreen
```
添加事件：
``` bash
durationchange //提示视频的时长已改变
ended //播放结束
firstplay
fullscreenchange
loadedalldata
loadeddata
loadedmetadata
loadstart
pause //暂停
play  //播放
progress
seeked
seeking
timeupdate
volumechange
waiting
resize inherited
  
var myFunc = function(){
    // Do something when the event is fired
};

```
事件绑定：
``` bash
myPlayer.on("ended", function(){
    console.log("end", this.currentTime());
});
myPlayer.on("pause", function(){
    console.log("pause")
});
```
事件移除：
``` bash
myPlayer.removeEvent(“eventName”, myFunc);
```

> 同一页面多个视频调用示例：

一个页面中有多个视频，需要点击后触发bootstrap 的模态窗，再弹出视频，实现如下：
主要HTML代码部分：
``` bash
<a videohref="http://xxx.mp4" class="video_link"><img  src="../images/xxx.jpg"/></a>
```

主要JS代码部分：
``` bash
$(".video_link").click(function() {
    var myPlayer = videojs('my-video');
    var videoUrl = $(this).attr("videohref");
    videojs("my-video", {}, function() {
        window.myPlayer = this;
        $("#mymoda .video-con #my-video source").attr("src", videoUrl);
        myPlayer.src(videoUrl);
        myPlayer.load(videoUrl);
        myPlayer.play();
    });
    $(".click-modal").click();
});
// 模态窗消失时，关闭视频    
$('#mymoda').on('hidden.bs.modal', function() {
    myPlayer.pause();
});
```
更多API使用可查看[官网文档](http://docs.videojs.com/html5)。

### 写在最后

> 转载来源：[视频播放插件Video.js](http://www.jq22.com/jquery-info404)
> 插件官网：[Video.js](http://videojs.com)