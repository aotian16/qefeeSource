title: Swift设计模式之保护代理模式
categories: ios
tags: 设计模式
date: 2016-05-12 11:02:01

---

<!--head-->

**转自**

* [Swift设计模式](http://qefee.com/tags/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/)

**原文**

* [Design-Patterns-In-Swift](https://github.com/ochococo/Design-Patterns-In-Swift#behavioral)



<!--more-->



<!--body-->

```swift
// 保护代理模式
// 百度百科：为其他对象提供一种代理以控制对这个对象的访问。在某些情况下，一个对象不适合或者不能直接引用另一个对象，而代理对象可以在客户端和目标对象之间起到中介的作用
// 设计模式分类：结构型模式

/**
 *  电脑接口
 */
protocol PC {
    func connect(ip: String) -> Bool
}

/// 电脑实现
class MyPC: PC {
    func connect(ip: String) -> Bool {
        print("connect to \(ip)")
        return true
    }
}

/// 代理实现
class MyProxy: PC {
    
    var pc: MyPC!
    
    init(pc: MyPC) {
        self.pc = pc
    }
    
    func connect(ip: String) -> Bool {
        if ip == "10.10.10.10" {
            print("\(ip) has been limited")
            return false
        } else {
            pc.connect(ip)
            return true
        }
    }
}

let pc = MyPC()
let proxy = MyProxy(pc: pc)

proxy.connect("1.2.3.4")
proxy.connect("10.10.10.10")
```
