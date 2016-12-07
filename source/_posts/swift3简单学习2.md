title: swift3简单学习2
categories: ios
tags: swift
date: 2016-09-20 16:03:15

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


## 语言向导Language Guide

### 基本

略

### 注释

#### 行内注释

```swift
// this is comment 行内注释
```

#### 块注释

```swift
/* This is also a comment
 but is written over multiple lines. 
 块注释
 */
```

#### 注释嵌套

```swift
/* This is the start of the first multiline comment.
 /* This is the second, nested multiline comment. */
 This is the end of the first multiline comment. */
```

### 整型

- 整形有8，16，32，64位的


- 支持有符号，无符号

#### 整型范围

```swift
let minValue = UInt8.min  // minValue is equal to 0, and is of type UInt8
let maxValue = UInt8.max  // maxValue is equal to 255, and is of type UInt8
```

#### `Int`，平台无关的有符号整型

- 32位平台上，长度同Int32
- 64位平台上，长度同Int64

#### `UInt`，平台无关的无符号整型

- 32位平台上，长度同UInt32
- 64位平台上，长度同UInt64

### 浮点型

- `Float`，单精度浮点型，32位


- `Double`，双精度浮点型，64位

### 类型安全与类型推断

略

#### 数值字面值

##### 整型

```swift
let decimalInteger = 17           // 十进制
let binaryInteger = 0b10001       // 17 in binary notation 二进制
let octalInteger = 0o21           // 17 in octal notation  八进制
let hexadecimalInteger = 0x11     // 17 in hexadecimal notation 十六进制
```

##### 浮点型

略

### 数值类型转换

#### 整型转换

```swift
let twoThousand: UInt16 = 2_000
let one: UInt8 = 1
let twoThousandAndOne = twoThousand + UInt16(one)
```

#### 浮点型转换

##### int to float

```swift
let three = 3
let pointOneFourOneFiveNine = 0.14159
let pi = Double(three) + pointOneFourOneFiveNine
// pi equals 3.14159, and is inferred to be of type Double
```

##### float to int

```swift
let integerPi = Int(pi)
// integerPi equals 3, and is inferred to be of type Int
// 等于3，被截断
```

### 类型别名

```swift
// 定义别名
typealias AudioSample = UInt16
```

```swift
// 使用别名
var maxAmplitudeFound = AudioSample.min
// maxAmplitudeFound is now 0
```

### 元组

#### 定义元组

```swift
let http404Error = (404, "Not Found")
// http404Error is of type (Int, String), and equals (404, "Not Found")
```

#### 访问元组值

```swift
let (statusCode, statusMessage) = http404Error
print("The status code is \(statusCode)")
// Prints "The status code is 404"
print("The status message is \(statusMessage)")
// Prints "The status message is Not Found"
```

#### 使用`_`忽略部分元组值

```swift
let (justTheStatusCode, _) = http404Error
print("The status code is \(justTheStatusCode)")
// Prints "The status code is 404"
```

#### 用索引访问元组值

```swift
print("The status code is \(http404Error.0)")
// Prints "The status code is 404"
print("The status message is \(http404Error.1)")
// Prints "The status message is Not Found"
```

#### 定义带名字的元组值

```swift
let http200Status = (statusCode: 200, description: "OK")
```

#### 使用带名字的元组值

```swift
print("The status code is \(http200Status.statusCode)")
// Prints "The status code is 200"
print("The status message is \(http200Status.description)")
// Prints "The status message is OK"
```

### 可选类型

```swift
let possibleNumber = "123"
let convertedNumber = Int(possibleNumber)
// convertedNumber is inferred to be of type "Int?", or "optional Int"
// 字符串转int的时候有可能失败，所以返回的是可选类型
```

#### nil

略

#### if声明和强制解包

if判断后，用`!`来强制解包

```swift
if convertedNumber != nil {
    print("convertedNumber has an integer value of \(convertedNumber!).") // 使用!强制解包
}
// Prints "convertedNumber has an integer value of 123."
```

#### 可选绑定

如果可选类型不为nil，就会提取值到actualNumber上，

否则走else分支

```swift
if let actualNumber = Int(possibleNumber) {
    print("\"\(possibleNumber)\" has an integer value of \(actualNumber)")
} else {
    print("\"\(possibleNumber)\" could not be converted to an integer")
}
// Prints ""123" has an integer value of 123"
```

#### 带条件的可选绑定

下面两个等价

```swift
if let firstNumber = Int("4"), let secondNumber = Int("42"), firstNumber < secondNumber && secondNumber < 100 {
    print("\(firstNumber) < \(secondNumber) < 100")
}
// Prints "4 < 42 < 100"
```

```swift
if let firstNumber = Int("4") {
    if let secondNumber = Int("42") {
        if firstNumber < secondNumber && secondNumber < 100 {
            print("\(firstNumber) < \(secondNumber) < 100")
        }
    }
}
// Prints "4 < 42 < 100"
```

#### 隐性解包的可选类型 (* 不确定是不是这个名字)

即用`！`声明的可选类型。（一般表示比较确定有值的可选类型）

```swift
let possibleString: String? = "An optional string."
let forcedString: String = possibleString! // 需要！强制解包

// 隐性解包的可选类型
let assumedString: String! = "An implicitly unwrapped optional string."
let implicitString: String = assumedString // 不需要！强制解包，会自动解包
```

```swift
if assumedString != nil {
    print(assumedString)
}
// Prints "An implicitly unwrapped optional string."
// 还是可以判断nil
```

```swift
if let definiteString = assumedString {
    print(definiteString)
}
// Prints "An implicitly unwrapped optional string."
// 也可以用let解包
```

### 错误处理

略（看前文）

### 断言

某种条件下，程序无需或不能继续处理时候，断言提供了一个调试的机会。

#### 断言调试

```swift
let age = -3
assert(age >= 0, "A person's age cannot be less than zero")
// this causes the assertion to trigger, because age is not >= 0
```

```swift
assert(age >= 0)
```

#### 什么时候使用断言

- 可能超出自定义下标范围的时候
- 可能有无效的函数参数
- 下文需要用到可选类型的值，但是可选类型可能为nil的时候

### 基本运算符

略（加减乘除等）

#### 术语

| No.  | Name  | Demo           |
| :--: | ----- | -------------- |
|  1   | 一元运算符 | `-a`,`!b`,`c!` |
|  2   | 二元运算符 | `a + b`        |
|  3   | 三元运算符 | `a ? b : c`    |

#### 赋值运算符

```swift
let b = 10
var a = 5
a = b
// a is now equal to 10
```

```swift
let (x, y) = (1, 2)
// x is equal to 1, and y is equal to 2
```

#### 算术运算符

略（加减乘除等）

#### 取余运算符

略(`%`)

#### 一元减运算符

略(`-`)

#### 一元加运算符

略(`+`)

#### 混合运算符

略(`+=`等)

#### 比较运算符

- `==`
- `!=`
- `>`
- `<`
- `>=`
- `<=`

##### 元组的比较

如果元组的类型相同，而且各个元素都是可以比较的话，就能用比较运算符。

```swift
(1, "zebra") < (2, "apple")   // true because 1 is less than 2
(3, "apple") < (3, "bird")    // true because 3 is equal to 3, and "apple" is less than "bird"
(4, "dog") == (4, "dog")      // true because 4 is equal to 4, and "dog" is equal to "dog"
```

#### 三元条件运算符

略(`a ? b : c`)

#### nil合并运算符(Nil-Coalescing Operator)(* 不确定是不是这个名字)

功能：可选类型为nil的时候赋予默认值。

```swift
let defaultColorName = "red"
var userDefinedColorName: String?   // defaults to nil,默认为nil
 
var colorNameToUse = userDefinedColorName ?? defaultColorName
// userDefinedColorName is nil, so colorNameToUse is set to the default of "red"
// 可选类型为nil，所以赋值为"red"
```

```swift
userDefinedColorName = "green"
colorNameToUse = userDefinedColorName ?? defaultColorName
// userDefinedColorName is not nil, so colorNameToUse is set to "green"
// 可选类型不为nil，所以赋值为"green"
```

#### 区间运算符(Range Operators)

有两种：

- 闭区间运算符，`a...b`，包前包后
- 半开区间运算符，`a..<b`，包前不包后

```swift
for index in 1...5 {
    print("\(index) times 5 is \(index * 5)")
}
// 1 times 5 is 5
// 2 times 5 is 10
// 3 times 5 is 15
// 4 times 5 is 20
// 5 times 5 is 25
```

```swift
let names = ["Anna", "Alex", "Brian", "Jack"]
let count = names.count
for i in 0..<count {
    print("Person \(i + 1) is called \(names[i])")
}
// Person 1 is called Anna
// Person 2 is called Alex
// Person 3 is called Brian
// Person 4 is called Jack
```

#### 逻辑运算符

略（与或非）

### 字符串与字符

#### 字符串字面值

```swift
let someString = "Some string literal value" // 双引号范围内的就是字符串字面值
```

#### 初始化一个空字符串

有两种方法

```swift
var emptyString = ""               // empty string literal
var anotherEmptyString = String()  // initializer syntax
// these two strings are both empty, and are equivalent to each other
```

判断是否为空

```swift
if emptyString.isEmpty {
    print("Nothing to see here")
}
// Prints "Nothing to see here"
```

#### 字符串可变性

变量可变，常量不可变。

```swift
var variableString = "Horse"
variableString += " and carriage"
// variableString is now "Horse and carriage"
// 字符串改变
 
let constantString = "Highlander"
constantString += " and another Highlander"
// this reports a compile-time error - a constant string cannot be modified
// ❎，编译错误
```

#### 字符串是值类型

也就是说，赋值或者传递参数的时候会拷贝一个副本。

#### 操作字符

##### 遍历字符串中的字符

```swift
for character in "Dog!🐶".characters {
    print(character)
}
// D
// o
// g
// !
// 🐶
```

##### 字符变量定义

```swift
let exclamationMark: Character = "!"
```

##### 字符数组转字符串

```swift
let catCharacters: [Character] = ["C", "a", "t", "!", "🐱"]
let catString = String(catCharacters)
print(catString)
// Prints "Cat!🐱"
```

#### 连接字符串和字符

```swift
let string1 = "hello"
let string2 = " there"
var welcome = string1 + string2 // 用+连接字符串
// welcome now equals "hello there"
```

```swift
var instruction = "look over"
instruction += string2 // 用+=连接字符串
// instruction now equals "look over there"
```

```swift
let exclamationMark: Character = "!"
welcome.append(exclamationMark) // 添加字符到字符串上
// welcome now equals "hello there!"
```

#### 字符串插入

```swift
let multiplier = 3
let message = "\(multiplier) times 2.5 is \(Double(multiplier) * 2.5)" // 在字符串中插入变量
// message is "3 times 2.5 is 7.5"
```

#### 字符串中的特殊符号

用`\`插入特殊符号

用`\u`插入unicode

```swift
let wiseWords = "\"Imagination is more important than knowledge\" - Einstein" // 
// "Imagination is more important than knowledge" - Einstein
let dollarSign = "\u{24}"        // $,  Unicode scalar U+0024 
let blackHeart = "\u{2665}"      // ♥,  Unicode scalar U+2665
let sparklingHeart = "\u{1F496}" // 💖, Unicode scalar U+1F496
```

#### 扩展字形组(* 不确定不是不是这个名字)

(警告： 这里也不是特别理解，需要自己去看下原文)

一个扩展自行组可能由一个或者多个Unicode标量组成。

比如：`é`有两种表示方法

- U+00E9
- U+0065 U+0301

```swift
let eAcute: Character = "\u{E9}"                         // é
let combinedEAcute: Character = "\u{65}\u{301}"          // e和 ́
// eAcute is é, combinedEAcute is é
```

两者都是一个swift字符。前者一个Unicode标量，后者两个。

```swift
let precomposed: Character = "\u{D55C}"                  // 한
let decomposed: Character = "\u{1112}\u{1161}\u{11AB}"   // ᄒ, ᅡ, ᆫ
// precomposed is 한, decomposed is 한
```

```swift
let enclosedEAcute: Character = "\u{E9}\u{20DD}"
// enclosedEAcute is é⃝
```

```swift
let regionalIndicatorForUS: Character = "\u{1F1FA}\u{1F1F8}"
// regionalIndicatorForUS is 🇺🇸
```

#### 计算字符数

```swift
let unusualMenagerie = "Koala 🐨, Snail 🐌, Penguin 🐧, Dromedary 🐪"
print("unusualMenagerie has \(unusualMenagerie.characters.count) characters")
// Prints "unusualMenagerie has 40 characters"
```

由于前文的原因，删减字符串中的字符不一定会改变字符数

```swift
var word = "cafe"
print("the number of characters in \(word) is \(word.characters.count)")
// Prints "the number of characters in cafe is 4"
 
word += "\u{301}"    // COMBINING ACUTE ACCENT, U+0301
 
print("the number of characters in \(word) is \(word.characters.count)")
// Prints "the number of characters in café is 4"
// e加个字符 ́变成了é，字符总数没变
```

#### 访问与修改字符串

通过属性，方法或者下标可以访问和修改字符串。

##### 字符串下标

由于前文所说的原因，swift中不能用整型访问字符串的字符，只能通过专门的`String.Index`来访问。

- `startIndex`指向第一个字符的位置
- `endIndex`指向最后一个字符之后的位置，所以`endIndex`不是一个有效的字符串下标。

```swift
let greeting = "Guten Tag!"
greeting[greeting.startIndex]
// G
greeting[greeting.index(before: greeting.endIndex)]
// !
greeting[greeting.index(after: greeting.startIndex)]
// u
let index = greeting.index(greeting.startIndex, offsetBy: 7)
greeting[index]
// a
```

访问所有字符串的字符索引

```swift
for index in greeting.characters.indices {
    print("\(greeting[index]) ", terminator: "")
}
// Prints "G u t e n   T a g ! "
```

##### 插入与删除

```swift
var welcome = "hello"
welcome.insert("!", at: welcome.endIndex) // 插入单个字符
// welcome now equals "hello!"
 
welcome.insert(contentsOf:" there".characters, at: welcome.index(before: welcome.endIndex)) // 插入字符串
// welcome now equals "hello there!"
```

```swift
welcome.remove(at: welcome.index(before: welcome.endIndex)) // 删除单个字符
// welcome now equals "hello there"
 
let range = welcome.index(welcome.endIndex, offsetBy: -6)..<welcome.endIndex
welcome.removeSubrange(range) // 删除区间
// welcome now equals "hello"
```

#### 比较字符串

##### 字符与字符串的相等性

- `==`判断相等
- `!=`判断不相等

```swift
let quotation = "We're a lot alike, you and I."
let sameQuotation = "We're a lot alike, you and I."
if quotation == sameQuotation {
    print("These two strings are considered equal")
}
// Prints "These two strings are considered equal"
```

由于上文的扩展字形组的原因，由不同Unicode标量组成的字符串也可能相等。

比如：

```swift
// "Voulez-vous un café?" using LATIN SMALL LETTER E WITH ACUTE
let eAcuteQuestion = "Voulez-vous un caf\u{E9}?"
 
// "Voulez-vous un café?" using LATIN SMALL LETTER E and COMBINING ACUTE ACCENT
let combinedEAcuteQuestion = "Voulez-vous un caf\u{65}\u{301}?"
 
if eAcuteQuestion == combinedEAcuteQuestion {
    print("These two strings are considered equal")
}
// Prints "These two strings are considered equal"
```

相反，两个长得一样的字符也可能不相等。

比如：

拉丁字符`A`，与西里尔字符`А`

```swift
let latinCapitalLetterA: Character = "\u{41}"
 
let cyrillicCapitalLetterA: Character = "\u{0410}"
 
if latinCapitalLetterA != cyrillicCapitalLetterA {
    print("These two characters are not equivalent.")
}
// Prints "These two characters are not equivalent."
```

#### 前缀与后缀相等性

```swift
let romeoAndJuliet = [
    "Act 1 Scene 1: Verona, A public place",
    "Act 1 Scene 2: Capulet's mansion",
    "Act 1 Scene 3: A room in Capulet's mansion",
    "Act 1 Scene 4: A street outside Capulet's mansion",
    "Act 1 Scene 5: The Great Hall in Capulet's mansion",
    "Act 2 Scene 1: Outside Capulet's mansion",
    "Act 2 Scene 2: Capulet's orchard",
    "Act 2 Scene 3: Outside Friar Lawrence's cell",
    "Act 2 Scene 4: A street in Verona",
    "Act 2 Scene 5: Capulet's mansion",
    "Act 2 Scene 6: Friar Lawrence's cell"
]
```

前缀

```swift
var act1SceneCount = 0
for scene in romeoAndJuliet {
    if scene.hasPrefix("Act 1 ") {
        act1SceneCount += 1
    }
}
print("There are \(act1SceneCount) scenes in Act 1")
// Prints "There are 5 scenes in Act 1"
```

后缀

```swift
var mansionCount = 0
var cellCount = 0
for scene in romeoAndJuliet {
    if scene.hasSuffix("Capulet's mansion") {
        mansionCount += 1
    } else if scene.hasSuffix("Friar Lawrence's cell") {
        cellCount += 1
    }
}
print("\(mansionCount) mansion scenes; \(cellCount) cell scenes")
// Prints "6 mansion scenes; 2 cell scenes"
```

#### 字符串的Unicode表示

有3种表示方法：

- UTF-8
- UTF-16
- UTF-32

比如字符串：

```swift
let dogString = "Dog‼🐶"
```

UTF-8结构

![utf-8](http://qefee.qiniudn.com/swift3stuty_utf-8.png)

```swift
for codeUnit in dogString.utf8 {
    print("\(codeUnit) ", terminator: "")
}
print("")
// 68 111 103 226 128 188 240 159 144 182
```

UTF-16结构

![utf-16](http://qefee.qiniudn.com/swift3stuty_utf-16.png)

```swift
for codeUnit in dogString.utf16 {
    print("\(codeUnit) ", terminator: "")
}
print("")
// Prints "68 111 103 8252 55357 56374 "
```

UTF-32结构

![utf-32](http://qefee.qiniudn.com/swift3stuty_utf-32.png)

```swift
for scalar in dogString.unicodeScalars {
    print("\(scalar.value) ", terminator: "")
}
print("")
// Prints "68 111 103 8252 128054 "
```

```swift
for scalar in dogString.unicodeScalars {
    print("\(scalar) ")
}
// D
// o
// g
// ‼
// 🐶
```

### 集合类型

3种主要的集合类型

- array，数组
- set，集
- dictionary，字典

![img](http://qefee.qiniudn.com/swift3stuty_collection_type.png)

#### 集合的可变性

集合是变量的话，长度和内容可变

集合是常量的话，长度和内容不可变

#### 数组

元素有序，可重复

##### 数组简写语法

```swift
Array<Element> // 原始写法
[Element]      // 简写(推荐)
```

##### 创建一个空数组

```swift
var someInts = [Int]()
print("someInts is of type [Int] with \(someInts.count) items.")
// Prints "someInts is of type [Int] with 0 items."
```

```swift
someInts.append(3)
// someInts now contains 1 value of type Int
someInts = [] // 类型已知的时候，可以这样写
// someInts is now an empty array, but is still of type [Int]
```

##### 创建一个有默认值的数组

```swift
var threeDoubles = Array(repeating: 0.0, count: 3) // 含有3个元素，每个默认值为0.0
// threeDoubles is of type [Double], and equals [0.0, 0.0, 0.0]
```

##### 合并数组

```swift
var anotherThreeDoubles = Array(repeating: 2.5, count: 3)
// anotherThreeDoubles is of type [Double], and equals [2.5, 2.5, 2.5]
 
var sixDoubles = threeDoubles + anotherThreeDoubles // 直接用+
// sixDoubles is inferred as [Double], and equals [0.0, 0.0, 0.0, 2.5, 2.5, 2.5]
```

##### 创建数组字面值

```swift
var shoppingList: [String] = ["Eggs", "Milk"]
// shoppingList has been initialized with two initial items
```

```swift
var shoppingList = ["Eggs", "Milk"] // 简写，使用类型推断为string数组
```

##### 访问与修改数组

```swift
print("The shopping list contains \(shoppingList.count) items.") // count元素个数
// Prints "The shopping list contains 2 items."
```

```swift
// isEmpty判断是否为空
if shoppingList.isEmpty {
    print("The shopping list is empty.")
} else {
    print("The shopping list is not empty.")
}
// Prints "The shopping list is not empty."
```

```swift
shoppingList.append("Flour") // append添加新元素
// shoppingList now contains 3 items, and someone is making pancakes
```

```swift
shoppingList += ["Baking Powder"] // 添加多个元素
// shoppingList now contains 4 items
shoppingList += ["Chocolate Spread", "Cheese", "Butter"]
// shoppingList now contains 7 items
```

```swift
var firstItem = shoppingList[0] // 获取元素
// firstItem is equal to "Eggs"
```

```swift
shoppingList[0] = "Six eggs" // 修改元素值
// the first item in the list is now equal to "Six eggs" rather than "Eggs"
```

```swift
shoppingList[4...6] = ["Bananas", "Apples"] // 修改元素区间
// shoppingList now contains 6 items
```

```swift
shoppingList.insert("Maple Syrup", at: 0) // 插入元素
// shoppingList now contains 7 items
// "Maple Syrup" is now the first item in the list
```

```swift
let mapleSyrup = shoppingList.remove(at: 0) // 删除元素
// the item that was at index 0 has just been removed
// shoppingList now contains 6 items, and no Maple Syrup
// the mapleSyrup constant is now equal to the removed "Maple Syrup" string
```

```swift
let apples = shoppingList.removeLast() // 删除最后一个元素
// the last item in the array has just been removed
// shoppingList now contains 5 items, and no apples
// the apples constant is now equal to the removed "Apples" string
```

```swift
// 遍历元素
for item in shoppingList {
    print(item)
}
// Six eggs
// Milk
// Flour
// Baking Powder
// Bananas
```

#### 集

元素无序，而且唯一

##### 集类型的哈希值

集中的元素必须是可哈希的。

哈希值是一个Int值，用来判断元素的相等性

swift的基本元素都是可哈希的，也就是说可以放入集。

##### 集的语法

```swift
Set<Element>
```

##### 创建一个空的集

```swift
var letters = Set<Character>()
print("letters is of type Set<Character> with \(letters.count) items.")
// Prints "letters is of type Set<Character> with 0 items."
```

```swift
letters.insert("a")
// letters now contains 1 value of type Character
letters = [] // 类型已知的时候可以这样清空
// letters is now an empty set, but is still of type Set<Character>
```

##### 创建一个有数组字面值的集

```swift
var favoriteGenres: Set<String> = ["Rock", "Classical", "Hip hop"]
// favoriteGenres has been initialized with three initial items
```

```swift
var favoriteGenres: Set = ["Rock", "Classical", "Hip hop"] // 类型可以推断的时候可以简写
```

##### 访问与修改集

```swift
print("I have \(favoriteGenres.count) favorite music genres.") // 元素个数
// Prints "I have 3 favorite music genres."
```

```swift
// 是否为空
if favoriteGenres.isEmpty {
    print("As far as music goes, I'm not picky.")
} else {
    print("I have particular music preferences.")
}
// Prints "I have particular music preferences."
```

```swift
favoriteGenres.insert("Jazz") // 插入
// favoriteGenres now contains 4 items
```

```swift
// 删除
// 删除的时候会返回删除的值的可选值，如果为nil就是没有
if let removedGenre = favoriteGenres.remove("Rock") {
    print("\(removedGenre)? I'm over it.")
} else {
    print("I never much cared for that.")
}
// Prints "Rock? I'm over it."
```

```swift
// 检查是否包含
if favoriteGenres.contains("Funk") {
    print("I get up on the good foot.")
} else {
    print("It's too funky in here.")
}
// Prints "It's too funky in here."
```

```swift
// 迭代
for genre in favoriteGenres {
    print("\(genre)")
}
// Jazz
// Hip hop
// Classical
```

```swift
// 排序
// 集本身是无序的，想要排序需要调用sorted方法
for genre in favoriteGenres.sorted() {
    print("\(genre)")
}
// Classical
// Hip hop
// Jazz
```

##### 集运算

###### 基本的集运算

![img](http://qefee.qiniudn.com/swift3stuty_collection_type_set_%20fundamental.png)

- `intersection(_:)`求交集，a，b中都有的元素


- `symmetricDifference(_:)`,(不知道叫什么)，a，b含有的不同元素


- `union(_:)`求并集，a，b中所有的元素


- `subtracting(_:)`,(不知道叫什么)，在a中不在b中的元素

```swift
let oddDigits: Set = [1, 3, 5, 7, 9]
let evenDigits: Set = [0, 2, 4, 6, 8]
let singleDigitPrimeNumbers: Set = [2, 3, 5, 7]
 
oddDigits.union(evenDigits).sorted()
// [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
oddDigits.intersection(evenDigits).sorted()
// []
oddDigits.subtracting(singleDigitPrimeNumbers).sorted()
// [1, 9]
oddDigits.symmetricDifference(singleDigitPrimeNumbers).sorted()
// [1, 2, 9]
```

###### 集的关系与相等性

![img](http://qefee.qiniudn.com/swift3stuty_collection_type_set_%20membership.png)

- `==`判断相等


- `isSubset(of:)`判断是否是子集


- `isSuperset(of:)`判断是否是超集


- `isStrictSubset(of:)`和`isStrictSuperset(of:)`判断是否是严格子集和超集，就是一方包含另一方，但是不相等
- `isDisjoint(with:)`是否没有相同的元素

```swift
let houseAnimals: Set = ["🐶", "🐱"]
let farmAnimals: Set = ["🐮", "🐔", "🐑", "🐶", "🐱"]
let cityAnimals: Set = ["🐦", "🐭"]
 
houseAnimals.isSubset(of: farmAnimals)
// true
farmAnimals.isSuperset(of: houseAnimals)
// true
farmAnimals.isDisjoint(with: cityAnimals)
// true
```

#### 字典

##### 字典的简写

```swift
Dictionary<Key, Value> // 全写
```

```swift
[Key: Value] // 类型已知的时候可以简写
```

##### 创建空字典

```swift
var namesOfIntegers = [Int: String]()
// namesOfIntegers is an empty [Int: String] dictionary
```

```swift
namesOfIntegers[16] = "sixteen" // 插入
// namesOfIntegers now contains 1 key-value pair
namesOfIntegers = [:] // 清空
// namesOfIntegers is once again an empty dictionary of type [Int: String]
```

##### 创建带字面值的字典

```swift
var airports: [String: String] = ["YYZ": "Toronto Pearson", "DUB": "Dublin"]
```

```swift
var airports = ["YYZ": "Toronto Pearson", "DUB": "Dublin"] // 简写
```

##### 访问与修改字典

```swift
print("The airports dictionary contains \(airports.count) items.") // 元素个数
// Prints "The airports dictionary contains 2 items."
```

```swift
// 是否为空
if airports.isEmpty {
    print("The airports dictionary is empty.")
} else {
    print("The airports dictionary is not empty.")
}
// Prints "The airports dictionary is not empty."
```

```swift
airports["LHR"] = "London" // 插入
// the airports dictionary now contains 3 items
```

```swift
airports["LHR"] = "London Heathrow" // 如果已经有key的话，就变成了修改了
// the value for "LHR" has been changed to "London Heathrow"
```

```swift
// updateValue，同上面两个一样也是插入或者更新值。
// 不同的是，方法返回一个可选值，如果是更新，会返回原来的值,否则返回nil，这样可以用来确认是否是更新
if let oldValue = airports.updateValue("Dublin Airport", forKey: "DUB") {
    print("The old value for DUB was \(oldValue).")
}
// Prints "The old value for DUB was Dublin."
```

```swift
// 同理，用下标访问字典也是一样，返回元素可选值
if let airportName = airports["DUB"] {
    print("The name of the airport is \(airportName).")
} else {
    print("That airport is not in the airports dictionary.")
}
// Prints "The name of the airport is Dublin Airport."
```

```swift
airports["APL"] = "Apple International"
// "Apple International" is not the real airport for APL, so delete it
airports["APL"] = nil // 删除
// APL has now been removed from the dictionary
```

```swift
// removeValue删除，返回删除元素可选值，用来确认是否有删除操作
if let removedValue = airports.removeValue(forKey: "DUB") {
    print("The removed airport's name is \(removedValue).")
} else {
    print("The airports dictionary does not contain a value for DUB.")
}
// Prints "The removed airport's name is Dublin Airport."
```

##### 迭代字典

```swift
// 用元组迭代key和value
for (airportCode, airportName) in airports {
    print("\(airportCode): \(airportName)")
}
// YYZ: Toronto Pearson
// LHR: London Heathrow
```

```swift
// 也可以单独迭代key，value
for airportCode in airports.keys {
    print("Airport code: \(airportCode)")
}
// Airport code: YYZ
// Airport code: LHR
 
for airportName in airports.values {
    print("Airport name: \(airportName)")
}
// Airport name: Toronto Pearson
// Airport name: London Heathrow
```

```swift
let airportCodes = [String](airports.keys) // 字典的keys转数组
// airportCodes is ["YYZ", "LHR"]
 
let airportNames = [String](airports.values)
// airportNames is ["Toronto Pearson", "London Heathrow"] // 字典的values转数组
```

### 控制流

#### For-In循环

```swift
// 区间
for index in 1...5 {
    print("\(index) times 5 is \(index * 5)")
}
// 1 times 5 is 5
// 2 times 5 is 10
// 3 times 5 is 15
// 4 times 5 is 20
// 5 times 5 is 25
```

```swift
let base = 3
let power = 10
var answer = 1
// 不用值的时候可以用_无视掉
for _ in 1...power {
    answer *= base
}
print("\(base) to the power of \(power) is \(answer)")
// Prints "3 to the power of 10 is 59049"
```

```swift
let names = ["Anna", "Alex", "Brian", "Jack"]
// 数组
for name in names {
    print("Hello, \(name)!")
}
// Hello, Anna!
// Hello, Alex!
// Hello, Brian!
// Hello, Jack!
```

```swift
let numberOfLegs = ["spider": 8, "ant": 6, "cat": 4]
// 字典用元组来循环取值
for (animalName, legCount) in numberOfLegs {
    print("\(animalName)s have \(legCount) legs")
}
// ants have 6 legs
// spiders have 8 legs
// cats have 4 legs
```

#### While循环

有两种:

- while 条件前置
- repeat-while 条件后置

##### While

略

##### Repeat-while

略

#### 条件语句

##### If

```swift
var temperatureInFahrenheit = 30
if temperatureInFahrenheit <= 32 {
    print("It's very cold. Consider wearing a scarf.")
}
// Prints "It's very cold. Consider wearing a scarf."
```

##### If-else

```swift
temperatureInFahrenheit = 40
if temperatureInFahrenheit <= 32 {
    print("It's very cold. Consider wearing a scarf.")
} else {
    print("It's not that cold. Wear a t-shirt.")
}
// Prints "It's not that cold. Wear a t-shirt."
```

##### If-else if-else

```swift

temperatureInFahrenheit = 90
if temperatureInFahrenheit <= 32 {
    print("It's very cold. Consider wearing a scarf.")
} else if temperatureInFahrenheit >= 86 {
    print("It's really warm. Don't forget to wear sunscreen.")
} else {
    print("It's not that cold. Wear a t-shirt.")
}
// Prints "It's really warm. Don't forget to wear sunscreen."
```

##### else可以省略

```swift
temperatureInFahrenheit = 72
if temperatureInFahrenheit <= 32 {
    print("It's very cold. Consider wearing a scarf.")
} else if temperatureInFahrenheit >= 86 {
    print("It's really warm. Don't forget to wear sunscreen.")
}
```

#### Switch

switch必须包含所有的条件语句，如果想要一个类似其他的条件，可以提供一个default语句。

switch默认是每个分支执行后跳出的，不需要`break`

```swift
let someCharacter: Character = "z"
switch someCharacter {
case "a":
    print("The first letter of the alphabet")
case "z":
    print("The last letter of the alphabet")
default:
    print("Some other character")
}
// Prints "The last letter of the alphabet"
```

每个switch语句分支必须包含至少一条可执行语句。

比如下面的下是不对的：

```swift
let anotherCharacter: Character = "a"
switch anotherCharacter {
case "a": // 这里错误，至少需要一条可执行语句 Invalid, the case has an empty body
case "A":
    print("The letter A")
default:
    print("Not the letter A")
}
// This will report a compile-time error.
```

```swift
let anotherCharacter: Character = "a"
// 一个case可以包含多个条件
switch anotherCharacter {
case "a", "A":
    print("The letter A")
default:
    print("Not the letter A")
}
// Prints "The letter A"
```

```swift
let approximateCount = 62
let countedThings = "moons orbiting Saturn"
var naturalCount: String
// case可以进行区间匹配
switch approximateCount {
case 0:
    naturalCount = "no"
case 1..<5:
    naturalCount = "a few"
case 5..<12:
    naturalCount = "several"
case 12..<100:
    naturalCount = "dozens of"
case 100..<1000:
    naturalCount = "hundreds of"
default:
    naturalCount = "many"
}
print("There are \(naturalCount) \(countedThings).")
// Prints "There are dozens of moons orbiting Saturn."
```

```swift
let somePoint = (1, 1)
// case也可以进行元组匹配
switch somePoint {
case (0, 0): // 匹配原点
    print("(0, 0) is at the origin")
case (_, 0): // 匹配y轴
    print("(\(somePoint.0), 0) is on the x-axis")
case (0, _): // 匹配x轴
    print("(0, \(somePoint.1)) is on the y-axis")
case (-2...2, -2...2): // 匹配区域
    print("(\(somePoint.0), \(somePoint.1)) is inside the box")
default: // 其他
    print("(\(somePoint.0), \(somePoint.1)) is outside of the box")
}
// Prints "(1, 1) is inside the box"
```

![img](http://qefee.qiniudn.com/swift3stuty_%20conditional_switch_tuples.png)

```swift
let anotherPoint = (2, 0)
// case语句可以绑定值
switch anotherPoint {
case (let x, 0):
    print("on the x-axis with an x value of \(x)")
case (0, let y):
    print("on the y-axis with a y value of \(y)")
case let (x, y):
    print("somewhere else at (\(x), \(y))")
}
// Prints "on the x-axis with an x value of 2"
```

![img](http://qefee.qiniudn.com/swift3study_conditional_switch_value_binding.png)

```swift
let yetAnotherPoint = (1, -1)
// case语句可以包含一个where子句
switch yetAnotherPoint {
case let (x, y) where x == y:
    print("(\(x), \(y)) is on the line x == y")
case let (x, y) where x == -y:
    print("(\(x), \(y)) is on the line x == -y")
case let (x, y):
    print("(\(x), \(y)) is just some arbitrary point")
}
// Prints "(1, -1) is on the line x == -y"
```

![img](http://qefee.qiniudn.com/swift3study_conditional_switch_where.png)

```swift
let someCharacter: Character = "e"
// 混合case语句，一个case可以包含多个条件
switch someCharacter {
case "a", "e", "i", "o", "u":
    print("\(someCharacter) is a vowel")
case "b", "c", "d", "f", "g", "h", "j", "k", "l", "m",
     "n", "p", "q", "r", "s", "t", "v", "w", "x", "y", "z":
    print("\(someCharacter) is a consonant")
default:
    print("\(someCharacter) is not a vowel or a consonant")
}
// Prints "e is a vowel"
```

```swift
let stillAnotherPoint = (9, 0)
// 混合的元组case
switch stillAnotherPoint {
case (let distance, 0), (0, let distance):
    print("On an axis, \(distance) from the origin")
default:
    print("Not on an axis")
}
// Prints "On an axis, 9 from the origin"
```

### 控制转移语句

- continue
- break
- fallthrough
- return
- throw

#### Continue

跳过本次循环

```swift
let puzzleInput = "great minds think alike"
var puzzleOutput = ""
for character in puzzleInput.characters {
    switch character {
    case "a", "e", "i", "o", "u", " ":
        continue
    default:
        puzzleOutput.append(character)
    }
}
print(puzzleOutput)
// Prints "grtmndsthnklk"
```

#### Break

##### 循环语句中的break

跳出循环

##### Switch语句中的break

```swift
let numberSymbol: Character = "三"  // Chinese symbol for the number 3
var possibleIntegerValue: Int?
switch numberSymbol {
case "1", "١", "一", "๑":
    possibleIntegerValue = 1
case "2", "٢", "二", "๒":
    possibleIntegerValue = 2
case "3", "٣", "三", "๓":
    possibleIntegerValue = 3
case "4", "٤", "四", "๔":
    possibleIntegerValue = 4
default:
    break
}
if let integerValue = possibleIntegerValue {
    print("The integer value of \(numberSymbol) is \(integerValue).")
} else {
    print("An integer value could not be found for \(numberSymbol).")
}
// Prints "The integer value of 三 is 3."
```

#### Fallthrough

switch默认是不会跳到下个分支去的，可以用`fallthrough`来强制进入下个分支。

```swift
let integerToDescribe = 5
var description = "The number \(integerToDescribe) is"
switch integerToDescribe {
case 2, 3, 5, 7, 11, 13, 17, 19:
    description += " a prime number, and also"
    fallthrough
default:
    description += " an integer."
}
print(description)
// Prints "The number 5 is a prime number, and also an integer."
```

#### 标签语句

略（跳转到带有标签的语句）

#### 提前退出

`guard`和`if`一样，基于表达式的布尔值执行。

不同的是，`guard`总是跟着一个else，当表达式为`false`的时候执行else中的语句

```swift
func greet(person: [String: String]) {
    guard let name = person["name"] else {
        return
    }
    
    print("Hello \(name)!")
    
    guard let location = person["location"] else {
        print("I hope the weather is nice near you.")
        return
    }
    
    print("I hope the weather is nice in \(location).")
}
 
greet(person: ["name": "John"])
// Prints "Hello John!"
// Prints "I hope the weather is nice near you."
greet(person: ["name": "Jane", "location": "Cupertino"])
// Prints "Hello Jane!"
// Prints "I hope the weather is nice in Cupertino."
```

#### 检查api的可用性

```swift
if #available(iOS 10, macOS 10.12, *) {
    // Use iOS 10 APIs on iOS, and use macOS 10.12 APIs on macOS
} else {
    // Fall back to earlier iOS and macOS APIs
}
```

### 函数

#### 定义与调用函数

```swift
func greet(person: String) -> String {
    let greeting = "Hello, " + person + "!"
    return greeting
}
```

```swift
print(greet(person: "Anna"))
// Prints "Hello, Anna!"
print(greet(person: "Brian"))
// Prints "Hello, Brian!"print(greet(person: "Anna"))
// Prints "Hello, Anna!"
print(greet(person: "Brian"))
// Prints "Hello, Brian!"
```

#### 函数参数与返回值

##### 无参数的函数

```swift
func sayHelloWorld() -> String {
    return "hello, world"
}
print(sayHelloWorld())
// Prints "hello, world"func sayHelloWorld() -> String {
    return "hello, world"
}
print(sayHelloWorld())
// Prints "hello, world"
```

##### 多参数的函数

```swift
func greet(person: String, alreadyGreeted: Bool) -> String {
    if alreadyGreeted {
        return greetAgain(person: person)
    } else {
        return greet(person: person)
    }
}
print(greet(person: "Tim", alreadyGreeted: true))
// Prints "Hello again, Tim!"
```

##### 没有返回值的函数

```swift
func greet(person: String) {
    print("Hello, \(person)!")
}
greet(person: "Dave")
// Prints "Hello, Dave!"
```
```swift
// 函数的返回值可以无视
func printAndCount(string: String) -> Int {
    print(string)
    return string.characters.count
}
func printWithoutCounting(string: String) {
    let _ = printAndCount(string: string)
}
printAndCount(string: "hello, world") 
// prints "hello, world" and rturns a value of 12
printWithoutCounting(string: "hello, world")
// prints "hello, world" but does not return a value
```
##### 返回多个返回值的函数
```swift
// 可以用元组返回多个值
func minMax(array: [Int]) -> (min: Int, max: Int) {
    var currentMin = array[0]
    var currentMax = array[0]
    for value in array[1..<array.count] {
        if value < currentMin {
            currentMin = value
        } else if value > currentMax {
            currentMax = value
        }
    }
    return (currentMin, currentMax)
}
```
```swift
let bounds = minMax(array: [8, -6, 2, 109, 3, 71])
print("min is \(bounds.min) and max is \(bounds.max)")
// Prints "min is -6 and max is 109"
```
##### 可选元组返回值
```swift
// 当数组为空，返回nil。所以返回值是一个可选值
func minMax(array: [Int]) -> (min: Int, max: Int)? {
    if array.isEmpty { return nil }
    var currentMin = array[0]
    var currentMax = array[0]
    for value in array[1..<array.count] {
        if value < currentMin {
            currentMin = value
        } else if value > currentMax {
            currentMax = value
        }
    }
    return (currentMin, currentMax)
}
```
```swift
if let bounds = minMax(array: [8, -6, 2, 109, 3, 71]) {
    print("min is \(bounds.min) and max is \(bounds.max)")
}
// Prints "min is -6 and max is 109"
```
##### 函数的实参标签与形参名
- 实参标签 用于调用函数
- 形参名 用于函数内部实现

```swift
// 默认的情况下，实参标签就是他们的形参名
func someFunction(firstParameterName: Int, secondParameterName: Int) {
    // In the function body, firstParameterName and secondParameterName
    // refer to the argument values for the first and second parameters.
}
someFunction(firstParameterName: 1, secondParameterName: 2)
```
###### 定义实参标签
```swift
// from就是实参标签，调用函数的时候会用到
func greet(person: String, from hometown: String) -> String {
    return "Hello \(person)!  Glad you could visit from \(hometown)."
}
print(greet(person: "Bill", from: "Cupertino"))
// Prints "Hello Bill!  Glad you could visit from Cupertino."
```
###### 删除实参标签
```swift
// 用_来删除实参标签
func someFunction(_ firstParameterName: Int, secondParameterName: Int) {
    // In the function body, firstParameterName and secondParameterName
    // refer to the argument values for the first and second parameters.
}
someFunction(1, secondParameterName: 2)
```
##### 参数默认值

```swift
// 参数parameterWithDefault的默认值是12
func someFunction(parameterWithoutDefault: Int, parameterWithDefault: Int = 12) {
    // In the function body, if no arguments are passed to the function
    // call, the value of parameterWithDefault is 12.
}
someFunction(parameterWithoutDefault: 3, parameterWithDefault: 6) // parameterWithDefault is 6
someFunction(parameterWithoutDefault: 4) // parameterWithDefault is 12
```

##### 可变个数参数

一个函数最多有一个可变个数参数

```swift
// 用...定义可变个数参数
func arithmeticMean(_ numbers: Double...) -> Double {
    var total: Double = 0
    for number in numbers {
        total += number
    }
    return total / Double(numbers.count)
}
arithmeticMean(1, 2, 3, 4, 5)
// returns 3.0, which is the arithmetic mean of these five numbers
arithmeticMean(3, 8.25, 18.75)
// returns 10.0, which is the arithmetic mean of these three numbers
```

##### In-Out参数

函数的参数默认是常数，不能修改的，需要修改参数的时候，可以将参数定义为In-Out参数。

```swift
func swapTwoInts(_ a: inout Int, _ b: inout Int) {
    let temporaryA = a
    a = b
    b = temporaryA
}
```

```swift
var someInt = 3
var anotherInt = 107
swapTwoInts(&someInt, &anotherInt)
print("someInt is now \(someInt), and anotherInt is now \(anotherInt)")
// Prints "someInt is now 107, and anotherInt is now 3
```

#### 函数类型

函数都有他自己的类型，比如：

```swift
func addTwoInts(_ a: Int, _ b: Int) -> Int {
    return a + b
}
func multiplyTwoInts(_ a: Int, _ b: Int) -> Int {
    return a * b
}
```

上面的两个函数有相同的类型

`(Int, Int) -> Int`

即接收两个整型参数并返回一个整型返回值。

```swift
func printHelloWorld() {
    print("hello, world")
}
```

`() -> Void`表示无参数，无返回值的函数类型。

##### 使用函数类型

```swift
var mathFunction: (Int, Int) -> Int = addTwoInts
```

```swift
print("Result: \(mathFunction(2, 3))")
// Prints "Result: 5"
```

```swift
mathFunction = multiplyTwoInts
print("Result: \(mathFunction(2, 3))")
// Prints "Result: 6"
```

```swift
let anotherMathFunction = addTwoInts // 也可以使用类型推导
// anotherMathFunction is inferred to be of type (Int, Int) -> Int
```

##### 函数类型作为参数

```swift
func printMathResult(_ mathFunction: (Int, Int) -> Int, _ a: Int, _ b: Int) {
    print("Result: \(mathFunction(a, b))")
}
printMathResult(addTwoInts, 3, 5)
// Prints "Result: 8"
```

##### 函数参数作为返回值

```swift
func stepForward(_ input: Int) -> Int {
    return input + 1
}
func stepBackward(_ input: Int) -> Int {
    return input - 1
}
```

```swift
func chooseStepFunction(backward: Bool) -> (Int) -> Int {
    return backward ? stepBackward : stepForward
}
```

```swift
var currentValue = 3
let moveNearerToZero = chooseStepFunction(backward: currentValue > 0)
// moveNearerToZero now refers to the stepBackward() function
```

```swift
print("Counting to zero:")
// Counting to zero:
while currentValue != 0 {
    print("\(currentValue)... ")
    currentValue = moveNearerToZero(currentValue)
}
print("zero!")
// 3...
// 2...
// 1...
// zero!
```

#### 嵌套函数

```swift
func chooseStepFunction(backward: Bool) -> (Int) -> Int {
    func stepForward(input: Int) -> Int { return input + 1 }
    func stepBackward(input: Int) -> Int { return input - 1 }
    return backward ? stepBackward : stepForward
}
var currentValue = -4
let moveNearerToZero = chooseStepFunction(backward: currentValue > 0)
// moveNearerToZero now refers to the nested stepForward() function
while currentValue != 0 {
    print("\(currentValue)... ")
    currentValue = moveNearerToZero(currentValue)
}
print("zero!")
// -4...
// -3...
// -2...
// -1...
// zero!
```

### 闭包

| No.  | Type  | Detail              |
| :--: | ----- | ------------------- |
|  1   | 全局函数  | 就是有名字的闭包，而且不捕获任何值   |
|  2   | 嵌套函数  | 就是有名字的闭包，可以捕获包围函数的值 |
|  3   | 闭包表达式 | 轻量级的闭包，可以捕获包围块的值    |

#### 闭包表达式

##### Sorted方法

Swift标准库提供了一个排序方法用来给数组排序，排序基于接收的闭包参数。

```swift
let names = ["Chris", "Alex", "Ewa", "Barry", "Daniella"]
```

```swift
// 函数也是特殊的闭包
func backward(_ s1: String, _ s2: String) -> Bool {
    return s1 > s2
}
// 这里直接传入排序函数名作为闭包参数
var reversedNames = names.sorted(by: backward)
// reversedNames is equal to ["Ewa", "Daniella", "Chris", "Barry", "Alex"]
```
##### 闭包表达式语法

```swift
reversedNames = names.sorted(by: { (s1: String, s2: String) -> Bool in
    return s1 > s2
})
```

```swift
reversedNames = names.sorted(by: { (s1: String, s2: String) -> Bool in return s1 > s2 } ) // 代码比较短，可以写成一行
```

##### 从上下文推导类型

```swift
reversedNames = names.sorted(by: { s1, s2 in return s1 > s2 } ) // 由于闭包作为参数，swift可以推导出类型
```

##### 单行闭包的隐含式返回值

```swift
reversedNames = names.sorted(by: { s1, s2 in s1 > s2 } ) // 单行内容的表达式可以省略return
```

##### 简写的实参名

```swift
reversedNames = names.sorted(by: { $0 > $1 } )
```

##### 操作符函数

```swift
reversedNames = names.sorted(by: >)
```

#### 尾闭包

函数的最后一个参数是闭包的时候支持尾闭包写法。

```swift
func someFunctionThatTakesAClosure(closure: () -> Void) {
    // function body goes here
}
 
// Here's how you call this function without using a trailing closure:
// 可以用普通闭包写法
someFunctionThatTakesAClosure(closure: {
    // closure's body goes here
})
 
// Here's how you call this function with a trailing closure instead:
// 尾闭包写法
someFunctionThatTakesAClosure() {
    // trailing closure's body goes here
}
```

```swift
reversedNames = names.sorted() { $0 > $1 }
```

如果闭包是函数的唯一参数的时候可以省略圆括号

```swift
reversedNames = names.sorted { $0 > $1 }
```
尾闭包在闭包体比较长的时候比较有用。
比如数组的`map`函数
```swift
let digitNames = [
    0: "Zero", 1: "One", 2: "Two",   3: "Three", 4: "Four",
    5: "Five", 6: "Six", 7: "Seven", 8: "Eight", 9: "Nine"
]
let numbers = [16, 58, 510]
```
```swift
let strings = numbers.map {
    (number) -> String in
    var number = number
    var output = ""
    repeat {
        output = digitNames[number % 10]! + output
        number /= 10
    } while number > 0
    return output
}
// strings is inferred to be of type [String]
// its value is ["OneSix", "FiveEight", "FiveOneZero"]
```
#### 捕获值

闭包可以捕获包围块的常量与变量，即使原范围的值已经不存在了。

```swift
func makeIncrementer(forIncrement amount: Int) -> () -> Int {
    var runningTotal = 0
    func incrementer() -> Int {
        runningTotal += amount // 捕获runningTotal变量
        return runningTotal
    }
    return incrementer
}
```

```swift
// 调用函数后返回闭包，此闭包捕获了一个变量
let incrementByTen = makeIncrementer(forIncrement: 10)
```
```swift
incrementByTen()
// returns a value of 10 返回10
incrementByTen()
// returns a value of 20 返回20
incrementByTen()
// returns a value of 30 返回30
```
```swift
// 调用函数后返回闭包，此闭包也捕获了一个变量
let incrementBySeven = makeIncrementer(forIncrement: 7)
incrementBySeven()
// returns a value of 7 返回7
```
```swift
// 不同的闭包捕获的变量不同
incrementByTen()
// returns a value of 40 返回40
```
#### 闭包是引用类型

```swift
let alsoIncrementByTen = incrementByTen
alsoIncrementByTen()
// returns a value of 50 返回50
```

#### 非逃逸闭包

可以用`@noescape`申明闭包为非逃逸的，这样就可以让编译器优化代码。

```swift
func someFunctionWithNonescapingClosure(closure: @noescape () -> Void) {
    closure()
}
```

数组的`sorted(by:)`函数就被参数定义为一个非逃逸的闭包，应为闭包在排序完成后就不再需要了。

而有些函数会接收逃逸闭包作为参数，然后进行异步操作。

```swift
// 存储可逃逸闭包，用来异步执行
var completionHandlers: [() -> Void] = []
func someFunctionWithEscapingClosure(completionHandler: () -> Void) {
    // completionHandler是可逃逸闭包
    completionHandlers.append(completionHandler)
}
```
```swift
class SomeClass {
    var x = 10
    func doSomething() {
        someFunctionWithNonescapingClosure { x = 200 }
        someFunctionWithEscapingClosure { self.x = 100 }
    }
}
 
let instance = SomeClass()
instance.doSomething()
print(instance.x)
// Prints "200"
 
completionHandlers.first?()
print(instance.x) // 异步执行闭包
// Prints "100"
```
#### 自动闭包

```swift
var customersInLine = ["Chris", "Alex", "Ewa", "Barry", "Daniella"]
print(customersInLine.count)
// Prints "5"
 
let customerProvider = { customersInLine.remove(at: 0) } // 闭包，会延迟到调用的时候加载
print(customersInLine.count)
// Prints "5"
 
print("Now serving \(customerProvider())!") // 调用闭包后，加载代码
// Prints "Now serving Chris!"
print(customersInLine.count)
// Prints "4"
```
```swift
// customersInLine is ["Alex", "Ewa", "Barry", "Daniella"]
func serve(customer customerProvider: () -> String) {
    print("Now serving \(customerProvider())!")
}
serve(customer: { customersInLine.remove(at: 0) } ) // 闭包当参数
// Prints "Now serving Alex!"
```
自动闭包写法，`@autoclosure`。

```swift
// customersInLine is ["Ewa", "Barry", "Daniella"]
// 自动闭包,@autoclosure
func serve(customer customerProvider: @autoclosure () -> String) {
    print("Now serving \(customerProvider())!")
}
serve(customer: customersInLine.remove(at: 0))
// Prints "Now serving Ewa!"
```
自动闭包默认是非逃逸的，如果想要定义一个逃逸的自动闭包，`@autoclosure(escaping)`

```swift
// customersInLine is ["Barry", "Daniella"]
var customerProviders: [() -> String] = []
// 逃逸的自动闭包
func collectCustomerProviders(_ customerProvider: @autoclosure(escaping) () -> String) {
    customerProviders.append(customerProvider)
}
collectCustomerProviders(customersInLine.remove(at: 0))
collectCustomerProviders(customersInLine.remove(at: 0))
 
print("Collected \(customerProviders.count) closures.")
// Prints "Collected 2 closures."
for customerProvider in customerProviders {
    print("Now serving \(customerProvider())!")
}
// Prints "Now serving Barry!"
// Prints "Now serving Daniella!"
```

### 枚举

#### 语法
```swift
enum CompassPoint {
    case north
    case south
    case east
    case west
}
```
```swift
// 多个case可以写在一行
enum Planet {
    case mercury, venus, earth, mars, jupiter, saturn, uranus, neptune
}
```
#### 在switch语句中匹配枚举
```swift
directionToHead = .south
switch directionToHead {
case .north:
    print("Lots of planets have a north")
case .south:
    print("Watch out for penguins")
case .east:
    print("Where the sun rises")
case .west:
    print("Where the skies are blue")
}
// Prints "Watch out for penguins"
```
```swift
let somePlanet = Planet.earth
switch somePlanet {
case .earth:
    print("Mostly harmless")
default:
    print("Not a safe place for humans")
}
// Prints "Mostly harmless"
```
#### 关联值

枚举量可以关联值。

```swift
enum Barcode {
    case upc(Int, Int, Int, Int)
    case qrCode(String)
}
```
```swift
var productBarcode = Barcode.upc(8, 85909, 51226, 3)
```
```swift
productBarcode = .qrCode("ABCDEFGHIJKLMNOP")
```

```swift
switch productBarcode {
case .upc(let numberSystem, let manufacturer, let product, let check):
    print("UPC: \(numberSystem), \(manufacturer), \(product), \(check).")
case .qrCode(let productCode):
    print("QR code: \(productCode).")
}
// Prints "QR code: ABCDEFGHIJKLMNOP."
```
简写
```swift
switch productBarcode {
case let .upc(numberSystem, manufacturer, product, check):
    print("UPC : \(numberSystem), \(manufacturer), \(product), \(check).")
case let .qrCode(productCode):
    print("QR code: \(productCode).")
}
// Prints "QR code: ABCDEFGHIJKLMNOP."
```
#### Raw值
与关联值差不多，raw值也可以赋予枚举一个值，每个枚举的raw值类型一致。
```swift
// 字符raw值
enum ASCIIControlCharacter: Character {
    case tab = "\t"
    case lineFeed = "\n"
    case carriageReturn = "\r"
}
```
##### 隐含分配的raw值
```swift
// int型枚举的raw值，如果不赋值的话，默认第一个为0，后面的依次增加。
enum Planet: Int {
    case mercury = 1, venus, earth, mars, jupiter, saturn, uranus, neptune
}
```
```swift
// 字符串型的枚举的raw值是枚举case名。
enum CompassPoint: String {
    case north, south, east, west
}
```
```swift
let earthsOrder = Planet.earth.rawValue
// earthsOrder is 3
 
let sunsetDirection = CompassPoint.west.rawValue
// sunsetDirection is "west"
```

#### 从raw值初始化枚举

```swift
// 由于从raw值初始化枚举不一定成功，所以返回的是枚举的可选类型
let possiblePlanet = Planet(rawValue: 7)
// possiblePlanet is of type Planet? and equals Planet.uranus
```
```swift
let positionToFind = 11
if let somePlanet = Planet(rawValue: positionToFind) {
    switch somePlanet {
    case .earth:
        print("Mostly harmless")
    default:
        print("Not a safe place for humans")
    }
} else {
    print("There isn't a planet at position \(positionToFind)")
}
// Prints "There isn't a planet at position 11"
```
#### 递归枚举
枚举可以递归，关键字`indirect`
```swift
// 在需要的case前面添加indirect
enum ArithmeticExpression {
    case number(Int)
    indirect case addition(ArithmeticExpression, ArithmeticExpression)
    indirect case multiplication(ArithmeticExpression, ArithmeticExpression)
}
```
```swift
// 也可以加在enum前面
indirect enum ArithmeticExpression {
    case number(Int)
    case addition(ArithmeticExpression, ArithmeticExpression)
    case multiplication(ArithmeticExpression, ArithmeticExpression)
}
```
```swift
let five = ArithmeticExpression.number(5)
let four = ArithmeticExpression.number(4)
let sum = ArithmeticExpression.addition(five, four)
let product = ArithmeticExpression.multiplication(sum, ArithmeticExpression.number(2))
```
```swift
func evaluate(_ expression: ArithmeticExpression) -> Int {
    switch expression {
    case let .number(value):
        return value
    case let .addition(left, right):
        return evaluate(left) + evaluate(right)
    case let .multiplication(left, right):
        return evaluate(left) * evaluate(right)
    }
}
 
print(evaluate(product))
// Prints "18"
```
### 类和结构体

#### 比较类与结构体

略

#### 定义语法

```swift
class SomeClass {
    // class definition goes here
}
struct SomeStructure {
    // structure definition goes here
}
```

```swift
struct Resolution {
    var width = 0
    var height = 0
}
class VideoMode {
    var resolution = Resolution()
    var interlaced = false
    var frameRate = 0.0
    var name: String?
}
```
#### 类和结构体的实例
```swift
let someResolution = Resolution()
let someVideoMode = VideoMode()
```
#### 访问属性
```swift
print("The width of someResolution is \(someResolution.width)")
// Prints "The width of someResolution is 0"
```
```swift
print("The width of someVideoMode is \(someVideoMode.resolution.width)")
// Prints "The width of someVideoMode is 0"
```
```swift
someVideoMode.resolution.width = 1280
print("The width of someVideoMode is now \(someVideoMode.resolution.width)")
// Prints "The width of someVideoMode is now 1280"
```


#### 按成员初始化结构体
```swift
let vga = Resolution(width: 640, height: 480)
```
#### 结构体与枚举都是值类型
也就是说，赋予变量或常量，还有传递给函数，都是原始值的副本。
比如:
```swift
let hd = Resolution(width: 1920, height: 1080)
var cinema = hd // 复制结构体
```
```swift
cinema.width = 2048 // 改变其中一个的值
```
```swift
print("cinema is now \(cinema.width) pixels wide") // 值已改变
// Prints "cinema is now 2048 pixels wide"
```
```swift
print("hd is still \(hd.width) pixels wide") // 复制的结构体没有变，说明确实是两个实例
// Prints "hd is still 1920 pixels wide"
```
枚举也是一样，是值类型
```swift
enum CompassPoint {
    case north, south, east, west
}
var currentDirection = CompassPoint.west
let rememberedDirection = currentDirection
currentDirection = .east
if rememberedDirection == .west {
    print("The remembered direction is still .west")
}
// Prints "The remembered direction is still .west"
```
#### 类是引用类型
不同于值类型，类在赋予变量或常量，还有传递给函数的时候都是引用的同一个实例。
比如:
```swift
let tenEighty = VideoMode()
tenEighty.resolution = hd
tenEighty.interlaced = true
tenEighty.name = "1080i"
tenEighty.frameRate = 25.0
```
```swift
let alsoTenEighty = tenEighty // 复制类引用
alsoTenEighty.frameRate = 30.0 // 改变引用类的值
```
```swift
print("The frameRate property of tenEighty is now \(tenEighty.frameRate)") // 可以看到原值也被改变了，说明指向是同一个
// Prints "The frameRate property of tenEighty is now 30.0"
```

#### 相同性操作符
由于类是引用类型，所以swift提供类两个相同性操作符来比较两个实例是否指向同一个对象。
- `===` 相同
- `!==` 不相同

```swift
if tenEighty === alsoTenEighty {
    print("tenEighty and alsoTenEighty refer to the same VideoMode instance.")
}
// Prints "tenEighty and alsoTenEighty refer to the same VideoMode instance."
```
**注意不要写成`==`。**

#### 指针
略
#### 选择类还是结构体
略
#### 字符串，数组，字典的赋值与复制
在swift中字符串，数组，字典是结构体。
所以赋值与赋值的时候会生成一个副本。
### 属性
属性是类，结构体，枚举的关联值。
- 存储属性 可以存储值
- 计算属性 不能存储值，只能提供一个计算后的值


存储属性与计算属性的支持性

|  类型  | 存储属性 | 计算属性 |
| :--: | :--: | :--: |
|  类   |  ✅   |  ✅   |
| 结构体  |  ✅   |  ✅   |
|  枚举  |  ❌   |  ✅   |

#### 存储属性
可以是变量，也可以是常量。
```swift
struct FixedLengthRange {
    var firstValue: Int
    let length: Int
}
var rangeOfThreeItems = FixedLengthRange(firstValue: 0, length: 3)
// the range represents integer values 0, 1, and 2
rangeOfThreeItems.firstValue = 6
// the range now represents integer values 6, 7, and 8
```
##### 常量结构体实例的存储属性
由于结构式是值类型，所以一旦申明结构体为常量，那么结构体的所有属性都不能修改，即使是申明为变量的属性也不行。
```swift
let rangeOfFourItems = FixedLengthRange(firstValue: 0, length: 4)
// this range represents integer values 0, 1, 2, and 3
rangeOfFourItems.firstValue = 6 // 这里会报错
// this will report an error, even though firstValue is a variable property
```
##### 延迟创建的存储属性
用`lazy`可以定义一个延迟创建的属性。
这个属性会在第一次使用的时候才创建。
```swift
class DataImporter {
    /*
     DataImporter is a class to import data from an external file.
     The class is assumed to take a non-trivial amount of time to initialize.
     */
    var fileName = "data.txt"
    // the DataImporter class would provide data importing functionality here
}
 
class DataManager {
    lazy var importer = DataImporter()
    var data = [String]()
    // the DataManager class would provide data management functionality here
}
 
let manager = DataManager() // 此时DataImporter还没有被创建
manager.data.append("Some data")
manager.data.append("Some more data")
// the DataImporter instance for the importer property has not yet been created
```
```swift
print(manager.importer.fileName) // 第一次调用DataImporter的时候创建
// the DataImporter instance for the importer property has now been created
// Prints "data.txt"
```

##### 存储属性与实例变量

略

#### 计算属性

```swift
struct Point {
    var x = 0.0, y = 0.0 // 存储属性
}
struct Size {
    var width = 0.0, height = 0.0 // 存储属性
}
struct Rect {
    var origin = Point() // 存储属性
    var size = Size() // 存储属性
    // 计算属性
    var center: Point {
        get {
            let centerX = origin.x + (size.width / 2)
            let centerY = origin.y + (size.height / 2)
            return Point(x: centerX, y: centerY)
        }
        set(newCenter) {
            origin.x = newCenter.x - (size.width / 2)
            origin.y = newCenter.y - (size.height / 2)
        }
    }
}
var square = Rect(origin: Point(x: 0.0, y: 0.0),
                  size: Size(width: 10.0, height: 10.0))
let initialSquareCenter = square.center
square.center = Point(x: 15.0, y: 15.0)
print("square.origin is now at (\(square.origin.x), \(square.origin.y))")
// Prints "square.origin is now at (10.0, 10.0)"
```

![img](http://qefee.qiniudn.com/swift3study_computed_properties.png)

##### 简写Setter申明
计算属性的setter方法没有第一变量名的时候，可以用`newValue`来引用新值。
```swift
struct AlternativeRect {
    var origin = Point()
    var size = Size()
    var center: Point {
        get {
            let centerX = origin.x + (size.width / 2)
            let centerY = origin.y + (size.height / 2)
            return Point(x: centerX, y: centerY)
        }
        set {
            origin.x = newValue.x - (size.width / 2)
            origin.y = newValue.y - (size.height / 2)
        }
    }
}
```
##### 只读计算属性
即只提供get方法的计算属性。
```swift
struct Cuboid {
    var width = 0.0, height = 0.0, depth = 0.0
    var volume: Double {
        return width * height * depth
    }
}
let fourByFiveByTwo = Cuboid(width: 4.0, height: 5.0, depth: 2.0)
print("the volume of fourByFiveByTwo is \(fourByFiveByTwo.volume)")
// Prints "the volume of fourByFiveByTwo is 40.0"
```
#### 属性监听器
- willSet 即将改变属性值时候调用
- didSet 已经改变属性值时候调用

```swift
class StepCounter {
    var totalSteps: Int = 0 {
        willSet(newTotalSteps) {
            print("About to set totalSteps to \(newTotalSteps)")
        }
        didSet {
            if totalSteps > oldValue  {
                print("Added \(totalSteps - oldValue) steps")
            }
        }
    }
}
let stepCounter = StepCounter()
stepCounter.totalSteps = 200
// About to set totalSteps to 200
// Added 200 steps
stepCounter.totalSteps = 360
// About to set totalSteps to 360
// Added 160 steps
stepCounter.totalSteps = 896
// About to set totalSteps to 896
// Added 536 steps
```
##### 全局与本地变量
属性监听器也适用于全局与本地变量。
#### 类型属性
类型属性即属于类型本身的属性。
##### 类型属性语法
用`static`来定义类型属性。
类的计算属性，也可以用`class`来定义，表明可以被子类重写。
```swift
struct SomeStructure {
    static var storedTypeProperty = "Some value."
    static var computedTypeProperty: Int {
        return 1
    }
}
enum SomeEnumeration {
    static var storedTypeProperty = "Some value."
    static var computedTypeProperty: Int {
        return 6
    }
}
class SomeClass {
    static var storedTypeProperty = "Some value."
    static var computedTypeProperty: Int {
        return 27
    }
    class var overrideableComputedTypeProperty: Int {
        return 107
    }
}
```
##### 操作类型属性
```swift
print(SomeStructure.storedTypeProperty)
// Prints "Some value."
SomeStructure.storedTypeProperty = "Another value."
print(SomeStructure.storedTypeProperty)
// Prints "Another value."
print(SomeEnumeration.computedTypeProperty)
// Prints "6"
print(SomeClass.computedTypeProperty)
// Prints "27"
```

### 方法

方法即属于特定类型的函数。

#### 实例方法
```swift
class Counter {
    var count = 0
    func increment() {
        count += 1
    }
    func increment(by amount: Int) {
        count += amount
    }
    func reset() {
        count = 0
    }
}
```
```swift
let counter = Counter()
// the initial counter value is 0
counter.increment()
// the counter's value is now 1
counter.increment(by: 5)
// the counter's value is now 6
counter.reset()
// the counter's value is now 0
```
##### self属性
每个实例方法都隐含有一个`self`属性，指向实例自身。
比如上述`increment`方法可以定义为:
```swift
func increment() {
    self.count += 1
}
```
self一般可以省略，不过与本地变量或者参数重名的时候不可以省略。
```swift
struct Point {
    var x = 0.0, y = 0.0
    func isToTheRightOf(x: Double) -> Bool {
        return self.x > x
    }
}
let somePoint = Point(x: 4.0, y: 5.0)
if somePoint.isToTheRightOf(x: 1.0) {
    print("This point is to the right of the line where x == 1.0")
}
// Prints "This point is to the right of the line where x == 1.0"
```
##### 在实例方法中修改值类型
实例方法默认是不能就该值类型的值的。
除非在实例方法前用`mutating`申明可变。
```swift
struct Point {
    var x = 0.0, y = 0.0
    mutating func moveBy(x deltaX: Double, y deltaY: Double) {
        x += deltaX
        y += deltaY
    }
}
var somePoint = Point(x: 1.0, y: 1.0)
somePoint.moveBy(x: 2.0, y: 3.0)
print("The point is now at (\(somePoint.x), \(somePoint.y))")
// Prints "The point is now at (3.0, 4.0)"
```
需要注意的是，如果值类型是常量的话，即使是`mutating`方法也不能修改值。
```swift
let fixedPoint = Point(x: 3.0, y: 3.0) // 这里将报错
fixedPoint.moveBy(x: 2.0, y: 3.0)
// this will report an error
```
##### 在突变方法中给self赋值
突变方法就是定义为`mutating`的方法。
可以在突变方法中给self赋值。
```swift
struct Point {
    var x = 0.0, y = 0.0
    mutating func moveBy(x deltaX: Double, y deltaY: Double) {
        self = Point(x: x + deltaX, y: y + deltaY)
    }
}
```
```swift
enum TriStateSwitch {
    case off, low, high
    mutating func next() {
        switch self {
        case .off:
            self = .low
        case .low:
            self = .high
        case .high:
            self = .off
        }
    }
}
var ovenLight = TriStateSwitch.low
ovenLight.next()
// ovenLight is now equal to .high
ovenLight.next()
// ovenLight is now equal to .off
```
#### 类型方法
属于特定类型的方法。
```swift
class SomeClass {
    class func someTypeMethod() {
        // type method implementation goes here
    }
}
SomeClass.someTypeMethod()
```
略
### 下标
类，结构体与枚举可以定义下标。
#### 下标语法
```swift
subscript(index: Int) -> Int {
    get {
        // return an appropriate subscript value here
    }
    set(newValue) {
        // perform a suitable setting action here
    }
}
```
```swift
// 定义只读的下标
subscript(index: Int) -> Int {
    // return an appropriate subscript value here
}
```
下面看一个只读下标的实现。
```swift
struct TimesTable {
    let multiplier: Int
    subscript(index: Int) -> Int {
        return multiplier * index
    }
}
let threeTimesTable = TimesTable(multiplier: 3)
print("six times three is \(threeTimesTable[6])")
// Prints "six times three is 18"
```
#### 下标使用方法
```swift
var numberOfLegs = ["spider": 8, "ant": 6, "cat": 4]
numberOfLegs["bird"] = 2
```
#### 下标选项
下标可以接受任意类型参数，任意个数参数，也可以返回任意类型。
下标可以使用可变参数。
不可以使用in-out参数。
不可以提供参数默认值。
类和结构体可以提供多个下标实现。

```swift
struct Matrix {
    let rows: Int, columns: Int
    var grid: [Double]
    init(rows: Int, columns: Int) {
        self.rows = rows
        self.columns = columns
        grid = Array(repeating: 0.0, count: rows * columns)
    }
    func indexIsValid(row: Int, column: Int) -> Bool {
        return row >= 0 && row < rows && column >= 0 && column < columns
    }
    subscript(row: Int, column: Int) -> Double {
        get {
            assert(indexIsValid(row: row, column: column), "Index out of range")
            return grid[(row * columns) + column]
        }
        set {
            assert(indexIsValid(row: row, column: column), "Index out of range")
            grid[(row * columns) + column] = newValue
        }
    }
}
```
```swift
var matrix = Matrix(rows: 2, columns: 2)
```

![img](http://qefee.qiniudn.com/swift3study_subscript_options%20.png)

```swift
matrix[0, 1] = 1.5
matrix[1, 0] = 3.2
```

```swift
let someValue = matrix[2, 2] // 将激活断言，报错
// this triggers an assert, because [2, 2] is outside of the matrix bounds
```

### 继承

略。

#### 定义一个基础类

```swift
class Vehicle {
    var currentSpeed = 0.0
    var description: String {
        return "traveling at \(currentSpeed) miles per hour"
    }
    func makeNoise() {
        // do nothing - an arbitrary vehicle doesn't necessarily make a noise
    }
}
```
```swift
let someVehicle = Vehicle()
```
```swift
print("Vehicle: \(someVehicle.description)")
// Vehicle: traveling at 0.0 miles per hour
```
#### 子类
用:来继承。
语法
```swift
class SomeSubclass: SomeSuperclass {
    // subclass definition goes here
}
```
demo
```swift
class Bicycle: Vehicle {
    var hasBasket = false
}
```
```swift
let bicycle = Bicycle()
bicycle.hasBasket = true
```
```swift
bicycle.currentSpeed = 15.0
print("Bicycle: \(bicycle.description)")
// Bicycle: traveling at 15.0 miles per hour
```
```swift
// 子类还可以被继承
class Tandem: Bicycle {
    var currentNumberOfPassengers = 0
}
```
```swift
let tandem = Tandem()
tandem.hasBasket = true
tandem.currentNumberOfPassengers = 2
tandem.currentSpeed = 22.0
print("Tandem: \(tandem.description)")
// Tandem: traveling at 22.0 miles per hour
```
#### 重写
子类可以重写父类的
- 实例方法
- 类型方法
- 实例属性
- 类型属性
- 下标

关键字: `override`

##### 访问超类方法，属性与下标
- super.someMethod() 方法
- super.someProperty 属性
- super[someIndex] 下标

##### 重写方法
```swift
class Train: Vehicle {
    override func makeNoise() {
        print("Choo Choo")
    }
}
```
```swift
let train = Train()
train.makeNoise()
// Prints "Choo Choo"
```
##### 重写属性
###### 重写属性的Getters和Setters
```swift
class Car: Vehicle {
    var gear = 1
    override var description: String {
        return super.description + " in gear \(gear)"
    }
}
```
```swift
let car = Car()
car.currentSpeed = 25.0
car.gear = 3
print("Car: \(car.description)")
// Car: traveling at 25.0 miles per hour in gear 3
```
###### 重写属性监听器
```swift
class AutomaticCar: Car {
    override var currentSpeed: Double {
        didSet {
            gear = Int(currentSpeed / 10.0) + 1
        }
    }
}
```
```swift
let automatic = AutomaticCar()
automatic.currentSpeed = 35.0
print("AutomaticCar: \(automatic.description)")
// AutomaticCar: traveling at 35.0 miles per hour in gear 4
```
###### 阻止重写
用`final`关键字可以防止被重写。

### 初始化

初始化是指准备实例化类，结构体，枚举的过程。

#### 为存储属性设置初始值

类和结构体初始化的时候，必须为所有的存储属性设置合适的初始值。

可以在初始化方法中为存储属性设置初始值，也可以为存储属性设置默认值。

##### 初始化方法

即实例化的时候调用的方法。

语法:
```swift
init() {
    // perform some initialization here
}
```
demo:
```swift
struct Fahrenheit {
    var temperature: Double
    init() {
        temperature = 32.0
    }
}
var f = Fahrenheit()
print("The default temperature is \(f.temperature)° Fahrenheit")
// Prints "The default temperature is 32.0° Fahrenheit"
```
##### 属性默认值
在存储属性定义的时候直接可以赋与一个默认值。
```swift
struct Fahrenheit {
    var temperature = 32.0
}
```
#### 自定义初始化方法
##### 初始化参数
```swift
struct Celsius {
    var temperatureInCelsius: Double
    init(fromFahrenheit fahrenheit: Double) {
        temperatureInCelsius = (fahrenheit - 32.0) / 1.8
    }
    init(fromKelvin kelvin: Double) {
        temperatureInCelsius = kelvin - 273.15
    }
}
let boilingPointOfWater = Celsius(fromFahrenheit: 212.0)
// boilingPointOfWater.temperatureInCelsius is 100.0
let freezingPointOfWater = Celsius(fromKelvin: 273.15)
// freezingPointOfWater.temperatureInCelsius is 0.0
```
##### 参数名与实参标签
由于初始化函数的实参标签比较重要，所以如果没有设置的话，swift会自动设置。
```swift
struct Color {
    let red, green, blue: Double
    init(red: Double, green: Double, blue: Double) {
        self.red   = red
        self.green = green
        self.blue  = blue
    }
    init(white: Double) {
        red   = white
        green = white
        blue  = white
    }
}
```
```swift
let magenta = Color(red: 1.0, green: 0.0, blue: 1.0)
let halfGray = Color(white: 0.5)
```
##### 没有实参标签的初始化方法参数
```swift
struct Celsius {
    var temperatureInCelsius: Double
    init(fromFahrenheit fahrenheit: Double) {
        temperatureInCelsius = (fahrenheit - 32.0) / 1.8
    }
    init(fromKelvin kelvin: Double) {
        temperatureInCelsius = kelvin - 273.15
    }
    // 用_来忽视掉实参标签
    init(_ celsius: Double) {
        temperatureInCelsius = celsius
    }
}
let bodyTemperature = Celsius(37.0)
// bodyTemperature.temperatureInCelsius is 37.0
#### 可选类型的属性类型
如果一个属性逻辑上允许为nil，那么在初始化过程中可以不赋值。因为自动初始化为nil。
​```swift
class SurveyQuestion {
    var text: String
    var response: String?
    init(text: String) {
        self.text = text
    }
    func ask() {
        print(text)
    }
}
let cheeseQuestion = SurveyQuestion(text: "Do you like cheese?")
cheeseQuestion.ask()
// Prints "Do you like cheese?"
cheeseQuestion.response = "Yes, I do like cheese."
```
#### 初始化中常量属性的赋值
在初始化过程中可以给常量赋值，而一旦赋值就不再能修改。
```swift
class SurveyQuestion {
    let text: String
    var response: String?
    init(text: String) {
        self.text = text
    }
    func ask() {
        print(text)
    }
}
let beetsQuestion = SurveyQuestion(text: "How about beets?")
beetsQuestion.ask()
// Prints "How about beets?"
beetsQuestion.response = "I also like beets. (But not with cheese.)"
```
#### 默认初始化函数
swift为没有提供初始化函数的类和结构体提供了默认的初始化函数。
```swift
class ShoppingListItem {
    var name: String?
    var quantity = 1
    var purchased = false
}
var item = ShoppingListItem()
```
##### 结构体的默认初始化函数
```swift
struct Size {
    var width = 0.0, height = 0.0
}
let twoByTwo = Size(width: 2.0, height: 2.0)
```
##### 值类型的初始化方法代理
```swift
struct Size {
    var width = 0.0, height = 0.0
}
struct Point {
    var x = 0.0, y = 0.0
}
```
```swift
struct Rect {
    var origin = Point()
    var size = Size()
    init() {}
    init(origin: Point, size: Size) {
        self.origin = origin
        self.size = size
    }
    init(center: Point, size: Size) {
        let originX = center.x - (size.width / 2)
        let originY = center.y - (size.height / 2)
        self.init(origin: Point(x: originX, y: originY), size: size) // 用self.init使用初始化方法代理
    }
}
```
```swift
let basicRect = Rect()
// basicRect's origin is (0.0, 0.0) and its size is (0.0, 0.0)
```
```swift
let originRect = Rect(origin: Point(x: 2.0, y: 2.0),
                      size: Size(width: 5.0, height: 5.0))
// originRect's origin is (2.0, 2.0) and its size is (5.0, 5.0)
```
```swift
let centerRect = Rect(center: Point(x: 4.0, y: 4.0),
                      size: Size(width: 3.0, height: 3.0))
// centerRect's origin is (2.5, 2.5) and its size is (3.0, 3.0)
```
#### 类继承与初始化
##### 指定初始化方法与便利初始化方法
- 指定初始化方法，会初始化所有存储属性，包括继承来的，每个类至少有一个。
- 便利初始化方法，次要的，支持性的初始化方法。

##### 语法
指定初始化方法
```swift
init(parameters) {
    statements
}
```
便利初始化方法
```swift
// 关键字convenience
convenience init(parameters) {
    statements
}
```
##### 类类型的初始化方法代理
规则:
1. 指定初始化方法必须调用直接继承的类的指定初始化方法。
2. 便利初始化方法必须调用本类的另一个初始化方法。
3. 便利初始化方法必须最终调用一个指定初始化方法。

简单来说就是：
- 指定初始化方法必须向上委托
- 便利初始化方法必须横向委托

![img](http://qefee.qiniudn.com/swift3study_initialization_initializer_delegation.png)


![img](http://qefee.qiniudn.com/swift3study_initialization_initializer_delegation1.png)

##### 二阶初始化
略
##### 初始化函数的继承与重写
```swift
class Vehicle {
    var numberOfWheels = 0
    var description: String {
        return "\(numberOfWheels) wheel(s)"
    }
}
```
```swift
let vehicle = Vehicle()
print("Vehicle: \(vehicle.description)")
// Vehicle: 0 wheel(s)
```
```swift
class Bicycle: Vehicle {
    // 重写
    override init() {
        super.init()
        numberOfWheels = 2
    }
}
```
```swift
let bicycle = Bicycle()
print("Bicycle: \(bicycle.description)")
// Bicycle: 2 wheel(s)
```
### 反初始化方法
略(挺复杂的)

