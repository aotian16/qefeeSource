title: nodejs自动签到程序

date: 2016-01-07 17:41:59

categories: nodejs

tags: nodejs

---

<!--head-->

用`nodejs`做了个自动签到程序, 还是挺有成就感的.

[TOC]

## 程序功能

1. 模拟登录和签到
2. 发送邮件
3. 简单加密

<!--more-->



<!--body-->

## 程序实现

### 前期调研

先用`chrome`浏览器找到登录和签到的url.

方法请看参考文章.

### 用到的模块

1. superagent 模拟登录模块
2. nodemailer 邮件模块
3. crypto 加密模块

### 代码

`index.js`

``` javascript
var requestModule = require('superagent');
var crypto = require('crypto');
var nodemailer = require('nodemailer');

var transporter = nodemailer.createTransport({
    host: "smtp.exmail.qq.com", // 主机
    secureConnection: true, // 使用 SSL
    //service: 'Gmail',
    auth: {
        user: 'xxx@xxx.com',
        pass: 'xxxx'
    }
});


function sendMain(to, msg) {
    var mailOptions = {
        from: 'xxx@xxx.com', // sender address
        to: to, // list of receivers
        subject: '某某网站签到消息', // Subject line
        text: msg, // plaintext body
        html: '<b>' + msg + '</b>' // html body
    };

    transporter.sendMail(mailOptions, function(error, info){
        if(error){
            console.log(error);
        }else{
            console.log('Message sent: ' + info.response);
        }
    });
}

var headers = {
    Accept: '*/*',
    'Accept-Encoding': 'gzip, deflate, sdch',
    'Accept-Language': 'zh-CN,zh;q=0.8,ja;q=0.6',
    Connection: 'keep-alive',
    Host: 'www.某某网址.com',
    Referer: 'http://www.某某网址.com/login',
    'User-Agent': 'Mozilla/5.0 (Macintosh; Intel Mac OS X 10_11_2) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/47.0.2526.106 Safari/537.36'
};

var headers1 = {
    Accept: 'text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,*/*;q=0.8',
    'Accept-Encoding': 'gzip, deflate, sdch',
    'Accept-Language': 'zh-CN,zh;q=0.8,ja;q=0.6',
    'Cache-Control': 'max-age=0',
    Connection: 'keep-alive',
    Host: 'www.某某网址.com',
    Referer: 'http://www.某某网址.com/usercenter/summary',
    'Upgrade-Insecure-Requests': 1,
    'User-Agent': 'Mozilla/5.0 (Macintosh; Intel Mac OS X 10_11_2) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/47.0.2526.106 Safari/537.36'
}

var origin = 'http://www.某某网址.com';
var loginUrl = origin + '/ajax/login';

var users = [
    {
        name: '某某人1',
        username: '手机号1',
        password: '密码1',
        mail: '邮件1'
    },
    {
        name: '某某人2',
        username: '手机号2',
        password: '密码2',
        mail: '邮件2'
    }
]

function requestSignin(json, user) {
    var cookie = ''
    var ECS_NID = json.msg_data.ECS_NID
    cookie += '; ECS_NID=' + ECS_NID

    var signinUrlStr = origin + '/usercenter/signin/perform'

    requestModule.post(signinUrlStr).set(headers1).set('Cookie', cookie).type('JSON').end(function (error, result) {
        if (error) {
            var info = '出错啦,可能网络有问题.' + error
            console.log(info)
        } else {
            var text = result.text
            var json = JSON.parse(text)
            if (json.code == 1) {
                var info = user.name + '          签到成功'
                console.log(info)
                //sendMain(user.mail, info)
            } else {
                var info = user.name + '          签到失败'
                console.log(info)
                sendMain(user.mail, info)
            }
        }
    });
}

function requestLogin(user) {
    var md5 = crypto.createHash('md5');
    md5.update(user.password);
    var params = "username=" + user.username
        + "&password=" + md5.digest('hex')
        + "&back_url=" + null
        + "&back_flag=" + 'null'
        + "&web_rember=" + 0
        + "&promote_channel=" + null + "&jsonpcallback=loginCallback";

    var url = loginUrl + '/?' + params;
    requestModule.get(url).set(headers).redirects(0).end(function (error, result) {
        if (error) {
            var info = '出错啦,可能网络有问题.' + error
            console.log(info)
            sendMain(user.to, info)
        } else {
            var text = result.text // jsonp格式
            var length = text.length
            var jsonStr = text.substring(14, length - 1) // 截取json

            var json = JSON.parse(jsonStr)
            if (json.code == 1) {
                requestSignin(json, user);
            } else {
                var info = '出错啦,返回的数据格式有问题.'
                console.log(info)
                sendMain(user.to, info)
            }
        }
    });
}


for(var i in users) {
    requestLogin(users[i])
}

console.log('日志日期为: ' + new Date())
```

`package.son`

``` json
{
  "name": "register_某某网址",
  "version": "0.0.1",
  "description": "Register 某某网址",
  "main": "index.js",
  "scripts": {
    "test": "test"
  },
  "keywords": [
    "key"
  ],
  "author": "aotian16",
  "license": "MIT",
    "dependencies": {
        "superagent": "latest",
        "nodemailer": "latest"
    }
}
```

## 参考文章

[node.js实现模拟登录，自动签到领流量。](https://cnodejs.org/topic/54e96cf7ddce2d471403203f)
