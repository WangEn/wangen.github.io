---
title: jquery无刷新添加和删除input输入框 增加减少input框
date: 2017-12-25 22:48:11
tags: 
    - jQuery
    - form
    - input
categories: form
---
### 演示说明
![form-input-1](http://www.freejs.net/demo/278/demo.jpg)
[点击查看演示](http://www.freejs.net/demo/278/index.html)

### 主要实现代码
HTML代码部分
主要展示一个input输入框，一个点击事件的按钮；删除时判断至少存在一个元素，增加时可以限制最大允许生成的input个数
``` bash
<p><span><a href="#" id="AddMoreFileBox" class="btn btn-info">添加更多的input输入框</a></span></p>  
<div id="InputsWrapper">  
    <div>
        <input type="text" name="mytext[]" id="field_1" value="Text 1"/>
        <a href="#" class="removeclass">×</a>
    </div>  
</div>  
```
JavaScript代码部分
首先引入jQuery文件，调用代码如下：
``` bash
<script>  
$(document).ready(function() {  
  
    var MaxInputs       = 8; //maximum input boxes allowed  
    var InputsWrapper   = $("#InputsWrapper"); //Input boxes wrapper ID  
    var AddButton       = $("#AddMoreFileBox"); //Add button ID  
  
    var x = InputsWrapper.length; //initlal text box count  
    var FieldCount=1; //to keep track of text box added  
  
    $(AddButton).click(function (e)  //on add input button click  
    {  
        if(x <= MaxInputs) //max input box allowed  
        {  
            FieldCount++; //text box added increment  
            //add input box  
            $(InputsWrapper).append('<div><input type="text" name="mytext['+FieldCount+']" id="field_'+ FieldCount +'" value="Text '+ FieldCount +'"/><a href="#" class="removeclass">×</a></div>');  
            x++; //text box increment  
        }  
        return false;  
    });  
  
    $("body").on("click",".removeclass", function(e){ //user click on remove text  
        if( x > 1 ) {  
                $(this).parent('div').remove(); //remove text box  
                x--; //decrement textbox  
        }  
        return false;  
    })   
  
});  
</script> 
```

### 写在最后

> 原文地址:http://www.freejs.net/article_biaodan_278.html