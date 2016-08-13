title: idea导出可执行jar包
categories: tools
tags: idea
date: 2016-08-13 10:49:38

---

<!--head-->

以下以hello world程序为例，图示打包过程。

[TOC]

<!--more-->

<!--body-->

## Artifacts Config

File -> Project Structure -> Artifacts -> + -> From modules ...

![img](http://qefee.qiniudn.com/20160813_idea%E5%AF%BC%E5%87%BA%E5%8F%AF%E6%89%A7%E8%A1%8Cjar%E5%8C%85_1.png)

![img](http://qefee.qiniudn.com/20160813_idea%E5%AF%BC%E5%87%BA%E5%8F%AF%E6%89%A7%E8%A1%8Cjar%E5%8C%85_2.png)

## Build Artifacts

Build -> Build Artifacts

![img](http://qefee.qiniudn.com/20160813_idea%E5%AF%BC%E5%87%BA%E5%8F%AF%E6%89%A7%E8%A1%8Cjar%E5%8C%85_3.png)

![img](http://qefee.qiniudn.com/20160813_idea%E5%AF%BC%E5%87%BA%E5%8F%AF%E6%89%A7%E8%A1%8Cjar%E5%8C%85_4.png)

## 注意点

如果工程结构不对应，会导致找不到主文件，需要手动调整路径。

比如maven工程。

<u>*下面两个图片来源于网络*</u>

**修改前**

![img](http://i.stack.imgur.com/HFSeo.png)

**修改后**

![img](http://i.stack.imgur.com/K2l34.png)

## reference

* [Create an excutable jar in IntelliJ IDEA](http://stackoverflow.com/questions/17024715/create-an-excutable-jar-in-intellij-idea)

* [IDEA如何打包可运行jar的一个问题。](http://bglmmz.iteye.com/blog/2058785)
* [Packaging a Module into a JAR File](https://www.jetbrains.com/help/idea/2016.1/packaging-a-module-into-a-jar-file.html)