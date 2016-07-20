title: mac添加android的adb等工具到环境变量
categories: android
tags: android
date: 2016-07-20 14:46:45

---

<!--head-->

1，打开`.bash_profile`文件

```shell
cd ~
vim .bash_profile
```

2，添加环境变量

添加下面两行到`PATH`中去

```shell
export PATH=$PATH:/Users/你的用户名/Library/Android/sdk/platform-tools
export PATH=$PATH:/Users/你的用户名/Library/Android/sdk/tools
```

保存退出

3，更新环境变量

```shell
source .bash_profile
```

4，验证

```shell
adb devices
```

显示设备列表就是成功了

**参考文章**

[在Mac pro上如何配置adb命令？](http://jingyan.baidu.com/article/59703552c0f8818fc1074041.html)

<!--more-->

<!--body-->
