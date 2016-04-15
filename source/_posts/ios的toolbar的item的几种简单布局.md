title: ios的toolbar的item的几种简单布局
categories: ios
tags: toolbar
date: 2016-03-10 16:23:54

---

<!--head-->

From: [ios的toolbar的item的几种简单布局](http://qefee.com/2016/03/10/ios%E7%9A%84toolbar%E7%9A%84item%E7%9A%84%E5%87%A0%E7%A7%8D%E7%AE%80%E5%8D%95%E5%B8%83%E5%B1%80/?from=csdn)

简单介绍toolbar的几种布局方法

1. 居中
2. 固定长度
3. 分隔在两端



<!--more-->



<!--body-->

**运行效果**

![run](http://qefee.qiniudn.com/20160310_ios%E7%9A%84toolbar%E7%9A%84item%E7%9A%84%E5%87%A0%E7%A7%8D%E7%AE%80%E5%8D%95%E5%B8%83%E5%B1%80_run.png)

**设计页面**

![design](http://qefee.qiniudn.com/20160310_ios%E7%9A%84toolbar%E7%9A%84item%E7%9A%84%E5%87%A0%E7%A7%8D%E7%AE%80%E5%8D%95%E5%B8%83%E5%B1%80_design.png)



### 一. 居中

第一个toolbar中有3个元素

1. Flexible Space
2. Bar Button Item
3. Flexible Space

`Flexible Space`翻译过来就是弹性空间, 可以想象成一根弹簧,

在`Bar Button Item`两边放两个弹簧, 就把按钮挤到中间去了.

### 二. 固定长度

第二个toolbar中有两个元素

1. Fixed Space
2. Item

`Fixed Space`翻译过来就是固定空间, 可以想象成一根棍子, 长度可以调节,

我这里设置为200

### 三. 分割两端

第三个toolbar中有3个元素

1. Left
2. Flexible Space
3. Right

`Left`和`Right`是两个Item,

用一个弹簧把两个Item挤到两边去



------

上面就是集中常用的布局了吧.

如果3个item的话, 一般就是左中右, 其实应该就是item + flex + item + flex + item,

更多实现就自己想了.
