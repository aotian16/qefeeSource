title: Swift设计模式之原型模式
categories: ios
tags: 设计模式
date: 2016-05-11 15:18:08

---

<!--head-->

**转自**

* [Swift设计模式](http://qefee.com/tags/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/)

**原文**

* [Design-Patterns-In-Swift](https://github.com/ochococo/Design-Patterns-In-Swift#behavioral)



<!--more-->



<!--body-->

```swift
// 原型模式
// 百度百科：用原型实例指定创建对象的种类，并且通过拷贝这些原型创建新的对象
// 设计模式分类：创建型模式

class ChungasRevengeDisplay {
    var name: String?
    let font: String
    
    init(font: String) {
        self.font = font
    }
    
    func clone() -> ChungasRevengeDisplay {
        return ChungasRevengeDisplay(font:self.font)
    }
}

let Prototype = ChungasRevengeDisplay(font:"GotanProject")

let Philippe = Prototype.clone()
Philippe.name = "Philippe"

let Christoph = Prototype.clone()
Christoph.name = "Christoph"

let Eduardo = Prototype.clone()
Eduardo.name = "Eduardo"
```
