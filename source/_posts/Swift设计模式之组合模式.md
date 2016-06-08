title: Swift设计模式之组合模式
categories: ios
tags: 设计模式
date: 2016-05-12 10:21:08

---

<!--head-->

**转自**

* [Swift设计模式](http://qefee.com/tags/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/)

**原文**

* [Design-Patterns-In-Swift](https://github.com/ochococo/Design-Patterns-In-Swift#behavioral)



<!--more-->



<!--body-->

```swift
// 桥梁模式
// 百度百科：继承或实现的类通过不同的实现方式来完成抽象类或接口的变化 , 也就是实现过程的变化 , 但可能会有这样的情况 , 抽象过程同样需要进行变化 , 也就是抽象类或者接口需要变化 , 这样就会造成原有的继承或实现关系复杂 , 关系混乱 .桥梁模式利用将抽象层和实现层进行解耦 , 使两者不再像继承或实现这样的较强的关系 , 从而使抽象和实现层更加独立的完成变化的过程 . 使系统更加清晰
// 设计模式分类：创建型模式

/**
 *  开关接口
 */
protocol Switch {
    var appliance: Appliance {get set}
    func turnOn()
}

/**
 *  电器接口
 */
protocol Appliance {
    func run()
}

/// 开关实现
class RemoteControl: Switch {
    var appliance: Appliance
    
    func turnOn() {
        self.appliance.run()
    }
    
    init(appliance: Appliance) {
        self.appliance = appliance
    }
}

/// 电器实现，电视
class TV: Appliance {
    func run() {
        print("tv turned on");
    }
}

/// 电器实现，吸尘器
class VacuumCleaner: Appliance {
    func run() {
        print("vacuum cleaner turned on")
    }
}

var tvRemoteControl = RemoteControl(appliance: TV())
tvRemoteControl.turnOn()

var fancyVacuumCleanerRemoteControl = RemoteControl(appliance: VacuumCleaner())
fancyVacuumCleanerRemoteControl.turnOn()
```
