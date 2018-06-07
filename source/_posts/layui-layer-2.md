---
title: 结合Layui实现iframe无刷新给父页面添加新数据
date: 2018-01-29 22:56:02
tags: 
    - layui
    - layer
    - iframe
categories: layui
---

## 需求概要
在父页面会员列表点击【添加】操作，弹出iFrame弹层，包含输入input各项信息的表单，编辑完成提交后，将表单中input的value值传递给父页面，并且在父页面会员列表table中无刷新添加一行表格`<tr>`，模拟后台数据的提交与接收。

## 参考资料

> [layer:iframe-子父操作](http://layer.layui.com/)

主要依赖参考：
``` javascript
//给父页面传值
$('#transmit').on('click', function(){
    parent.$('#parentIframe').text('我被改变了');
    parent.layer.tips('Look here', '#parentIframe', {time: 5000});
    parent.layer.close(index);
});
```

## 优化实现方法

frame页面确定增加时，触发给父页面传值事件。结合jQuery查询到父页面table的ID，将form表单提交的field值插入到父页面table结尾。以下示例代码，删除部分不需要内容：

### 主要HTML代码

``` html
<!--父页面添加按钮操作-->
<div class="weadmin-block">
	<button class="layui-btn layui-btn-danger" onclick="delAll()"><i class="layui-icon"></i>批量删除</button>
	<button class="layui-btn" onclick="WeAdminShow('添加用户','./add.html',600,400)"><i class="layui-icon"></i>添加</button>

</div>
<!--父页面会员列表-->
<table class="layui-table" id="memberList">
	<thead>
		<tr>
			<th>
				<div class="layui-unselect header layui-form-checkbox" lay-skin="primary"><i class="layui-icon">&#xe605;</i></div>
			</th>
			<th>ID</th>
			<th>用户名</th>
			<th>性别</th>
			<th>手机</th>
			<th>邮箱</th>
			<th>地址</th>
			<th>加入时间</th>
			<th>状态</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td>
				<div class="layui-unselect layui-form-checkbox" lay-skin="primary" data-id="2"><i class="layui-icon">&#xe605;</i></div>
			</td>
			<td>1</td>
			<td>小明</td>
			<td>男</td>
			<td>13000000000</td>
			<td>admin@mail.com</td>
			<td>北京市 朝阳区</td>
			<td>2017-01-01 11:11:42</td>
			<td class="td-status">
				<span class="layui-btn layui-btn-normal layui-btn-xs">已启用</span></td>
		</tr>
	</tbody>
</table>

<!--子页面frame 主要表单代码实例-->
<form class="layui-form">
	<div class="layui-form-item">
		<label for="L_username" class="layui-form-label"><span class="we-red">*</span>登录名</label>
		<div class="layui-input-inline">
			<input type="text" id="L_username" name="username" lay-verify="required|nikename" autocomplete="off" class="layui-input">
		</div>
		<div class="layui-form-mid layui-word-aux">
			请设置至少5个字符，将会成为您唯一的登录名
		</div>
	</div>
	<div class="layui-form-item">
	    <label for="L_sex" class="layui-form-label">性别</label>
	    <div class="layui-input-block" id="L_sex">
	      <input type="radio" name="sex" value="男" title="男" checked>
	      <input type="radio" name="sex" value="女" title="女">
	      <input type="radio" name="sex" value="未知" title="未知">
	    </div>
	</div>

	<div class="layui-form-item">
		<label for="L_email" class="layui-form-label"><span class="we-red">*</span>手机</label>
		<div class="layui-input-inline">
			<input type="text" id="L_phone" name="phone" lay-verify="required|phone" autocomplete="off" class="layui-input">
		</div>
	</div>
	<div class="layui-form-item">
		<label for="L_email" class="layui-form-label"><span class="we-red">*</span>邮箱</label>
		<div class="layui-input-inline">
			<input type="text" id="L_email" name="email" lay-verify="email" autocomplete="off" class="layui-input">
		</div>

	</div>
	<div class="layui-form-item">
		<label for="L_repass" class="layui-form-label"></label>
		<button class="layui-btn" lay-filter="add" lay-submit="">增加</button>
	</div>
</form>
```

### 主要JavaScript代码

``` javascript
<script>
layui.use(['form', 'layer'], function() {
	$ = layui.jquery;
	var form = layui.form,
		layer = layui.layer;
	//监听提交
	form.on('submit(add)', function(data) {
		console.log(data.field);
		var f = data.field; //获取表单中提交的value
		//发异步，把数据提交给php后台，此处模拟实现
		layer.alert("增加成功", {
			icon: 6
		}, function() {
			// 获得frame索引
			var index = parent.layer.getFrameIndex(window.name);

  			//获取父页面会员列表的长度，给每条数据赋值不同的data-id属性
			var _len = parent.$('#memberList tr').length;
			alert(_len);
			//根据需求，按照父页面表格已有字段，在table结尾插入一组数据
			parent.$('#memberList').append(						
				'<tr data-id="' + _len + '">' +
					'<td>'+
						'<div class="layui-unselect layui-form-checkbox" lay-skin="primary" data-id="' + _len + '"><i class="layui-icon">&#xe605;</i></div>'+
					'</td>'+
					'<td>' + _len + '</td>'+
					'<td>'+f.username+'</td>'+
					'<td>'+f.sex+'</td>'+
					'<td>'+f.phone+'</td>'+
					'<td>'+f.email+'</td>'+
					'<td>2018-01-01 11:11:42</td>'+
					'<td class="td-status"><span class="layui-btn layui-btn-normal layui-btn-xs">已启用</span></td>'+
					'</td>'+
				'</tr>'	
			);
			//关闭当前frame
			parent.layer.close(index);
		});
		return false;
	});
});
</script>
```

## 给父页面赋值后再添加的实现方法

> 根据自己的实际需求选择合适的实现方法最好，个人在此项实现场景中不推荐此方法，因为此项目中父页面对应多个frame子页面，每个frame子页面对应的代码写在自己页面内部，感觉可能代码会更规整一些。

### 实现思路：

在父页面创建一个隐藏的form表单用来接收frame子页面的传值，在父页面创建一个addMember()函数来在table尾部插入一组新数据；在iframe子页面编辑完成触发事件时，给父页面隐藏表单传值，然后执行父页面的addMember()函数。

### 增加HTML主要代码

在以上实现方法基础上，新增隐藏form表单
``` html
<!--列表页增加form表单，接收add页面传过来的值，模拟后台提交与接收-->
<form class="layui-form" id="modalForm" style="display: none;">
	<div class="layui-form-item">
		<label for="L_username" class="layui-form-label">
                <span class="we-red">*</span>登录名
            </label>
		<div class="layui-input-inline">
			<input type="text" id="L_username" name="username" class="layui-input">
		</div>
	</div>
	<div class="layui-form-item">
		<label for="L_sex" class="layui-form-label">性别</label>
		<div class="layui-input-inline">
			<input type="text" id="L_sex" name="sex" class="layui-input">
		</div>
	</div>

	<div class="layui-form-item">
		<label for="L_email" class="layui-form-label">
             <span class="we-red">*</span>手机
        </label>
		<div class="layui-input-inline">
			<input type="text" id="L_phone" name="phone" class="layui-input">
		</div>
	</div>
	<div class="layui-form-item">
		<label for="L_email" class="layui-form-label">
                <span class="we-red">*</span>邮箱
            </label>
		<div class="layui-input-inline">
			<input type="text" id="L_email" name="email" lass="layui-input">
		</div>
	</div>
	<div class="layui-form-item">
		<label for="L_pass" class="layui-form-label">
          <span class="we-red">*</span>密码
      </label>
		<div class="layui-input-inline">
			<input type="password" id="L_pass" name="pass" class="layui-input">
		</div>
	</div>
</form>
```

### 增加JavaScript代码

``` javascript
//父页面创建addMember()函数
function addMember(){
	var _len = $('#memberList tr').length;
	alert(_len);
	$('#memberList').append(						
		'<tr data-id="' + _len + '">' +
			'<td>'+
				'<div class="layui-unselect layui-form-checkbox" lay-skin="primary" data-id="5"><i class="layui-icon">&#xe605;</i></div>'+
			'</td>'+
			'<td>5</td>'+
			'<td>'+$('input[name="username"]').val()+'</td>'+
			'<td>'+$('input[name="sex"]').val()+'</td>'+
			'<td>'+$('input[name="phone"]').val()+'</td>'+
			'<td>'+$('input[name="email"]').val()+'</td>'+
			'<td>北京市西城区</td>'+
			'<td>2018-01-01 11:11:42</td>'+
			'<td class="td-status"><span class="layui-btn layui-btn-normal layui-btn-xs">已启用</span></td>'+
			'<td class="td-manage">'+
				'<a onclick="member_stop(this,\'10001\')" href="javascript:;" title="启用"><i class="layui-icon">&#xe601;</i></a>'+
				'<a title="编辑" onclick="WeAdminShow(\'编辑\',\'./edit.html\',600,400)" href="javascript:;"><i class="layui-icon">&#xe642;</i></a>'+
				'<a onclick="WeAdminShow(\'修改密码\',\'./password.html\',600,400)" title="修改密码" href="javascript:;"><i class="layui-icon">&#xe631;</i></a>'+
				'<a title="删除" onclick="member_del(this,\'要删除的id\')" href="javascript:;"><i class="layui-icon">&#xe640;</i></a>'+
			'</td>'+
		'</tr>'	
	);
}

//frame子页面改写点击监听事件
//监听提交
form.on('submit(add)', function(data) {
	console.log(data.field);
	var f = data.field;
	//发异步，把数据提交给php
	layer.alert("增加成功", {
		icon: 6
	}, function() {
		// 获得frame索引
		var index = parent.layer.getFrameIndex(window.name);
		//将form提交的值传递给父页面的隐藏表单对应的input值
		parent.$('input[name="username"]').val(f.username);
		parent.$('input[name="sex"]').val(f.sex);
		parent.$('input[name="email"]').val(f.email);
		parent.$('input[name="phone"]').val(f.phone);
		parent.$('input[name="pass"]').val(f.pass);
		//然后执行父页面的addMember()函数
		parent.addMember();
		//关闭当前frame
		parent.layer.close(index);
	});
	return false;
});
```