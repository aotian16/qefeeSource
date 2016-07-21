title: 在vps上搭建代理服务器

categories: tools

tags: proxy

date: 2016-01-13 11:06:26



---

<!--head-->

由于**你懂的**原因, 我们需要代理翻墙.

[TOC]

# 几种连接方式

|  no  | name          | info          | image                                    |
| :--: | ------------- | ------------- | ---------------------------------------- |
|  1   | direct        | 不行            | ![direct](http://qefee.qiniudn.com/20160113_%E5%9C%A8vps%E4%B8%8A%E6%90%AD%E5%BB%BA%E4%BB%A3%E7%90%86%E6%9C%8D%E5%8A%A1%E5%99%A8direct.png) |
|  2   | proxy         | 刚开始可以, 过一会就不行 | ![direct](http://qefee.qiniudn.com/20160113_%E5%9C%A8vps%E4%B8%8A%E6%90%AD%E5%BB%BA%E4%BB%A3%E7%90%86%E6%9C%8D%E5%8A%A1%E5%99%A8proxy.png) |
|  3   | encrypt proxy | 可以            | ![encrypt](http://qefee.qiniudn.com/20160113_%E5%9C%A8vps%E4%B8%8A%E6%90%AD%E5%BB%BA%E4%BB%A3%E7%90%86%E6%9C%8D%E5%8A%A1%E5%99%A8encrypt%20proxy.png) |

看图3就可以知道, 我们需要一个代理服务器, 也就是我们的vps.

客户端加密后通过代理服务器来翻墙.

<!--more-->

<!--body-->

# 广告时间

你还没有买vps的话，我这边有个**[vultr](https://www.vultr.com/)**的优惠分享地址。

你可以得要至少一个月的费用。（我也能得到优惠 😄）

1. [优惠1](http://www.vultr.com/?ref=6924251-3B) 你可以获得20美元，30天内激活，每月至少10美元（**限时**）
2. [优惠2](http://www.vultr.com/?ref=6868383) 你可以获得10美元，30天内激活，每月至少10美元

# 软件安装

安装方式各有不同, 这里就只是列个大纲. 详细请看参考文档或者百度.

## 服务端

就是我们的vps代理服务器.

1. squid
2. stunnel

## 客户端

就是需要翻墙的电脑.

1. stunnel



# 软件配置

## 服务端

### 1.squid

我没改动配置, 使用了默认配置.

### 2.stunnel

<u>*(以下内容拷贝自参考文档)*</u>

接下来，我们需要在Squid上添加一层加密。

#### 2.1.生成公钥和私钥

生成私钥（`privatekey.pem`）：

```
openssl genrsa -out privatekey.pem 2048

```

生成公钥（`publickey.pem`）：

```
openssl req -new -x509 -key privatekey.pem -out publickey.pem -days 1095

```

（需要注意的是，`Common Name`需要与服务器的IP或者主机名一致）

合并：

```
cat privatekey.pem publickey.pem >> /etc/stunnel/stunnel.pem

```

#### 2.2.修改stunnel配置

新建一个配置文件`/etc/stunnel/stunnel.conf`，输入如下内容

```
client = no
[squid]
accept = 4128
connect = 127.0.0.1:3128
cert = /etc/stunnel/stunnel.pem
```

配置中指定了stunnel所暴露的HTTPS代理端口为4128，可以修改为其他的值。



## 客户端

### 1.stunnel

修改配置文件`stunnel.conf`,

其中`stunnel.pem`就是我们在服务器上生成的那个, 需要从服务器下载到客户端.

4128是我们上面服务器指定的端口.

7777是我们开放的端口, 即用户使用的代理端口.

```
cert = /etc/stunnel/stunnel.pem
client = yes
[squid]
accept = 127.0.0.1:7777
connect = [你的服务器 IP]:4128
```



# 启动软件

先启动服务端的`squid`和`stunnel`, 然后启动客户端的`stunnel`.

没问题的话, 现在可以指定代理为`127.0.0.1:7777`加密代理上网了.

浏览器推荐Chrome的插件[Proxy SwitchyOmega](https://chrome.google.com/webstore/detail/proxy-switchyomega/padekgcemlokbadohgkifijomclgjgif?hl=en)

# 参考文档

1. [使用Squid搭建HTTPS代理服务器](http://www.predatorray.me/%E5%9C%A8VPS%E4%B8%8A%E6%90%AD%E5%BB%BASquid%E4%BB%A3%E7%90%86%E6%9C%8D%E5%8A%A1%E5%99%A8/)
2. [CentOS配置基于web认证的squid 3.1.23](http://www.centoscn.com/image-text/config/2014/1203/4231.html)
3. [How To Set Up an SSL Tunnel Using Stunnel on Ubuntu](https://www.digitalocean.com/community/tutorials/how-to-set-up-an-ssl-tunnel-using-stunnel-on-ubuntu)
