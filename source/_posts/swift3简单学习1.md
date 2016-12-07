title: swift3简单学习1
categories: ios
tags: swift
date: 2016-09-11 15:17:58

---

<!--head-->

swift3都出来了，赶紧学习下。

本文基本就是简单记录下。

目录

1. ​
2. ​

[原文](https://swift.org/documentation/#the-swift-programming-language)

[TOC]

<!--more-->

<!--body-->

# Welcome to Swift

## A Swift Tour

### `hello world`程序

```swift
print("Hello, world!")
```

### 变量和常量

```swift
var myVariable = 42 // 变量
myVariable = 50
let myConstant = 42 // 常量
let explicitDouble: Double = 70 // 带类型的常量
```

### 类型转换

```swift
let label = "The width is "
let width = 94
let widthLabel = label + String(width) // 必须强制转换
```

### 在字符串中包含变量

```swift
let apples = 3
let oranges = 5
let appleSummary = "I have \(apples) apples."
let fruitSummary = "I have \(apples + oranges) pieces of fruit."
```

### 数组与字典

```swift
var shoppingList = ["catfish", "water", "tulips", "blue paint"]
shoppingList[1] = "bottle of water"
 
var occupations = [
    "Malcolm": "Captain",
    "Kaylee": "Mechanic",
]
occupations["Jayne"] = "Public Relations"
```

```swift
let emptyArray = [String]() // 空数组
let emptyDictionary = [String: Float]() // 空字典
```

### 可选类型

```swift
var optionalString: String? = "Hello"
print(optionalString == nil)
 
var optionalName: String? = "John Appleseed"
var greeting = "Hello!"
if let name = optionalName { // 获取可选类型的值
    greeting = "Hello, \(name)"
}
```

```swift
let nickName: String? = nil
let fullName: String = "John Appleseed"
let informalGreeting = "Hi \(nickName ?? fullName)" // 给可选类型一个默认值.当nickName为nil, 返回默认值，否则返回nickName的值.
```

### switch语句

```swift
let vegetable = "red pepper"
switch vegetable {
case "celery": // 字符串
    print("Add some raisins and make ants on a log.")
case "cucumber", "watercress": // 多个选项
    print("That would make a good tea sandwich.")
case let x where x.hasSuffix("pepper"): // 条件选项
    print("Is it a spicy \(x)?")
default:
    print("Everything tastes good in soup.")
}
```

### for-in语句

```swift
let interestingNumbers = [
    "Prime": [2, 3, 5, 7, 11, 13],
    "Fibonacci": [1, 1, 2, 3, 5, 8],
    "Square": [1, 4, 9, 16, 25],
]
var largest = 0
for (kind, numbers) in interestingNumbers {
    for number in numbers {
        if number > largest {
            largest = number
        }
    }
}
print(largest)
```

### while语句

```swift
var n = 2
while n < 100 {
    n = n * 2
}
print(n)
 
var m = 2
repeat {
    m = m * 2
} while m < 100
print(m)
```

### range范围

- `..<`包前不包后
- `...`包前包后

```swift
var total = 0
for i in 0..<4 {
    total += i
}
print(total)
```

### 函数与闭包

```swift
// 定义函数
func greet(person: String, day: String) -> String {
    return "Hello \(person), today is \(day)."
}
// 使用函数
greet(person: "Bob", day: "Tuesday")
```

```swift
// 可以在参数前面加自定义标签，这样在调用函数就显示为自定义标签。
// 也可以用_，忽略标签
func greet(_ person: String, on day: String) -> String {
    return "Hello \(person), today is \(day)."
}
greet("John", on: "Wednesday")
```

#### 元组

```swift
func calculateStatistics(scores: [Int]) -> (min: Int, max: Int, sum: Int) {
    var min = scores[0]
    var max = scores[0]
    var sum = 0
    
    for score in scores {
        if score > max {
            max = score
        } else if score < min {
            min = score
        }
        sum += score
    }
    
    return (min, max, sum) // 函数返回一个元组
}
let statistics = calculateStatistics(scores: [5, 3, 100, 3, 9])
print(statistics.sum) // 可以用名字调用元组元素
print(statistics.2) // 也可以用数字调用元组元素
```

#### 可变参数

```swift
func sumOf(numbers: Int...) -> Int {
    var sum = 0
    for number in numbers {
        sum += number
    }
    return sum
}
sumOf()
sumOf(numbers: 42, 597, 12)
```

#### 函数嵌套

```swift
func returnFifteen() -> Int {
    var y = 10
    func add() {
        y += 5
    }
    add()
    return y
}
returnFifteen()
```

#### 函数是第一类型

```swift
// 返回一个函数
func makeIncrementer() -> ((Int) -> Int) {
    func addOne(number: Int) -> Int {
        return 1 + number
    }
    return addOne
}
var increment = makeIncrementer()
increment(7)
```

```swift
// 函数作为参数
func hasAnyMatches(list: [Int], condition: (Int) -> Bool) -> Bool {
    for item in list {
        if condition(item) {
            return true
        }
    }
    return false
}
func lessThanTen(number: Int) -> Bool {
    return number < 10
}
var numbers = [20, 19, 7, 12]
hasAnyMatches(list: numbers, condition: lessThanTen)
```

#### 闭包

```swift
numbers.map({
    (number: Int) -> Int in
    let result = 3 * number
    return result
})
```

```swift
// 简洁点的闭包:
// * 当闭包类型已知，可以省略参数和返回值
let mappedNumbers = numbers.map({ number in 3 * number })
print(mappedNumbers)
```

```swift
// 更加简洁的闭包:
// * 可以用数字代替参数名
// * 闭包是函数最后一个参数的话，可以跟在圆括号后面
// * 闭包是函数的唯一参数的话，可以省略圆括号
let sortedNumbers = numbers.sorted { $0 > $1 }
print(sortedNumbers)
```

### 对象与类

```swift
// 类定义
class Shape {
    var numberOfSides = 0 // 属性定义
    // 方法定义
    func simpleDescription() -> String {
        return "A shape with \(numberOfSides) sides."
    }
}

var shape = Shape() // 实例化
shape.numberOfSides = 7 // 调用属性
var shapeDescription = shape.simpleDescription() // 调用方法
```

#### 构造方法

```swift
class NamedShape {
    var numberOfSides: Int = 0
    var name: String
    
    // init就是构造方法
    init(name: String) {
        self.name = name // 通过self来引用自己
    }
    
    func simpleDescription() -> String {
        return "A shape with \(numberOfSides) sides."
    }
}
```

#### 析构方法

`deinit`

#### 类继承与方法重写

```swift
// 用:来继承类
class Square: NamedShape {
    var sideLength: Double
    
    init(sideLength: Double, name: String) {
        self.sideLength = sideLength
        super.init(name: name) // 用super调用父类
        numberOfSides = 4
    }
    
    func area() ->  Double {
        return sideLength * sideLength
    }
    
    // 用override来重写方法
    override func simpleDescription() -> String {
        return "A square with sides of length \(sideLength)."
    }
}
let test = Square(sideLength: 5.2, name: "my test square")
test.area()
test.simpleDescription()
```

#### 储值属性与计算属性（* 不确定是不是这么叫）

```swift
class EquilateralTriangle: NamedShape {
    var sideLength: Double = 0.0 // 储值属性，可以保存值
    
    init(sideLength: Double, name: String) {
        self.sideLength = sideLength
        super.init(name: name)
        numberOfSides = 3
    }
    
    // 计算属性，不存储值，通过get与set方法来扩展属性
    var perimeter: Double {
        get {
            return 3.0 * sideLength
        }
        set {
            sideLength = newValue / 3.0 // newValue表示新值
        }
    }
    
    override func simpleDescription() -> String {
        return "An equilateral triangle with sides of length \(sideLength)."
    }
}
var triangle = EquilateralTriangle(sideLength: 3.1, name: "a triangle")
print(triangle.perimeter)
triangle.perimeter = 9.9
print(triangle.sideLength)
```

#### 属性监听

- `willSet`监听属性即将改变事件
- `didSet`监听属性已经改变事件

```swift
class TriangleAndSquare {
    var triangle: EquilateralTriangle {
        willSet {
            square.sideLength = newValue.sideLength
        }
    }
    var square: Square {
        willSet {
            triangle.sideLength = newValue.sideLength
        }
    }
    init(size: Double, name: String) {
        square = Square(sideLength: size, name: name)
        triangle = EquilateralTriangle(sideLength: size, name: name)
    }
}
var triangleAndSquare = TriangleAndSquare(size: 10, name: "another test shape")
print(triangleAndSquare.square.sideLength)
print(triangleAndSquare.triangle.sideLength)
triangleAndSquare.square = Square(sideLength: 50, name: "larger square")
print(triangleAndSquare.triangle.sideLength)
```

#### 可选类型与表达式

```swift
let optionalSquare: Square? = Square(sideLength: 2.5, name: "optional square")
let sideLength = optionalSquare?.sideLength // 如果?前的值为nil，后面的表达式将无视，直接返回nil
```

### 枚举和结构体

#### 枚举

```swift
enum Rank: Int {
    case ace = 1
    case two, three, four, five, six, seven, eight, nine, ten
    case jack, queen, king
    func simpleDescription() -> String {
        switch self {
        case .ace:
            return "ace"
        case .jack:
            return "jack"
        case .queen:
            return "queen"
        case .king:
            return "king"
        default:
            return String(self.rawValue)
        }
    }
}
let ace = Rank.ace
let aceRawValue = ace.rawValue // 获取原始值
```

#### 用`init?(rawValue:)`原始值实例化枚举

```swift
// 实例化出来的是可选类型
if let convertedRank = Rank(rawValue: 3) {
    let threeDescription = convertedRank.simpleDescription()
}
```

#### 结构体

```swift
struct Card {
    var rank: Rank
    var suit: Suit
    // 结构体也可以有方法
    func simpleDescription() -> String {
        return "The \(rank.simpleDescription()) of \(suit.simpleDescription())"
    }
}
let threeOfSpades = Card(rank: .three, suit: .spades)
let threeOfSpadesDescription = threeOfSpades.simpleDescription()
```

#### 带值的枚举

```swift
enum ServerResponse {
    case result(String, String)
    case failure(String)
}
 
let success = ServerResponse.result("6:00 am", "8:09 pm")
let failure = ServerResponse.failure("Out of cheese.")
 
switch success {
case let .result(sunrise, sunset):
    print("Sunrise is at \(sunrise) and sunset is at \(sunset).")
case let .failure(message):
    print("Failure...  \(message)")
}
```

### 协议和扩展

#### 协议`protocol`

```swift
protocol ExampleProtocol {
    var simpleDescription: String { get }
    mutating func adjust() // mutating 表示可以改变对象,class不需要，结构体需要
}
```

```swift
// 类，结构体和枚举都可以实现协议
class SimpleClass: ExampleProtocol {
    var simpleDescription: String = "A very simple class."
    var anotherProperty: Int = 69105
    func adjust() {
        simpleDescription += "  Now 100% adjusted."
    }
}
var a = SimpleClass()
a.adjust()
let aDescription = a.simpleDescription
 
struct SimpleStructure: ExampleProtocol {
    var simpleDescription: String = "A simple structure"
    mutating func adjust() {
        simpleDescription += " (adjusted)"
    }
}
var b = SimpleStructure()
b.adjust()
let bDescription = b.simpleDescription
```

#### 扩展`extension`,可以添加对象的机能

```swift
// 扩展int，添加一个计算属性和一个方法
extension Int: ExampleProtocol {
    var simpleDescription: String {
        return "The number \(self)"
    }
    mutating func adjust() {
        self += 42
    }
}
print(7.simpleDescription)
```

### 错误处理

#### 错误协议`Error`

```swift
// 实现错误协议表明这是错误
enum PrinterError: Error {
    case outOfPaper
    case noToner
    case onFire
}
```

#### 错误声明`throws`和错误抛出`throw`

```swift
func send(job: Int, toPrinter printerName: String) throws -> String {
    if printerName == "Never Has Toner" {
        throw PrinterError.noToner
    }
    return "Job sent"
}
```

#### 错误捕捉`try`,`catch`

```swift
do {
    let printerResponse = try send(job: 1040, toPrinter: "Bi Sheng")
    print(printerResponse)
} catch {
    print(error) // error是默认的错误名，也可以自己命名
}
```

#### 错误捕捉分支

```swift
do {
    let printerResponse = try send(job: 1440, toPrinter: "Gutenberg")
    print(printerResponse)
} catch PrinterError.onFire {
    print("I willl just put this over here, with the rest of the fire.")
} catch let printerError as PrinterError {
    print("Printer error: \(printerError).")
} catch {
    print(error)
}
```

#### 错误转可选类型

如果抛出错误，可选类型的值为nil，而且错误被无视

如果正常，可选类型的值为返回值

```swift
let printerSuccess = try? send(job: 1884, toPrinter: "Mergenthaler")
let printerFailure = try? send(job: 1885, toPrinter: "Never Has Toner")
```

#### 延迟执行`defer`

`defer`块中的代码会在函数的其他代码执行完后运行。

（有点像析构函数）

```swift
var fridgeIsOpen = false
let fridgeContent = ["milk", "eggs", "leftovers"]
 
func fridgeContains(_ food: String) -> Bool {
    fridgeIsOpen = true
    defer {
        fridgeIsOpen = false
    }
    
    let result = fridgeContent.contains(food)
    return result
}
fridgeContains("banana")
print(fridgeIsOpen)
```

### 泛型

#### 泛型方法

```swift
func makeArray<Item>(repeating item: Item, numberOfTimes: Int) -> [Item] {
    var result = [Item]()
    for _ in 0..<numberOfTimes {
        result.append(item)
    }
    return result
}
makeArray(repeating: "knock", numberOfTimes:4)
```

#### 泛型类，枚举和构造体

```swift
// Reimplement the Swift standard library's optional type
enum OptionalValue<Wrapped> {
    case none
    case some(Wrapped)
}
var possibleInteger: OptionalValue<Int> = .none
possibleInteger = .some(100)
```

#### 条件化泛型,`where`条件

```swift
func anyCommonElements<T: Sequence, U: Sequence where T.Iterator.Element: Equatable, T.Iterator.Element == U.Iterator.Element>(_ lhs: T, _ rhs: U) -> Bool {
    for lhsItem in lhs {
        for rhsItem in rhs {
            if lhsItem == rhsItem {
                return true
            }
        }
    }
    return false
}
anyCommonElements([1, 2, 3], [3])
```