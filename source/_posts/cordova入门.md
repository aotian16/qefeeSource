title: cordova入门

categories: cordova

tags: cordova

date: 2016-02-17 09:43:48

---

<!--head-->

主要来自官网, 后面用一个简单的`DeviceInfo`项目来走一个完整流程.

自己也是菜鸟, 刚刚开始学习, 有问题的地方请指教.

<!--more-->

<!--body-->

[TOC]

# refer相关

* [cordova官网](http://cordova.apache.org/)

# install安装

需要先安装[Node.js](https://nodejs.org/en/), 而且`npm`命令可用.

`npm install -g cordova`

# get start第一个工程

##### 第一步. 创建工程

格式 ====> `cordova create <path>`

`cordova create hellocordova`

创建了一个叫做`hellocordova`的工程

##### 第二步. 添加平台

格式 ====> `cordova platform add <platform name>`

`cordova platform add browser`

添加`browser`浏览器平台

##### 第三部. 运行工程

格式 ====> `cordova run <platform name>`

`cordova run browser`

运行`browser`浏览器平台

# command命令

命令行输入`cordova`会输出以下信息

``` 
Synopsis(概要)

    cordova command [options]

Global Commands(全局命令)

    create ............................. Create a project(创建工程)
    help ............................... Get help for a command(帮助命令)

Project Commands(工程命令)

    info ............................... Generate project information(工程信息概要)
    requirements ....................... Checks and print out all the requirements
                                            for platforms specified(查看平台依赖)

    platform ........................... Manage project platforms(管理平台)
    plugin ............................. Manage project plugins(管理插件)

    prepare ............................ Copy files into platform(s) for building(复制文件准备编译)
    compile ............................ Build platform(s)(编译平台)
    clean .............................. Cleanup project from build artifacts(清理)

    run ................................ Run project(运行工程)
                                            (including prepare && compile)
    serve .............................. Run project with a local webserver(通过web服务器来运行工程)
                                            (including prepare)

aliases(别名):
    build -> cordova prepare && cordova compile(合并了俩个命令)
    emulate -> cordova run --emulator(运行模拟器)

Command-line Flags/Options(命令行选项)

    -v, --version ...................... prints out this utility's version(版本号)
    -d, --verbose ...................... debug mode produces verbose log output for all activity(调试信息),
                                         including output of sub-commands cordova invokes
    --no-update-notifier ............... disables check for CLI updates(不检查cli更新)
    --nohooks .......................... suppress executing hooks
                                            (taking RegExp hook patterns as parameters)
```

# 一个实用的项目

上面的那个start的项目没什么用, 下面做个有点用的. 走一个完整点的流程.

##### 一. create创建

```
cordova create DeviceInfo
```

##### 二. platform添加平台

不用全部添加, 可以添加自己需要的.

1. iOS 苹果
2. android 安卓
3. browser 浏览器

``` 
cordova platform add ios
cordova platform add android
cordova platform add browser
```

##### 三. requirements查看依赖

``` 
cordova requirements
```

如果有什么依赖没有安装, 先装好.

##### 四. plugin添加插件

这个项目我们要用到`cordoba-plugin-device`这个插件.

``` 
cordova plugin add cordova-plugin-device
```

我在mac电脑上安装安卓平台时候遇到了以下问题. 这里记录下解决办法.

**问题如下**. 貌似https什么的导致的.

``` 
* What went wrong:
A problem occurred configuring root project 'android'.
> Could not resolve all dependencies for configuration ':classpath'.
   > Could not resolve com.android.tools.build:gradle:1.5.0.
     Required by:
         :android:unspecified
      > Could not GET 'https://repo1.maven.org/maven2/com/android/tools/build/gradle/1.5.0/gradle-1.5.0.pom'.
         > peer not authenticated
```

**解决办法**如下.

修改`grade`的`mavenCentral`修改为http的.

**需要修改的文件:**

1. <projecthome>/platforms/build.gradle
2. <projecthome>/platforms/CordovaLib/build.gradle

具体修改为.

**修改前:**

``` 
mavenCentral()
```

**修改后:**

``` 
//mavenCentral()
maven {
    url "http://repo1.maven.org/maven2"
}
```

**解决问题参考的链接:**

1. [Gradle peer not authenticated](http://blog.csdn.net/lonewolf521125/article/details/44851219)
2. [Gradle can't connect to maven repo through corporate proxy - need to configure through Sencha/Cordova](http://stackoverflow.com/questions/30219065/gradle-cant-connect-to-maven-repo-through-corporate-proxy-need-to-configure-t)

##### 五. modify修改

需要修改的地方不多. 就一个文件`index.js`.

修改`receivedEvent`函数的内容为以下内容.

``` javascript
        var parentElement = document.getElementById(id);
        var listeningElement = parentElement.querySelector('.listening');
        var receivedElement = parentElement.querySelector('.received');

        listeningElement.setAttribute('style', 'display:none;');
        receivedElement.setAttribute('style', 'display:block;');

        var deviceInfo = '';

        deviceInfo += "<p>device.cordova = " + device.cordova + "</p>";
        deviceInfo += "<p>device.model = " + device.model + "</p>";
        deviceInfo += "<p>device.platform = " + device.platform + "</p>";
        deviceInfo += "<p>device.uuid = " + device.uuid + "</p>";
        deviceInfo += "<p>device.version = " + device.version + "</p>";
        deviceInfo += "<p>device.manufacturer = " + device.manufacturer + "</p>";
        deviceInfo += "<p>device.isVirtual = " + device.isVirtual + "</p>";
        deviceInfo += "<p>device.serial = " + device.serial + "</p>";

        receivedElement.innerHTML = deviceInfo;

        console.log('Received Event: ' + id);
```

##### 六. run运行

**browser**

``` 
cordova run browser
```

**ios**

``` 
cordova run ios
```

**android**

``` 
cordova run android
```

