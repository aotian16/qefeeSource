title: Swift设计模式之装饰模式
categories: ios
tags: 设计模式
date: 2016-05-12 10:31:25

---

<!--head-->

**转自**

* [Swift设计模式](http://qefee.com/tags/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/)

**原文**

* [Design-Patterns-In-Swift](https://github.com/ochococo/Design-Patterns-In-Swift#behavioral)



<!--more-->



<!--body-->

```swift
// 装饰模式
// 百度百科：在不必改变原类文件和使用继承的情况下，动态地扩展一个对象的功能。它是通过创建一个包装对象，也就是装饰来包裹真实的对象
// 设计模式分类：结构型模式

/**
 *  咖啡接口
 */
protocol Coffee {
    /**
     价格
     
     - returns: 价格
     */
    func getCost() -> Double
    /**
     原料
     
     - returns: 原料
     */
    func getIngredients() -> String
}

/// 简单咖啡实现
class SimpleCoffee: Coffee {
    func getCost() -> Double {
        return 1.0
    }
    
    func getIngredients() -> String {
        return "Coffee"
    }
}

/// 咖啡装饰接口
class CoffeeDecorator: Coffee {
    private let decoratedCoffee: Coffee
    private let ingredientSeparator: String = ", "
    
    required init(decoratedCoffee: Coffee) {
        self.decoratedCoffee = decoratedCoffee
    }
    
    func getCost() -> Double {
        return decoratedCoffee.getCost()
    }
    
    func getIngredients() -> String {
        return decoratedCoffee.getIngredients()
    }
}

/// 牛奶装饰实现
class Milk: CoffeeDecorator {
    required init(decoratedCoffee: Coffee) {
        super.init(decoratedCoffee: decoratedCoffee)
    }
    
    override func getCost() -> Double {
        return super.getCost() + 0.5
    }
    
    override func getIngredients() -> String {
        return super.getIngredients() + ingredientSeparator + "Milk"
    }
}

/// whip装饰实现
class WhipCoffee: CoffeeDecorator {
    required init(decoratedCoffee: Coffee) {
        super.init(decoratedCoffee: decoratedCoffee)
    }
    
    override func getCost() -> Double {
        return super.getCost() + 0.7
    }
    
    override func getIngredients() -> String {
        return super.getIngredients() + ingredientSeparator + "Whip"
    }
}

var someCoffee: Coffee = SimpleCoffee() // 简单咖啡
print("Cost : \(someCoffee.getCost()); Ingredients: \(someCoffee.getIngredients())")

var milkCoffee = Milk(decoratedCoffee: someCoffee) // 装饰了牛奶的咖啡
print("Cost : \(milkCoffee.getCost()); Ingredients: \(milkCoffee.getIngredients())")

var whipCoffee = WhipCoffee(decoratedCoffee: milkCoffee) // 装饰了whip和牛奶的咖啡
print("Cost : \(whipCoffee.getCost()); Ingredients: \(whipCoffee.getIngredients())")
```
