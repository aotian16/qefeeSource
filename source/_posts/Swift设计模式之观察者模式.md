title: Swift设计模式之观察者模式
categories: ios
tags: 设计模式
date: 2016-05-10 16:07:29

---

<!--head-->

**转自**

* [Swift设计模式](http://qefee.com/tags/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/)

**原文**

* [Design-Patterns-In-Swift](https://github.com/ochococo/Design-Patterns-In-Swift#behavioral)



<!--more-->



<!--body-->

```swift
// 观察者模式
// 一个目标物件管理所有相依于它的观察者物件，并且在它本身的状态改变时主动发出通知。这通常透过呼叫各观察者所提供的方法来实现。此种模式通常被用来实现事件处理系统
// 设计模式分类：行为型模式

/// 观察者接口
protocol PropertyObserver : class {
    /**
     属性即将改变监听方法
     
     - parameter propertyName:     属性名
     - parameter newPropertyValue: 新的值
     */
    func willChangePropertyName(propertyName:String, newPropertyValue:Int)
    /**
     属性已经改变监听方法
     
     - parameter propertyName:     属性名
     - parameter oldPropertyValue: 原来的值
     */
    func didChangePropertyName(propertyName:String, oldPropertyValue:Int)
}

/// 观察对象
class TestChambers {
    
    weak var observer:PropertyObserver?
    
    var testChamberNumber: Int = 0 {
        willSet(newValue) {
            observer?.willChangePropertyName("testChamberNumber", newPropertyValue:newValue)
        }
        didSet {
            observer?.didChangePropertyName("testChamberNumber", oldPropertyValue:oldValue)
        }
    }
}

/// 具体观察者
class Observer : PropertyObserver {
    func willChangePropertyName(propertyName: String, newPropertyValue: Int) {
        print("\(propertyName)的值将要改变为\(newPropertyValue)")
    }
    
    func didChangePropertyName(propertyName: String, oldPropertyValue: Int) {
        print("\(propertyName)的值已经改变,原来的值为\(oldPropertyValue)")
    }
}

var observerInstance = Observer()
var testChambers = TestChambers()
testChambers.observer = observerInstance
testChambers.testChamberNumber += 1
```
