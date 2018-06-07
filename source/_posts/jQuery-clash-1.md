---
title: jQuery不同版本库同时引入冲突解决办法
date: 2018-01-21 17:08:15
tags: 
    - jQuery
    - 冲突
categories: jQuery
---
> jQuery多个版本或和其他js库冲突主要是常用的$符号的冲突。

## 一、常规解决办法
在同一页面中引入了多个版本的jQuery，导致js失效，可以去掉其中一个版本的jQuery，但是最好保证jQuery的引入在其他js之前。优先尝试保留新版本jQuery，注释掉旧版本的；如若不行，尝试保留旧版本的jQuery。
但是有一些特殊情况：我们引入的js插件，有的依赖于新版本jQuery的特性，有的依赖于旧版本jQuery的特性，比如说，你的业务代码采用了最新版的jQuery库，而你选用的第三方插件依赖的更早版本的jQuery库（大多数原因是js插件未持续更新）；又比如说，你正维护着一个系统，它已有的业务代码由于各种原因，引用了较老版本的jQuery库，你新开发的模块采用的是其他版本的jQuery库。这种情况又该如何处理？
<center>
![jQuery-clash-1](https://raw.githubusercontent.com/WangEn/BlogBackup/master/blogimgs/jqueryClash101.jpg)
</center>
最直白的办法，寻找其他可替换的js插件，只保留一个jQuery即可。但是有时候我们必须保留现有的js插件，也就是说两个版本的jQuery都必须保留，怎么办？请继续往下看：
## 二、认识jQuery.noConflict()
jQuery是目前使用最广泛的前端框架之一，有大量的第三方库和插件基于它开发。为了避免全局命名空间污染，jQuery提供了jQuery.noConflict()方法解决变量冲突。这个方法，毫无疑问，非常有效。遗憾的是，jQuery的官方文档对该方法的描述不够清晰，许多开发者并不清楚当他们调用jQuery.noConflict()时，究竟发生了什么，从而导致在使用时出现了许多问题。尽管如此，jQuery.noConflict()背后实现原理依然值得Web开发者学习掌握，成为解决类似全局命名空间污染问题的利器。
### 1、jQuery.noConflict()的作用？
jQuery.noConflict()的存在只有一个目的：它允许你在同一个页面加载多个jQuery实例，尤其是不同版本的jQuery。在多版本jQuery共存时，不得不面对jQuery对象/方法冲突的问题。幸运的是，jQuery.noConflict()帮你解决了这个烦恼。
### 2、jQuery被加载时发生了什么？
当jQuery被页面引用/加载时，它被封装在一个自执行函数（匿名函数）里，它提供的所有一切变量、函数、对象都在匿名函数内部的可执行环境内，外部环境无法调用,以防止全局命名空间污染。
``` bash
<script src="//ajax.googleapis.com/ajax/libs/jquery/1.10.2/jquery.min.js"></script>
```
jQuery在匿名函数内部定义了两个全局对象:`jQuery`和`$`，把自己暴露给外部环境。开发者习惯使用的各种公共方法都是通过这两个对象进行访问的，如jQuery.ajax()/jQuery.css()等。在最初，它们指向匿名函数内部的同一个对象jQuery(私有变量)，通过它访问匿名函数内部的私有变量和函数。这使得匿名函数在自执行后其内部的私有变量和函数仍然进驻在内存里，不会被javascript的垃圾回收机制清除。
``` bash
window.jQuery = window.$ = jQuery;
```

当jQuery被页面加载后，当前页面有可能已经存在了`jQuery`和`$`这两个全局变量（比如，加载了其它的第三方库，其内部也定义了它俩），这就会导致已经存在的对象被覆盖（全局命名空间污染）。为了解决这个问题，jQuery在内部先将已经存在的全局变量缓存起来，保存在私有变量\_jQuery和\_\$中，供后续调用。所以，如果页面在加载jQuery库时，还不存在`jQuery`和`$`对象，那么\_jQuery和\_\$都是undefined；否则，它们都会保存对已有`jQuery`和`$`的引用（也许来自之前引用的第三方库或是不同版本的jQuery库）。之后，jQuery会像上文说描述的那样，覆盖这两个全局变量并将自己暴露给外部环境。至此，页面上的全局变量`jQuery`和`$`已经指向刚刚引入的jQuery库。
``` bash
// Map over jQuery in case of overwrite
_jQuery = window.jQuery,

// Map over the $ in case of overwrite
_$ = window.$,

// Otherwise expose jQuery to the global object as usual
window.jQuery = window.$ = jQuery;
```
### 3、jQuery.noConflict()的神奇效果？

假设你维护的系统已经引用了1.7.0版本的jQuery库，而你在新添加的功能里引用了1.10.2版本的jQuery库。那么，还有办法重新使用jQuery 1.7.0 或是同时使用两个版本的jQuery库吗？答案是肯定，那就是jQuery.noConflict()。实际上，利用jQuery.noConflict()，你可以立刻把全局变量`jQuery`和`$`重新指向之前引用的对象。很神奇吧？这就是为什么jQuery在对外暴露自己前内部缓存了之前引用的对象。
jQuery.noConflict()接受一个可选的布尔值参数，通常默认值是false。这个参数会带来什么影响呢？其实，很简单。如果调用jQuery.noConflict()或是jQuery.noConflict(false)，只有全局变量`$`会被重置恢复成之前的引用值；如果调用jQuery.noConflict(true)，那么全局变量`jQuery`和`$`都会被重置恢复成之前的引用值。**这一点非常重要，建议牢记**。当你调用jQuery.noConflict(false/true)之后，它会返回当前jQuery的实例，利用这个特性我们可以实现jQuery的重命名。
``` bash
// "Renaming" jQuery
var jayquery = jQuery.noConflict( true );
// Now we can call things like jayquery.ajax(), jayquery.css(), and so on
```
我们再来看一个代码片段，测试一下是否真正理解了神奇的noConflict()
``` bash
<!-- jQuery and $ are undefined -->
<script src="//ajax.googleapis.com/ajax/libs/jquery/1.10.2/jquery.min.js"></script>
<!-- jQuery and $ now point to jQuery 1.10.2 -->
<script src="//ajax.googleapis.com/ajax/libs/jquery/1.7.0/jquery.min.js">
<!-- jQuery and $ now point to jQuery 1.7.0 -->
<script>jQuery.noConflict();</script>
<!-- jQuery still points to jQuery 1.7.0; $ now points to jQuery 1.10.2 -->
<script src="//ajax.googleapis.com/ajax/libs/jquery/1.6.4/jquery.min.js">
<!-- jQuery and $ now point to jQuery 1.6.4 -->
<script>var jquery164 = jQuery.noConflict( true );</script>
<!-- jQuery now points to jQuery 1.7.0; $ now points to jQuery 1.10.2; jquery164 points to jQuery 1.6.4 -->
```
### 4、避免第三方库的冲突
以上的代码片段展示了如何解决多版本jQuery的冲突。接下来，我们尝试解决jQuery库和第三方库的冲突，下面出现的代码片段在jQuery的官方文档中都有，有兴趣的程序猿可以仔细阅读官方文档体会其中的区别。

#### 直接使用No-Conflict模式
使用No-Conflict模式，其实就是对jQuery进行重命名，再调用。
``` bash
<!-- 采用no-conflict模式，jquery.js在prototype.js之后被引入. -->
<script src="prototype.js"></script>
<script src="jquery.js"></script>
<script>
    var $j = jQuery.noConflict();
    // $j 引用了jQuery对象本身.
    $j(document).ready(function() {
        $j( "div" ).hide();
    });
    // $ 被重新指向prototype.js里定义的对象
    // document.getElementById(). mainDiv below is a DOM element, not a jQuery object.
    window.onload = function() {
        var mainDiv = $( "main" );
    }
</script>
```
#### 使用自执行函数封装
使用这种方式，你可以在匿名函数内部继续使用标准的`$`对象，这也是众多jQuery插件采用的方法。需要注意的是，使用这种方法，函数内部无法再使用prototype.js定义的`$`对象了。
``` bash
<!-- jquery.js在prototype.js之后被引入. -->
<script src="prototype.js"></script>
<script src="jquery.js"></script>
<script>
    jQuery.noConflict();
    (function( $ ) {
        // Your jQuery code here, using the $
    })( jQuery );
</script>
```
#### 使用标准jQuery(document).ready()函数
如果jQuery库在其它库之前引入，那么jQuery内部定义的`jQuery`和`$`会被第三方库覆盖，这时候再使用noConflict()已经没有什么意义了。解决的方法很简单，直接使用jQuery的标准调用方式。
``` bash
<!-- jquery.js在prototype.js之前被引入. -->
<script src="jquery.js"></script>
<script src="prototype.js"></script>
<script>
    // Use full jQuery function name to reference jQuery.
    jQuery( document ).ready(function() {
        jQuery( "div" ).hide();
    });
    // 或者
    jQuery(function($){
        // Your jQuery code here, using the $
    });
    // Use the $ variable as defined in prototype.js
    window.onload = function() {
        var mainDiv = $( "main" );
    };
</script>
```

## 三、冲突的解决
jQuery.noConflict()是解决jQuery冲突的主力军，通过不同情形下的调用来解决不同版本库共存的冲突。
### 1、同一页面jQuery多个版本冲突解决方法
``` bash
<!-- 引入1.6.4版的jq -->
<script src="http://ajax.googleapis.com/ajax/libs/jquery/1.6.4/jquery.js"></script>
<script> var jq164 = jQuery.noConflict(true); </script>
<!-- 引入1.4.2版的jq -->
<script src="http://ajax.googleapis.com/ajax/libs/jquery/1.4.2/jquery.js"></script>
<script> var jq142 = jQuery.noConflict(true); </script>

<script>
(function($){
    //此时的$是jQuery-1.6.4
    $('#');
})(jq164);
</script>
 
<script>
jq142(function($){
    //此时的$是jQuery-1.4.2
    $('#');
});
</script>
    
</body>
```

### 2、jQuery库在其他库之后导入

> jQuery noConflict() 方法会释放会 $ 标识符的控制，这样其他脚本就可以使用它了。

#### 2-1 可以通过jQuery全名替代简写的方式来使用 jQuery
在其他库和jQuery库都加载完毕后，可以在任何时候调用jQuery.noConflict()函数来将变量$的控制权移交给其他JavaSript库。然后就可以在程序里将jQuery()函数作为jQuery对象的制造工厂。

``` bash
<script src="prototype.js" type="text/javascript"></script>
<script src="jquery.js" type="text/javascript"></script>

<p id="pp">test---prototype</p>
<p>test---jQuery</p>

<script type="text/javascript">
jQuery.noConflict();                
//将变量$的控制权让渡给prototype.js，全名可以不调用。
jQuery(function(){                    //使用jQuery
    jQuery("p").click(function(){
        alert( jQuery(this).text() );
    });
});
//此处不可以再写成$,此时的$代表prototype.js中定义的$符号。

$("pp").style.display = 'none';        //使用prototype
</script>
```

#### 2-2 自定义一个快捷方式
> noConflict() 可返回对 jQuery 的引用，可以把它存入自定义名称，例如jq,$J变量，以供稍后使用。

这样可以确保jQuery不会与其他库冲突，同时又使用自定义一个快捷方式。

``` bash
<script type="text/javascript">
var $j = jQuery.noConflict();        //自定义一个比较短快捷方式
$j(function(){                        //使用jQuery
    $j("p").click(function(){
        alert( $j(this).text() );
    });
});

$("pp").style.display = 'none';        //使用prototype
</script>
```
#### 2-3 在不冲突的情况下仍然用$

如果想在jQuery 代码块使用 \$ 简写，不愿意改变这个快捷方式，可以把 \$ 符号作为变量传递给 ready 方法。这样就可以在函数内使用 \$ 符号了 ， 而在函数外，依旧不得不使用 "jQuery"。

``` bash
<script type="text/javascript">
jQuery.noConflict();     //将变量$的控制权让渡给prototype.js
jQuery(document).ready(function($){
    $("p").click(function(){        //继续使用 $ 方法
        alert( $(this).text() );
    });
});
//或者如下
jQuery(function($){                    //使用jQuery
    $("p").click(function(){        //继续使用 $ 方法
        alert( $(this).text() );
    });
});
</script>
```
或者使用IEF语句块，这应该是最理想的方式，因为可以通过改变最少的代码来实现全面的兼容性。

在我们自己写jquery插件时,应该都使用这种写法，因为我们不知道具体工作过程中是如何顺序引入各种js库的,而这种语句块的写法却能屏蔽冲突。

``` bash
<script type="text/javascript">
jQuery.noConflict();                //将变量$的控制权让渡给prototype.js
(function($){                        //定义匿名函数并设置形参为$
    $(function(){                    //匿名函数内部的$均为jQuery
        $("p").click(function(){    //继续使用 $ 方法
            alert($(this).text());
        });
    });
})(jQuery);                            //执行匿名函数且传递实参jQuery

$("pp").style.display = 'none';        //使用prototype
</script>
```
### 3、jQuery库在其他库之前导入
> jQuery库在其他库之前导入，$的归属权默认归后面的JavaScript库所有。那么可以直接使用"jQuery"来做一些jQuery的工作。

同时，可以使用$()方法作为其他库的快捷方式。这里无须调用jQuery.noConflict()函数。

``` bash
<!-- 引入 jQuery  -->
<script src="../../scripts/jquery.js" type="text/javascript"></script>
<!-- 引入 prototype  -->
<script src="lib/prototype.js" type="text/javascript"></script>

<body>
<p id="pp">Test-prototype(将被隐藏)</p>
<p >Test-jQuery(将被绑定单击事件)</p>

<script type="text/javascript">
jQuery(function(){   //直接使用 jQuery ,没有必要调用"jQuery.noConflict()"函数。
    jQuery("p").click(function(){      
        alert( jQuery(this).text() );
    });
});

$("pp").style.display = 'none'; //使用prototype
</script>
</body>
```
## 四、原理
### 1、源码
源码：看一下源码里怎么做到的

``` bash
var
// Map over jQuery in case of overwrite
_jQuery = window.jQuery,

// Map over the $ in case of overwrite
_$ = window.$,

jQuery.extend({
    noConflict: function( deep ) {
            if ( window.$ === jQuery ) {
                window.$ = _$;
            }

            if ( deep && window.jQuery === jQuery ) {
                window.jQuery = _jQuery;
            }

            return jQuery;
        }
});
```
在jQuery加载的时候，通过事先声明的_jQuery变量获取到当前window.jQuery，通过\_$ 获取到当前`window.$`。

通过jQuery.extend()把noConflict挂载到jQuery下面。所以我们在调用的时候都是jQuery.noConflict()这样调。

在调用noConflict()时做了2个判断，

第一个if，把$的控制权交出去。

第二个if，在noConflict()传参的时候把，jQuery的控制权交出去。

最后noConflict()返回jQuery对象，用哪个参数接收，哪个参数将拥有jQuery的控制权。

### 2、 验证
``` bash
//冲突   
var $ = 123;  //假设其他库中$为123
$(
    function () {
        console.log($); 
        # //报错Uncaught TypeError: $ is not a function
    }
);
```
解决冲突
``` bash
#//解决冲突
var jq = $.noConflict();
var $ = 123;
jq(function () {
    alert($); //123
});
```

#### 释放\$控制权例子

``` bash
<script>
    var $ = 123; // window.$是123，存储在私有的_$上。
</script>
<script src="https://code.jquery.com/jquery-2.2.4.js"></script>
<div>aaa</div>
<script>
    var jq = $.noConflict();
    #//当window.$===jQuery的时候，把_$赋给了window.$。
    jq(function () {
        alert($); //123
    });
</script>
``` 
#### 释放jQuery控制权例子

参数deep的作用：deep用来放弃jQuery对外的接口。
如下，noConflict()不写参数，弹出jQuery为构造函数。

``` bash
<script>
    var $ = 123;
    var jQuery=456;
</script>
<script src="https://code.jquery.com/jquery-2.0.3.js"></script>
<div>aaa</div>
<script>
    var jq = $.noConflict();
    jq(function () {
        alert(jQuery); //构造函数
    });
</script>
```


如果写个参数true，会弹出456。

``` bash
<script>
    var $ = 123;
    var jQuery=456;
</script>
<script src="https://code.jquery.com/jquery-2.0.3.js"></script>
<div>aaa</div>
<script>
    var jq = $.noConflict(true);
#//写了true或者参数的时候，deep为真window.jQuery===jQuery为真，所以进入if条件。把456赋值给window.jQuery
    jq(function () {
        alert(jQuery); //456
    });
</script>
```

## 写在最后
 
因知识本身在变化，作者也在不断学习成长，文章内容也不定时更新，为避免误导读者，留下参考教程，方便追根溯源。

> 1、[starof:jQuery库冲突解决办法](http://www.cnblogs.com/starof/p/6855186.html)
> 2、[jQuery多个版本和其他js库冲突的解决方法](http://www.jb51.net/article/90293.htm)
> 3、[老俞：三分钟玩转jQuery.noConflict()](https://www.cnblogs.com/laoyu/p/5189750.html)



