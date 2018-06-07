---
title: 使用layer的iframe层提交表单后,关闭当前iframe，然后刷新父页面
date: 2018-01-28 14:51:05
tags: 
    - layui
    - layer
    - iframe
categories: layui
---

需求：需要使用iframe来实现添加新账号的功能，在iframe页面中通过表单提交新账号的资料后，关闭iframe页面，并且刷新父页面账号列表。
## layer参考

layer独立组件官网：[http://layer.layui.com/](http://layer.layui.com/)
layui文档：[弹层组件文档](http://www.layui.com/doc/modules/layer.html)
> iframe层-父子操作子页面主要代码
``` javascript
//给父页面传值
$('#transmit').on('click', function(){
    parent.$('#parentIframe').text('我被改变了');
    parent.layer.tips('Look here', '#parentIframe', {time: 5000});
    parent.layer.close(index);
});
```

## 问题
直接在iframe页面通过`$('#transmit').on('click', function(){});`来执行关闭及刷新操作，会造成本页面的form表单无法提交到后台，因此换用ajax来提交form表单，判断ajax提交成功/失败后，再进行父页面传值等操作。

## 代码实现
主要代码如下：
### HTML相关代码
``` html
<form class="form-horizontal" id="myform">
	<div class="box-body">
		<div class="form-group col-sm-6">
			<label for="inputAccount1" class="col-sm-4 control-label">微信号</label>
			<div class="col-sm-8">
				<input type="text" class="form-control" id="inputAccount1" placeholder="微信号">
			</div>
		</div>
		<div class="form-group col-sm-6">
			<label for="inputAccount2" class="col-sm-4 control-label">手机号</label>
			<div class="col-sm-8">
				<input type="text" class="form-control" id="inputAccount2" placeholder="手机号">
			</div>
		</div>
		<div class="form-group col-sm-12">
			<label for="inputAccount3" class="col-sm-2 control-label">二维码</label>
			<div class="col-sm-10">
				<a href="javascript:;" id="inputAccount3" class="upload">点击上传图片<input class="change" type="file" multiple="multiple" />
				</a>
			</div>
		</div>
	</div>
</form>
```

### js相关代码

``` javascript
<script src="/js/jquery-2.2.3.min.js"></script>
<script src="/js/layer/layer.min.js"></script>
<script type="text/javascript">
	var index = parent.layer.getFrameIndex(window.name);
	//获取iframe父页面索引
    $('form').submit(function() {
    	$.ajax({
            url:"url",
            type:"post",
            data:$(this).serialize(),
            success:function(data){
                //console.log(data);
                parent.location.reload(); //刷新父页面
		        parent.layer.close(index);  //关闭当前页面，写在最后面
            },
            error:function(data){
            	//console.log("Error~");
                //console.log(data);
                parent.location.reload();
		        parent.layer.close(index);
            }
       });   
	});
</script>
```

### Tips
关于layer弹层iframe窗的子父操作，`parent.layer.close(index)`这个关闭操作一定要写在最后面，大概是因为执行关闭执行，其他操作就不会执行了。

## 写在最后

> 参考文档1：[layer弹窗iframe页面，关闭弹窗方法导致form表单无法提交到服务器](http://blog.csdn.net/u011848397/article/details/52353226?locationNum=11)
> 参考文档2：[使用layer的iframe层提交表单后，需要关闭当前的iframe层，然后刷新父页面的方法](http://blog.csdn.net/u011020900/article/details/52083166)
