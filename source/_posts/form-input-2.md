---
title: 结合layui实现无刷新添加和删除input输入框，自动生成不同name值
date: 2017-12-26 23:47:09
tags: 
    - layui
    - form
    - input
categories: form
---
### 前情回顾

> 随意删除添加后保存到服务端的input，再次编辑时（先调用服务端保存的值）添加新的input输入框name值可能会生成重复，提交保存时，name值重复的input元素会覆盖已有元素。

昨天借鉴[jquery无刷新添加和删除input输入框 增加减少input框](/2017/12/25/form-input-1/) 的思路实现了jQuery无刷新随意添加和删除input输入框，但是在使用layui结合ajax提交时，发现随意添加/删除后会生成重复name值的input输入框（比如name="edu_bg[5]" / name="edu_bg[6]" / name="edu_bg[5]" 三个输入框）进行编辑提交，提交后只保存了后两个值，因为layui表单中name不支持数组的形式，相同的name也只会提交最后一个，layui社区也有朋友分享出解决方法：[表单input不支持数组方式询问+解决办法](http://fly.layui.com/jie/9166/)，可以借鉴下。

### 思路整理

> 综合了一大堆搜索及意见，也不想改动框架的代码，决定采用以下方式：

1、获取已经存在的子元素input的长度及name值，将获取的name值推入一个数组arr1中；
2、定义初始值key=0,定义插入函数insertInput()，key++循环；
3、当edu_bg[key]不在数组arr1中时，创建元素并append()推入父元素内完成，否则，重新执行insertInput()函数。

### 代码实现
#### HTML代码

主要是定义几个已有的包含input的子元素（模拟从服务端获取的已有元素），然后定义一个包含点击事件的按钮元素。

``` bash
<div class="layui-form-item yf-input-del">
    <label class="layui-form-label">教育背景</label>
    <div class="layui-input-block" id="edu_bg">
        #//测试随机创建几个连续和不连续的子元素，真实场景下直接循环出你已有的子元素即可
        <div class="layui-input-inline">
            //input文本框
            <input type="text" name="edu_bg[5]" value="" autocomplete="off" placeholder="请输入" lay-verify="required" class="layui-input">
            //删除按钮
            <i class="layui-icon deleteEduBg">&#x1006;</i>
        </div>
        <div class="layui-input-inline">
            <input type="text" name="edu_bg[6]" value="" autocomplete="off" placeholder="请输入" lay-verify="required" class="layui-input">
            <i class="layui-icon deleteEduBg">&#x1006;</i>
        </div>
        <div class="layui-input-inline">
            <input type="text" name="edu_bg[9]" value="" autocomplete="off" placeholder="请输入" lay-verify="required" class="layui-input">
            <i class="layui-icon deleteEduBg">&#x1006;</i>
        </div>
    </div>
</div>
<div class="layui-form-item" id="edu_bg_add">
    <label class="layui-form-label"></label>
    <div class="layui-input-inline">
        <a href="javascript:;" class="layui-btn site-demo-active" onclick="insertInput()">
            <i class="layui-icon">&#xe654;</i> 新增1条教育背景
        </a>
    </div>
</div>
```

#### JavaScript代码

定义一个插入函数insertInput()和一个点击删除函数

``` bash
//引入layui.js
<script src="/layui/dist/layui.all.js"></script>
<script>
    var $ = layui.jquery; //引用layui内置的jQuery，也可以自己调用
    var key1=$("#edu_bg").children(".layui-input-inline").length;
    var FieldCount1=0;
    function insertInput() {
        //检索已有的input包含的name值
        var arr1=[];
        FieldCount1++;
        $("#edu_bg input[type='text']").each(function(){
            arr1.push($(this).attr('name'));
        });
        //console.log(arr1);
        //所有name加入数组arr1
        var div1 = $("<div></div>").addClass("layui-input-inline");
        //console.log(edu_bg[FieldCount1]);
        var newCount1="edu_bg["+FieldCount1+"]";
        //判断新生成的name值是否在已存在的数组中
        if($.inArray(newCount1, arr1) === -1){
            FieldCount1=FieldCount1;
            var input1 = "<input type='text' name='edu_bg["+FieldCount1 +"]' value='' autocomplete='off' placeholder='请输入' lay-verify='required' class='layui-input'>"
            var icon1 = "<i class='layui-icon deleteEduBg' onclick='deleteElement(this)'>&#x1006;</i>";
            div1.append(input1, icon1);
            $("#edu_bg").append(div1);  // 追加新元素
            key1++;
        }else{
            FieldCount1++;
            insertInput();
        }
    }
    $("body").on("click",".deleteEduBg", function(e){ //user click on remove text  
        if( key1 > 1 ) {  
            $(this).parent('div').remove(); //移除对应的父级div元素  
            key1--; //decrement textbox  
        }else{
            alert("请至少填写1条教育背景信息！");
            return false;  
        }
    }) 
</script>
```

#### 补充CSS样式

这部分没必要，自己来就行，只是为了自己测试时样式顺眼一丢丢。。

``` bash
<link rel="stylesheet" href="/layui/dist/css/layui.css">
<style type="text/css">
    /* 新增删除图标的样式 */
    .yf-input-del .layui-input-inline .layui-input {
        padding-right: 20px;
        margin-bottom: 10px;
    }
    .yf-input-del .layui-input-inline i.layui-icon {
        position: absolute;
        right: 2px;
        top: 8px;
        color: #999999;
    }
</style>
```

### 写在最后

整个实现的过程是自己摸索和反复测试的过程，代码中也还有不少需要优化的地方，也可能会在以后的使用中继续优化整改，抛砖引玉，仅供大家参考。

> layui官网文档 http://www.layui.com/doc/

> jQuery API中文文档 http://www.css88.com/jqapi-1.9/
