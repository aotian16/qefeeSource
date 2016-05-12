title: Swift设计模式整理
categories: ios
tags: 设计模式
date: 2016-05-12 13:13:49

---

<!--head-->

[TOC]

# 定位

swift设计模式的入门读物。各个模式详细介绍请自行搜索网文，博客，也可以查看下面的参考文章。

# 使用方法

1. 读懂代码
2. 在网上查找各种对应设计模式的含义
3. 再次回来读代码并联系含义

# 分类

| No.  | name                                     | type |
| :--: | ---------------------------------------- | :--: |
|  1   | [代理模式](http://qefee.com/2016/05/12/Swift%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F%E4%B9%8B%E4%BF%9D%E6%8A%A4%E4%BB%A3%E7%90%86%E6%A8%A1%E5%BC%8F/) | 结构型  |
|  2   | [外观模式](http://qefee.com/2016/05/12/Swift%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F%E4%B9%8B%E5%A4%96%E8%A7%82%E6%A8%A1%E5%BC%8F/) |      |
|  3   | [装饰模式](http://qefee.com/2016/05/12/Swift%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F%E4%B9%8B%E8%A3%85%E9%A5%B0%E6%A8%A1%E5%BC%8F/) |      |
|  4   | [组合模式](http://qefee.com/2016/05/12/Swift%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F%E4%B9%8B%E7%BB%84%E5%90%88%E6%A8%A1%E5%BC%8F/) |      |
|  5   | [桥梁模式](http://qefee.com/2016/05/11/Swift%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F%E4%B9%8B%E6%A1%A5%E6%A2%81%E6%A8%A1%E5%BC%8F/) |      |
|  6   | [适配器模式](http://qefee.com/2016/05/11/Swift%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F%E4%B9%8B%E9%80%82%E9%85%8D%E5%99%A8%E6%A8%A1%E5%BC%8F/) |      |
|  7   | [单例模式](http://qefee.com/2016/05/11/Swift%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F%E4%B9%8B%E5%8D%95%E4%BE%8B%E6%A8%A1%E5%BC%8F/) | 创建型  |
|  8   | [原型模式](http://qefee.com/2016/05/11/Swift%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F%E4%B9%8B%E5%8E%9F%E5%9E%8B%E6%A8%A1%E5%BC%8F/) |      |
|  9   | [工厂方法模式](http://qefee.com/2016/05/11/Swift%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F%E4%B9%8B%E5%B7%A5%E5%8E%82%E6%96%B9%E6%B3%95%E6%A8%A1%E5%BC%8F/) |      |
|  10  | [创建者模式](http://qefee.com/2016/05/11/Swift%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F%E4%B9%8B%E5%88%9B%E5%BB%BA%E8%80%85%E6%A8%A1%E5%BC%8F/) |      |
|  11  | [抽象工厂模式](http://qefee.com/2016/05/10/Swift%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F%E4%B9%8B%E6%8A%BD%E8%B1%A1%E5%B7%A5%E5%8E%82%E6%A8%A1%E5%BC%8F/) |      |
|  12  | [访问者模式](http://qefee.com/2016/05/10/Swift%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F%E4%B9%8B%E8%AE%BF%E9%97%AE%E8%80%85%E6%A8%A1%E5%BC%8F/) | 行为型  |
|  13  | [策略模式](http://qefee.com/2016/05/10/Swift%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F%E4%B9%8B%E7%AD%96%E7%95%A5%E6%A8%A1%E5%BC%8F/) |      |
|  14  | [状态模式](http://qefee.com/2016/05/10/Swift%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F%E4%B9%8B%E7%8A%B6%E6%80%81%E6%A8%A1%E5%BC%8F/) |      |
|  15  | [观察者模式](http://qefee.com/2016/05/10/Swift%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F%E4%B9%8B%E8%A7%82%E5%AF%9F%E8%80%85%E6%A8%A1%E5%BC%8F/) |      |
|  16  | [备忘录模式](http://qefee.com/2016/05/09/Swift%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F%E4%B9%8B%E5%A4%87%E5%BF%98%E5%BD%95%E6%A8%A1%E5%BC%8F/) |      |
|  17  | [中介者模式](http://qefee.com/2016/05/09/Swift%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F%E4%B9%8B%E4%B8%AD%E4%BB%8B%E8%80%85%E6%A8%A1%E5%BC%8F/) |      |
|  18  | [迭代器模式](http://qefee.com/2016/05/09/Swift%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F%E4%B9%8B%E8%BF%AD%E4%BB%A3%E5%99%A8%E6%A8%A1%E5%BC%8F/) |      |
|  19  | [解释器模式](http://qefee.com/2016/05/09/Swift%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F%E4%B9%8B%E8%A7%A3%E9%87%8A%E5%99%A8%E6%A8%A1%E5%BC%8F/) |      |
|  20  | [命令模式](http://qefee.com/2016/05/09/Swift%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F%E4%B9%8B%E5%91%BD%E4%BB%A4%E6%A8%A1%E5%BC%8F/) |      |
|  22  | [责任链模式](http://qefee.com/2016/05/09/Swift%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F%E4%B9%8B%E8%B4%A3%E4%BB%BB%E9%93%BE%E6%A8%A1%E5%BC%8F/) |      |

# 源码

改动了一部分原文章的代码！

* [**SwiftDesignPatterns**](https://github.com/aotian16/Blog/tree/master/Study/Dev/Swift/SwiftDesignPatterns)

# 参考文章

* [设计模式（百度百科）](http://baike.baidu.com/view/66964.htm)
* [设计模式分类](http://www.cnblogs.com/justForMe/archive/2011/07/18/2109211.html)
* [23种设计模式](http://www.cnblogs.com/beijiguangyong/archive/2010/11/15/2302807.html)
* [设计模式](http://www.runoob.com/design-pattern/design-pattern-tutorial.html)

<!--more-->



<!--body-->
