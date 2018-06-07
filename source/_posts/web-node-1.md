---
title: nodejs遇到的问题
date: 2018-02-25 23:27:38
tags: 
    - node
categories: Node
---

## node插件

### 1、nodemon 

> nodemon [https://github.com/remy/nodemon](https://github.com/remy/nodemon)

nodemon库是专门调试时候使用的，它会自动检测 node.js 代码的改动，然后帮你自动重启应用。在调试时可以完全用 nodemon 命令代替 node 命令。
> nodemon安装：`$ npm i -g nodemon`
> nodemon使用：`$ nodemon app.js` 替代node启动我们的应用

## node一些问题备忘


## 常见错误

转载自huansky的文章[nodejs遇到的问题](https://www.cnblogs.com/huansky/p/5524743.html)，在遇到问题6时找到此解决方法。

### 1、express-session deprecated undefined <font color=#ff0000 >**resave option**</font>; provide resave option app.js:49:9

express-session deprecated undefined <font color=#ff0000 >*saveUninitialized option*</font>; provide saveUninitialized option app.js:49:9
解答：其实我们可以看到斜体的部分就是导致错误的原因
解决办法：
``` javascript
app.use(session({
    secret: 'keyboard cat', 
    cookie: { maxAge: 60000 }, 
    resave: true, 
    saveUninitialized: true 
}))
```
具体参看[https://github.com/expressjs/session#options](https://github.com/expressjs/session#options)

### 2、failed to load c++ bson extension using pure js version

在package.json修改  “connect-mongo”:“0.8.2” 运行npm install安装模块,打开app.js，添加以下代码：

``` javascript
app.use(session({
  secret: settings.cookieSecret,
  key: settings.db,//cookie name
  cookie: {maxAge: 1000 * 60 * 60 * 24 * 30},//30 days
  store: new MongoStore({
    db: settings.db,
    host: settings.host,
    port: settings.port,
    url: 'mongodb://localhost/myblog'
  }),
  proxy: true,
  resave: true,
  saveUninitialized: true
```

### 3、怎么进入mongodb的shell

打开一个命令行切换到 `mongodb/bin/` （保证数据库已打开的前提下），输入：
``` bash
>mongo
>show dbs
>use xxxx;   (此处为你要选择的数据库)
```

### 4、在看nodejs权威指南的时候，我发现到了第7章，发现运行telnet localhost   8431提示说这是外部命令；

解决办法：依次打开“开始”→“控制面板”→“打开或关闭Windows功能”，在打开的窗口处，寻找并勾选“Telnet客户端”，然后点击“确定”。顺利安装后，再在运行下输入此命令就OK了。

> 1.开始–>控制面板–>程序和功能
> 2.左侧–>打开或者关闭windows功能
> 3.找到Telnet客户端，选择安装

### 5、安装express后，此时会出现can not find express，这是因为路径没有配置的原因。
 
解决办法：
> 计算机-->属性-->高级系统设置
高级-->环境变量
添加NODE_PATH并设置路径为C:\Users\YOUR_USER_NAME\AppData\Roaming\npm\node_modules
将%NODE_PATH%添加到当前用户环境变量path中

### 6、mocha init 出现错误  missing required argument path

文件夹 [^footnote] :lesson7->vendor,在vendor文件夹里运行mocha init出错，
解决办法：首先回到上一层文件夹lesson7；然后输入：`mocha init vendor`

### 7、npm install --save 与 npm install --save-dev

一个放在package.json 的dependencies,一个放在devDependencies里面;
产品模式用dependencies，开发模式用devDep。test相关npm该放到dev里面。


[^footnote]: [《Node.js 包教不包会》 -- by alsotang](https://github.com/alsotang/node-lessons/tree/master/lesson7) 。