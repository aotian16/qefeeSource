title: cordova开发ios时候getScript函数无效的解决办法
categories: cordova
tags: cordova
date: 2016-02-26 23:33:30

---

<!--head-->

在开发`cordova`程序的时候遇到一个需求, 就是想要利用`jquery`的`getScript`动态加载`JavaScript`.

**代码如下:**

```javascript
$.getScript(root + "/js/other.js", function(response, status) {
    console.log(response, status);
});
```

问题是, 在浏览器环境browser中运行可以的, 在ios中却报错(android没有试过),找不到文件.

既然找不到文件, 就找到我们要的路径也就可以了.

可以通过`window.location.href`来获取.

* 在browser中显示"http://localhost:8000/index.html",


* 在iOS显示"xxxx(记不住了, 反正不重要)/index.html".

可以看出来得到的是包括index.html的路径.

我们需要的是父目录, 因为js文件在另外一个文件夹中.

**代码如下:**

```javascript
var href = window.location.href;
var root = href.substr(0, href.length - 11); // 这里除去了'/index.html'

$.getScript(root + "/js/other.js", function(response, status) {
    console.log(response, status);
});
```

**注意事项:**

android环境没有尝试, 请自行测试.

<!--more-->



<!--body-->