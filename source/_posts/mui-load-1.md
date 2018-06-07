---
title: js依赖zepto的滚动加载方法
date: 2018-06-07 14:11:03
tags: 
	- mui
	- zepto
	- 加载
categories: mui
---

> 依赖zepto的滚动加载方法

``` javascript
/*
 * 监听滚动加载更多
 * 暂定依赖zepto.min.js
 * 传入参数pageStart(开始条数),pageSize(每次加载条数),url(api),type(GET/POST),container(主体id名),val(输出样式),isFirst(打开页面时首次是否加载)
 */

function scrollMore(pageStart, pageSize, url, type, container, val, isFirst) {
	/*滚动加载初始化*/
	var counter = 0; /*计数器*/
	var isEnd = false; /*结束标志*/
	var isAjax = false; /*防止滚动过快，服务端没来得及响应造成多次请求*/

	console.log("isFirst:" + isFirst + "--val:" + val);
	/*首次加载*/
	if(isFirst) {
		getData(pageStart, pageSize);
		counter = 1;
		//console.log(1111 + "counter:" + counter);
	}

	/*监听加载更多*/
	$(window).scroll(function() {
		//console.log(isEnd, isAjax);
		//console.log(pageStart,pageSize);
		/*滚动加载时如果已经没有更多的数据了、正在发生请求时，不能继续进行*/
		if(isEnd == true || isAjax == true) {
			return;
		}

		// 当滚动到最底部以上20像素时， 加载新内容
		if($(document).height() - $(this).scrollTop() - $(this).height() < 20) {
			console.log("counter:" + counter);
			if(counter) {
				pageStart += pageSize;
			}
			//pageStart += counter * pageSize;
			counter++;
			getData(pageStart, pageSize);
		}
	});

	//加载更多的请求
	function getData(offset, size) {
		isAjax = true;
		mui.ajax({
			type: type,
			url: url,
			dataType: 'json',
			success: function(res) {
				isAjax = false;
				var data = res.data
					,sum = res.data.length
					,result = '';

				/************业务逻辑块：实现拼接html内容并append到页面*****************/
				//console.log(offset , size, sum);
				/*如果剩下的记录数不够分页，就让分页数取剩下的记录数
				 * 例如分页数是5，只剩2条，则只取2条
				 */

				if(sum - offset < size) {
					size = sum - offset;
				}

				/*使用for循环输出拼接*/
				for(var i = offset; i < (offset + size); i++) {	
					var innerHtml;
					switch(val) {
						case 1:
							var book = data[i].book
								,serial = book.serial ? "连载中" : "已完结";
							innerHtml = '<div class="shu" data-id="'+book.id+'"><div class="imgWrap">' +
								'<img src="' + book.cover + '" alt=""></div><div class="words"><p class="bookname">' + book.name + '</p>' +
								'<p class="introduce"><span>[' + serial + ']：' + book.intro + '</span></p><p class="author">' + book.author + '</p>' +
								'</div></div>';
							break;
						case 2:
							var book = data[i].book
								,index = i + 1;
							innerHtml = '<li class="mui-table-view-cell" data-id="' + book.id + '">' +
								'<div class="mui-table bookContent" >'+
								'<div class="mui-table-cell bookimg">' +
								'<img class="img" src="' + book.cover + '" alt="">' +
								'</div>' +
								'<div class="mui-table-cell booklist">' +
								'<p class="bookname">' + book.name + '<span>' + index + '</span></p>' +
								'<p class="bIntroduce mui-ellipsis-3">' + book.intro + '</p>' +
								'<p class="bookauthor"><span class="book_author">' + book.author + '</span><span class="type">' + book.categoryname + '</span></p>' +
								'</div>' +
								'</div>' +
								'</li>';
							break;							
						case 3:	
							var item = data.chapter[i]
								,viptext = item.isvip ? '<span class="mui-badge mui-badge-danger">VIP</span>' : '<span class="mui-badge">免费</span>';
							innerHtml = '<li class="mui-table-view-cell" onclick="goDetail('+item.id+','+item.isvip+')">'+item.name+viptext+'</li>';
							break;
						default:
							break;
					}
					result += innerHtml;
				}
				$(container).append(result);

				/*******************************************/

				if((offset + size) >= sum) {
					isEnd = true;
					//mui.toast("没有更多了");
					var tips = '<div class="mui-pull-bottom-pocket mui-block mui-visibility">' +
						'<div class="mui-pull" style="font-weight:normal;color:#aaa">' +
						'<div class="mui-pull-loading mui-icon mui-spinner mui-hidden"></div>' +
						'<div class="mui-pull-caption mui-pull-caption-nomore">暂时没有更多数据啦</div>' +
						'</div></div>';
					$(container).append(tips);
					//提示没有了
				}
			},
			error: function(xhr, type) {
				alert('Ajax error!');
			}
		});
	}
}
```




