---
title: Dedecms织梦会员邮件验证与自定义表单发送邮件-官方类版-支持QQ邮箱163邮箱
date: 2017-12-17 00:11:03
toc: true
tags: 
    - DedeCMS
    - 邮件
categories: DedeCMS
---
#### 会员邮件验证效果

![member-email](http://www.dedediy.com/uploads/allimg/QQimg20171031201926.png)
#### 自定义表单发送邮件效果

![form-email](http://www.dedediy.com/uploads/allimg/QQimg20171031201550.png)

## 设置步骤如下：
#### 1、QQ邮箱 或者 163邮箱 开启SMTP服务，拿到授权码

![email-shouquanma](http://www.dedediy.com/uploads/allimg/QQimg20171031202215.png)

#### 2、网站后台 - 系统 - 系统基本参数 - 核心设置

> 网站发信EMAIL：wuruhua@163.com 或者 65602960@qq.com (本文邮箱均为示例，请按需求填写自己的邮箱)

> 是否启用smtp方式发送邮件：是

> smtp服务器：ssl://smtp.163.com 或者 ssl://smtp.qq.com

> smtp服务器端口：465

> SMTP服务器的用户邮箱：wuruhua@163.com 或者 65602960@qq.com

> SMTP服务器的用户帐号：wuruhua 或者 65602960

> SMTP服务器的用户密码：填你邮箱授权码，不是邮箱登录密码

如图

> 配置QQ邮箱的是这样
![email-qq-set](http://www.dedediy.com/uploads/allimg/QQimg20171031195906.png)

> 配置163邮箱的是这样
![email-163-set](http://www.dedediy.com/uploads/allimg/QQimg20171031195754.png)

> 至此会员邮件验证配置完成，会员就能收到验证邮件了；自定义表单发送邮件的小伙伴继续往下看

#### 3、网站后台 - 系统 - 系统基本参数 - 添加新变量

> 变量名称：cfg_shoujianren
> 变量类型：文本
> 参数说明：收件人
> 变量值：发送邮件通知者，如65602960@qq.com
>所属组：站点设置

如图
![system-set](http://www.dedediy.com/uploads/allimg/QQimg20171031203350.png)

#### 4、打开 /plus/diy.php 找到

``` bash
$id = $dsql->GetLastID();
```

在它的下面加入

``` bash
$mailtitle = "{$diy->name}--留言通知";
$mailbody = '';
foreach($diy->getFieldList() as $field=>$fieldvalue)
{
	$mailbody .= "{$fieldvalue[0]}:{${$field}}\r\n";
}
$headers = "From: ".$cfg_adminemail."\r\nReply-To: ".$cfg_adminemail;
if($cfg_sendmail_bysmtp == 'Y' && !empty($cfg_smtp_server))
{
	$mailtype = 'TXT';
	require_once(DEDEINC.'/mail.class.php');
	$smtp = new smtp($cfg_smtp_server,$cfg_smtp_port,true,$cfg_smtp_usermail,$cfg_smtp_password);
	$smtp->debug = false;
	$smtp->sendmail($cfg_shoujianren,$cfg_webname ,$cfg_smtp_usermail, $mailtitle, $mailbody, $mailtype);
}
else
{
	@mail($cfg_shoujianren, $mailtitle, $mailbody, $headers);
}
```

至此自定义表单发送邮件通知管理者设置完成

本文来源[www.dedediy.com](http://www.dedediy.com/luojishuju/175.html) ，仅作为备份参考使用