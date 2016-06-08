title: Swift设计模式之抽象工厂模式
categories: ios
tags: 设计模式
date: 2016-05-10 20:19:34

---

<!--head-->

**转自**

* [Swift设计模式](http://qefee.com/tags/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/)

**原文**

* [Design-Patterns-In-Swift](https://github.com/ochococo/Design-Patterns-In-Swift#behavioral)



<!--more-->



<!--body-->

```swift
// 抽象工厂模式
// 百度百科：为创建一组相关或相互依赖的对象提供一个接口，而且无需指定他们的具体类
// 设计模式分类：创建型模式

import Foundation

/**
 *  抽象工厂
 */
protocol Decimal {
    func stringValue() -> String
    // 工厂方法
    static func make(string : String) -> Decimal
}

typealias NumberFactory = (String) -> Decimal

// Number implementations with factory methods
/**
 *  具体工厂
 */
struct NextStepNumber : Decimal {
    private var nextStepNumber : NSNumber
    
    func stringValue() -> String { return nextStepNumber.stringValue }
    
    // 工厂方法
    static func make(string : String) -> Decimal {
        return NextStepNumber(nextStepNumber:NSNumber(longLong:(string as NSString).longLongValue))
    }
}

/**
 *  具体工厂
 */
struct SwiftNumber : Decimal {
    private var swiftInt : Int
    
    func stringValue() -> String { return "\(swiftInt)" }
    
    // 工厂方法
    static func make(string : String) -> Decimal {
        return SwiftNumber(swiftInt:(string as NSString).integerValue)
    }
}

enum NumberType {
    case NextStep, Swift
}

enum NumberHelper {
    static func factoryFor(type : NumberType) -> NumberFactory {
        switch type {
        case .NextStep:
            return NextStepNumber.make
        case .Swift:
            return SwiftNumber.make
        }
    }
}

let factoryOne = NumberHelper.factoryFor(.NextStep)
let numberOne = factoryOne("1")
numberOne.stringValue()

let factoryTwo = NumberHelper.factoryFor(.Swift)
let numberTwo = factoryTwo("2")
numberTwo.stringValue()
```
