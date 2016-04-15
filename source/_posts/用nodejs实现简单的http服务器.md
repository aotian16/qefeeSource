title: 用nodejs实现简单的http服务器

date: 2016-01-07 09:05:15

tags: nodejs

categories: nodejs

---

<!--head-->

教你一步一步用nodejs实现一个文件照片上传服务器.

需要熟悉的前提技术:

1. javascript
2. http request response



[TOC]

<!--more-->

<!--body-->

# 实现代码

## 第一版

``` javascript
var http = require("http");

var server = http.createServer();
server.listen(8888);
```

`node <文件名>.js`来运行,**下同**

服务器监听8888端口, 其他什么也不做

你可以用<http://localhost:8888>访问,**下同**

## 第二版

我们添加一些注释, 并响应用户的请求, 打印`Hello World`到屏幕上.

此外在后台打印一些log.

``` javascript
// 加载http模块
var http = require("http");

// 回调函数
function onRequest(request, response) {

    console.log('request received.');

    response.writeHead(200, {"Content-Type": "text/plain"});
    response.write("Hello World");
    response.end();
}

// 创建服务
var server = http.createServer(onRequest);

// 监听端口
var port = 8888;
// 开始监听
server.listen(port);

console.log('server started. httpModule://localhost:'+port);
```

## 第三版

我们要把server提取为一个模块, 方便组织.

现在我们有2个文件了. app是主文件, BaseServer是我们的服务器模块.

导出模块关键字是`exports`

**BaseServer.js**

``` javascript
// 加载http模块
var http = require("http");

// 定义导出模块
function startServer(port) {
    // 回调函数
    function onRequest(request, response) {

        console.log('request received.');

        response.writeHead(200, {"Content-Type": "text/plain"});
        response.write("Hello World");
        response.end();
    }

    // 创建服务
    var server = http.createServer(onRequest);

    // 开始监听
    server.listen(port);

    console.log('server started. httpModule://localhost:'+port)
}

// 导出模块
exports.startServer = startServer;
```

**app.js**

``` javascript
// 加载自定义的BaseServer.js模块
var baseServerModule = require('./BaseServer')

// 监听端口
var port = 8888;
baseServerModule.startServer(port)
```



## 第四版

深入理解下request, 我们打印request的消息看下.

**BaseServer.js**

``` javascript
// 加载http模块
var httpModule = require('http');
var urlModule = require('url');

// 定义导出模块
function startServer(port) {

    // 打印请求信息
    function printRequestInfo(request) {
        console.log('收到请求, 请求信息为:');
        console.log('  method         = ' + request.method);
        console.log('  url            = ' + request.url);
        console.log('  headers        = ' + request.headers);
        console.log('  trailers       = ' + request.trailers);
        console.log('  httpVersion    = ' + request.httpVersion);
        console.log('  connection     = ' + request.connection);

        var url = urlModule.parse(request.url);

        console.log('url详细信息 = ' + url);
        console.log('  protocol = ' + url.protocol);
        console.log('  slashes  = ' + url.slashes);
        console.log('  auth     = ' + url.auth);
        console.log('  host     = ' + url.host);
        console.log('  port     = ' + url.port);
        console.log('  hostname = ' + url.hostname);
        console.log('  hash     = ' + url.hash);
        console.log('  search   = ' + url.search);
        console.log('  query    = ' + url.query);
        console.log('  pathname = ' + url.pathname);
        console.log('  path     = ' + url.path);
        console.log('  href     = ' + url.href);

        console.log('headers详细信息:');
        for (var header in request.headers) {
            console.log('  ' + header + '     = ' + request.headers[header]);
        }
    }

    // 回调函数
    function onRequest(request, response) {
        printRequestInfo(request);

        response.writeHead(200, {"Content-Type": 'text/plain'});
        response.write('Hello World');
        response.end();
    }

    // 创建服务
    var server = httpModule.createServer(onRequest);

    // 开始监听
    server.listen(port);

    console.log('server started. http://localhost:'+port)
}

// 导出模块
exports.startServer = startServer;
```

**app.js**

``` javascript
// 加载自定义的BaseServer.js模块
var baseServerModule = require('./BaseServer')

// 监听端口
var port = 8888;
baseServerModule.startServer(port)
```

这次我们用<http://localhost:8888/path/to/somewhere/?key=value&k=v>访问,便于查看.



我们可以看到输出为

<pre>

/usr/local/bin/node app.js

server started. http://localhost:8888

收到请求, 请求信息为:

  method         = GET

  url            = /path/to/somewhere/?key=value&k=v

  headers        = [object Object]

  trailers       = [object Object]

  httpVersion    = 1.1

  connection     = [object Object]

url详细信息 = [object Object]

  protocol = null

  slashes  = null

  auth     = null

  host     = null

  port     = null

  hostname = null

  hash     = null

  search   = ?key=value&k=v

  query    = key=value&k=v

  pathname = /path/to/somewhere/

  path     = /path/to/somewhere/?key=value&k=v

  href     = /path/to/somewhere/?key=value&k=v

headers详细信息:

  host     = localhost:8888

  connection     = keep-alive

  cache-control     = max-age=0

  accept     = text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,*/*;q=0.8

  upgrade-insecure-requests     = 1

  user-agent     = Mozilla/5.0 (Macintosh; Intel Mac OS X 10_11_2) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/47.0.2526.106 Safari/537.36

  accept-encoding     = gzip, deflate, sdch

  accept-language     = zh-CN,zh;q=0.8,ja;q=0.6

</pre>

## 第五版

熟悉了request后,我们要给服务器加上路由功能

# 参考文章

[Node入门](http://www.nodebeginner.org/index-zh-cn.html)