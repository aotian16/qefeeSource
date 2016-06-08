title: Swift设计模式之单例模式
categories: ios
tags: 设计模式
date: 2016-05-11 15:23:17

---

<!--head-->

**转自**

* [Swift设计模式](http://qefee.com/tags/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/)

**原文**

* [Design-Patterns-In-Swift](https://github.com/ochococo/Design-Patterns-In-Swift#behavioral)



<!--more-->



<!--body-->

```swift
// 单例模式
// 百度百科：单例模式是一种常用的软件设计模式。在它的核心结构中只包含一个被称为单例的特殊类。通过单例模式可以保证系统中一个类只有一个实例
// 设计模式分类：创建型模式

class DeathStarSuperlaser {
    static let sharedInstance = DeathStarSuperlaser()
    
    private init() {
        // 私有化构造函数来保证只有一个实例
    }
}

let laser = DeathStarSuperlaser.sharedInstance
```
