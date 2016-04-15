title: TypeScript入门-语法篇

categories: typescript

tags: typescript

date: 2016-01-21 10:12:16

---



自学笔记, 基于官方教程上的代码, 添加注释.

**官方教程:**

[Handbook](http://www.typescriptlang.org/Handbook)

------



[TOC]

<!--head-->



<!--more-->



# 基本类型 Basic Types

``` typescript

// 1. Boolean,布尔
var isDone: boolean = false;

console.log(isDone);

// 2. Number,数值
var height: number = 6;

console.log(height);

// 3. String,字符串
var name: string = "bob"; // 可以双引号
var name1: string = 'tom'; // 也可以单引号

console.log(name);
console.log(name1);

// 4. Array,数组
var list:number[] = [1, 2, 3]; // 写法1
var list1:Array<number> = [1, 2, 3]; // 写法2

console.log(list);
console.log(list1);

// 5. Enum,枚举
enum Color {Red, Green, Blue}
var c: Color = Color.Green;

console.log(c); // 获取值
console.log(Color[c]); // 获取键, string
console.log(Color["Green"]); // 获取值

// # enum的值默认从0开始递增, 也可以自己指定
enum Color1 {Red = 1, Green = 2, Blue = 4}
var c1: Color1 = Color1.Green;

console.log(c1); // 获取值

// 6. Any,任意对象
// # 需要小心使用, 会改变数据类型
var anylist:any[] = [1, true, "free"];

anylist[2] = 100;

console.log(anylist);

// 7. Void,方法不返回值

function warnUser(): void {
    console.log("This is my warning message");
}

warnUser();
```

# 接口 Interfaces

## 第一个接口 Our First Interface

``` typescript
interface LabelledValue {
    label: string;
}

function printLabel(labelledObj: LabelledValue) {
    console.log(labelledObj.label);
}

var myObj = {label: "only label"};
var myObj1 = {size: 10, label: "Size 10 Object"};

// # 接口是鸭子类型, 只要是有label的对象都可以传进去
printLabel(myObj);
printLabel(myObj1);
```

## 可选属性 Optional Properties

``` typescript
interface SquareConfig {
    color?: string; // 用?来表示可选属性
    width?: number;
}

function createSquare(config: SquareConfig): {color: string; area: number} {
    var newSquare = {color: "white", area: 100}; // 默认值
    if (config.color) {
        newSquare.color = config.color;
    }
    if (config.width) {
        newSquare.area = config.width * config.width;
    }
    return newSquare;
}

var mySquare = createSquare({color: "black"});
var mySquare1 = createSquare({width: 20});

console.log(mySquare);
console.log(mySquare1);
```

## 函数类型 Function Types

``` typescript
interface SearchFunc {
    (source: string, subString: string): boolean; // 表示一个接受2个string参数, 返回bool的函数
}

var mySearch: SearchFunc;
mySearch = function(source: string, subString: string): boolean {
    var result = source.search(subString);
    if (result == -1) {
        return false;
    }
    else {
        return true;
    }
};

var result: boolean = mySearch("test", "te");

console.log(result);
```

## 数组类型 Array Types

``` typescript
interface StringArray {
    [index: number]: string; // 表示string数组
}

var myArray: StringArray;
myArray = ["Bob", "Fred"];

console.log(myArray["0"]);
console.log(myArray["1"]);
```

## 类类型 Class Types

``` typescript
interface ClockStatic {
    new (hour: number, minute: number);
}

// # 这里会报错. 因为只有实例部分才检查, 而构造函数是静态部分
//class Clock implements ClockStatic  {
//    currentTime: Date;
//    constructor(h: number, m: number) { }
//}

class Clock {
    currentTime: Date;
    constructor(h: number, m: number) { }
}

// # 这里需要注意
// # 我们直接使用class,这样就会检查签名
var cs: ClockStatic = Clock;
var newClock: ClockStatic = new cs(7, 30);

console.log(newClock);
```

## 继承接口 Extending Interfaces

``` typescript
interface Shape {
    color: string;
}

interface PenStroke {
    penWidth: number;
}

interface Square extends Shape, PenStroke { // 使用extends继承接口
    sideLength: number;
}

var square = <Square>{};
square.color = "blue";
square.sideLength = 10;
square.penWidth = 5.0;
```

## 混合类型 Hybrid Types

``` typescript
interface Counter {
    (start: number): string;
    interval: number;
    reset(): void;
}

var c: Counter;
c(10); // c可以当函数
c.reset(); // c也可以当对象
c.interval = 5.0; // c的属性
```



# 类 Classes

## 类 Classes

``` typescript
class Greeter {
    greeting: string; // 属性
    constructor(message: string) { // 构造函数
        this.greeting = message;
    }
    greet(): string { // 方法
        return "Hello, " + this.greeting;
    }
}

var greeter = new Greeter("world");
var result = greeter.greet();

console.log(result);
```

## 继承 Inheritance

``` typescript
class Animal {
    name:string;
    constructor(theName: string) { this.name = theName; }
    move(meters: number = 0) {
        console.log(this.name + " moved " + meters + "m.");
    }
}

class Snake extends Animal { // 使用extends关键字来继承
    constructor(name: string) { super(name); }
    move(meters = 5) {
        console.log("Slithering...");
        super.move(meters);
    }
}

class Horse extends Animal {
    constructor(name: string) { super(name); }
    move(meters = 45) {
        console.log("Galloping...");
        super.move(meters);
    }
}

var sam = new Snake("Sammy the Python");
var tom: Animal = new Horse("Tommy the Palomino");

sam.move();
tom.move(34);
```

## 公/私 修饰器 Private/Public modifiers

### 默认公有 Public by default

``` typescript
class Animal {
    private name:string; // 除非指定为private, 否则默认是public
    constructor(theName: string) { this.name = theName; }
    move(meters: number) {
        alert(this.name + " moved " + meters + "m.");
    }
}
```

### 理解私有 Understanding private

``` typescript
class Animal {
    private name:string;
    constructor(theName: string) { this.name = theName; }
}

class Rhino extends Animal {
    constructor() { super("Rhino"); }
}

class Employee {
    private name:string;
    constructor(theName: string) { this.name = theName; }
}

var animal = new Animal("Goat");
var rhino = new Rhino();
var employee = new Employee("Bob");

animal = rhino;
//animal = employee; // # 这里会报错. 虽然Animal和Employee属性名一致, 但是私有属性来源不一致

class A {
    public name: string;
    echo() {}
}

class B {
    public name: string;
    echo() {}
}

var a = new A();
var b = new B();

a = b; // 公有属性则没有关系
```

### 参数属性 Parameter properties

``` typescript
class Animal {
    constructor(private name: string) { } // 可以在构造方法用private直接指定属性,省去一步
    move(meters: number) {
        console.log(this.name + " moved " + meters + "m.");
    }
}

var dog = new Animal("dog");

dog.move(3);
```

## 访问器 Accessors

``` typescript
var passcode = "secret passcode";

class Employee {
    private _fullName: string;

    get fullName(): string { // 读属性. # 需要编译版本为ECMAScript 5
        return this._fullName;
    }

    set fullName(newName: string) { // 写属性. # 需要编译版本为ECMAScript 5
        if (passcode && passcode == "secret passcode") {
            this._fullName = newName;
        }
        else {
            console.log("Error: Unauthorized update of employee!");
        }
    }
}

var employee = new Employee();
employee.fullName = "Bob Smith";
if (employee.fullName) {
    console.log(employee.fullName);
}
```

## 静态属性 Static Properties

``` typescript
class Grid {
    static origin = {x: 0, y: 0}; // 用static定义静态属性
    calculateDistanceFromOrigin(point: {x: number; y: number;}) {
        var xDist = (point.x - Grid.origin.x);
        var yDist = (point.y - Grid.origin.y);
        return Math.sqrt(xDist * xDist + yDist * yDist) / this.scale;
    }
    constructor (public scale: number) { }
}

var grid1 = new Grid(1.0);  // 1x scale
var grid2 = new Grid(5.0);  // 5x scale

console.log(grid1.calculateDistanceFromOrigin({x: 10, y: 10}));
console.log(grid2.calculateDistanceFromOrigin({x: 10, y: 10}));
```

## Advanced Techniques

todo

# 模块 Modules

``` typescript
module Validation { // 用module关键字定义模块
    export interface StringValidator { // 用export关键字导出模块内的东西
        isAcceptable(s: string): boolean;
    }

    var lettersRegexp = /^[A-Za-z]+$/;
    var numberRegexp = /^[0-9]+$/;

    export class LettersOnlyValidator implements StringValidator {
        isAcceptable(s: string) {
            return lettersRegexp.test(s);
        }
    }

    export class ZipCodeValidator implements StringValidator {
        isAcceptable(s: string) {
            return s.length === 5 && numberRegexp.test(s);
        }
    }
}

// Some samples to try
var strings = ['Hello', '98052', '101'];
// Validators to use
var validators: { [s: string]: Validation.StringValidator; } = {}; // 使用模块时候要带上模块名
validators['[ZIP code]'] = new Validation.ZipCodeValidator();
validators['[Letters only]'] = new Validation.LettersOnlyValidator();
// Show whether each string passed each validator
strings.forEach(s => {
    for (var name in validators) {
        console.log('"' + s + '" ' + (validators[name].isAcceptable(s) ? ' matches ' : ' does not match ') + name);
    }
});
```

## 分割文件 Splitting Across Files

// todo 文件可以写在不同的文件中, 通过命令行统计导出一个模块

## 外部模块 Going External

**外部模块, 这是`node.js` 和 `require.js`用的.**

`Validation.ts`

``` typescript
export interface StringValidator {
    isAcceptable(s: string): boolean;
}
```

`LettersOnlyValidator.ts`

``` typescript
import validation = require('./Validation');
var lettersRegexp = /^[A-Za-z]+$/;
export class LettersOnlyValidator implements validation.StringValidator {
    isAcceptable(s: string) {
        return lettersRegexp.test(s);
    }
}
```

`ZipCodeValidator.ts`

``` typescript
import validation = require('./Validation');
var numberRegexp = /^[0-9]+$/;
export class ZipCodeValidator implements validation.StringValidator {
    isAcceptable(s: string) {
        return s.length === 5 && numberRegexp.test(s);
    }
}
```

`Test.ts`

``` typescript
import validation = require('./Validation') // 用import require导入外部模块
import zip = require('./ZipCodeValidator');
import letters = require('./LettersOnlyValidator');

// Some samples to try
var strings = ['Hello', '98052', '101'];
// Validators to use
var validators: { [s: string]: validation.StringValidator; } = {};
validators['ZIP code'] = new zip.ZipCodeValidator();
validators['Letters only'] = new letters.LettersOnlyValidator();
// Show whether each string passed each validator
strings.forEach(s => {
    for (var name in validators) {
        console.log('"' + s + '" ' + (validators[name].isAcceptable(s) ? ' matches ' : ' does not match ') + name);
    }
});
```

## Export =

``` typescript
import validation = require('./Validation');
var lettersRegexp = /^[A-Za-z]+$/;
class LettersOnlyValidator implements validation.StringValidator {
    isAcceptable(s: string) {
        return lettersRegexp.test(s);
    }
}
export = LettersOnlyValidator; // 可以用`export ='导出
```

## 别名 Alias

``` typescript
module Shapes {
    export module Polygons {
        export class Triangle { }
        export class Square { }
    }
}

import polygons = Shapes.Polygons;
// sq就是Shapes.Polygons.Square的别名, 
// 等效于 'new Shapes.Polygons.Square()'
var sq = new polygons.Square(); 
```

## 动态加载和其他加载方式 Optional Module Loading and Other Advanced Loading Scenarios

// todo 动态加载

## 使用其他js库 Working with Other JavaScript Libraries

// todo

## 模块的陷阱 Pitfalls of Modules

// todo

# 函数 Functions

## 函数 Functions

// todo

## 函数类型 Function Types

``` typescript
function add(x: number, y: number): number {
    return x+y;
}

var myAdd = function(x: number, y: number): number { return x+y; };

var r1 = add(1,2);
var r2 = myAdd(2,3);

console.log(r1,r2);
```

## 可选参数和默认参数 Optional and Default Parameters

``` typescript
function foo1(x: number, y?: number): number { // 用?表示参数可选
    if(y) {
        return x + y;
    } else {
        return x;
    }
}

var r1 = foo1(1);
var r2 = foo1(1,2);

console.log(r1, r2);

function foo2(x: number, y = 3): number { // 用= xx表示参数有默认值
    if(y) {
        return x + y;
    } else {
        return x;
    }
}

var r3 = foo2(1);
var r4 = foo2(1,2);

console.log(r3, r4);
```

## 可变参数 Rest Parameters

``` typescript
function foo1(x: number, ...arr: number[]): number { // 用...表示参数可变
    var sum = x;
    for(var i in arr) {
        sum += arr[i];
    }
    return sum;
}

var r1 = foo1(1);
var r2 = foo1(1,2);
var r3 = foo1(1,2,3);

console.log(r1, r2, r3);
```

## Lambdas和this Lambdas and using 'this'

``` typescript
//var deck = {
//    suits: ["hearts", "spades", "clubs", "diamonds"],
//    cards: Array(52),
//    createCardPicker: function() {
//        return function() {
//            var pickedCard = Math.floor(Math.random() * 52);
//            var pickedSuit = Math.floor(pickedCard / 13);
//
//            return {suit: this.suits[pickedSuit], card: pickedCard % 13};
//        }
//    }
//}
//
//var cardPicker = deck.createCardPicker();
//var pickedCard = cardPicker();
//
//console.log("card: " + pickedCard.card + " of " + pickedCard.suit);

// 上面的写法会报错,因为this指向不正确

// 我们使用lambda重新实现

var deck = {
    suits: ["hearts", "spades", "clubs", "diamonds"],
    cards: Array(52),
    createCardPicker: function() {
        // Notice: the line below is now a lambda, 
        // allowing us to capture 'this' earlier
        return () => { // 这里我们用了lambda, 会自动俘获this
            var pickedCard = Math.floor(Math.random() * 52);
            var pickedSuit = Math.floor(pickedCard / 13);

            return {suit: this.suits[pickedSuit], card: pickedCard % 13};
        }
    }
}

var cardPicker = deck.createCardPicker();
var pickedCard = cardPicker();

console.log("card: " + pickedCard.card + " of " + pickedCard.suit);
```

## 重载 Overloads

``` typescript
var suits = ["hearts", "spades", "clubs", "diamonds"];

function pickCard(x: {suit: string; card: number; }[]): number; // 可以重载, 这里是申明
function pickCard(x: number): {suit: string; card: number; }; // 可以重载, 这里是申明
function pickCard(x): any { // 可以重载, 这里是实现
    // Check to see if we're working with an object/array
    // if so, they gave us the deck and we'll pick the card
    if (typeof x == "object") {
        var pickedCard = Math.floor(Math.random() * x.length);
        return pickedCard;
    }
    // Otherwise just let them pick the card
    else if (typeof x == "number") {
        var pickedSuit = Math.floor(x / 13);
        return { suit: suits[pickedSuit], card: x % 13 };
    }
}

var myDeck = [{ suit: "diamonds", card: 2 }, { suit: "spades", card: 10 }, { suit: "hearts", card: 4 }];
var pickedCard1 = myDeck[pickCard(myDeck)];
alert("card: " + pickedCard1.card + " of " + pickedCard1.suit);

var pickedCard2 = pickCard(15);
alert("card: " + pickedCard2.card + " of " + pickedCard2.suit);
```

# 泛型 Generics

## 泛型 Hello World of Generics

``` typescript
function identity<T>(arg: T): T { // T是泛型
    return arg;
}
```

## 泛型变量 Working with Generic Type Variables

``` typescript
function loggingIdentity<T>(arg: Array<T>): Array<T> {
    // console.log(arg.len);  // 错误, 必需假定arg是any类型, 不能使用没有的属性和方法
    console.log(arg.length);  // 正确, 数组有length属性
    return arg;
}
```

## 泛型类型 Generic Types

``` typescript
function identity<T>(arg: T): T {
    return arg;
}

var myIdentity: <T>(arg: T)=>T = identity;    // 泛型类型表示 1
var myIdentity1: {<T>(arg: T): T} = identity; // 泛型类型表示 2
```

## 泛型类 Generic Classes

``` typescript
class GenericNumber<T> {
    zeroValue: T;
    add: (x: T, y: T) => T;
}

// number
var myGenericNumber = new GenericNumber<number>();
myGenericNumber.zeroValue = 0;
myGenericNumber.add = function(x, y) { return x + y; };

// string
var stringNumeric = new GenericNumber<string>();
stringNumeric.zeroValue = "----";
stringNumeric.add = function(x, y) { return x + y; };

console.log(myGenericNumber.add(myGenericNumber.zeroValue, 1));
console.log(stringNumeric.add(stringNumeric.zeroValue, "test"));
```

## 泛型限制 Generic Constraints

``` typescript
interface Lengthwise {
    length: number;
}

function loggingIdentity<T extends Lengthwise>(arg: T): T { // 限制为Lengthwise子类型
    console.log(arg.length);  // 这样就OK了
    return arg;
}
```

### 在泛型限制中使用泛型参数 Using Type Parameters in Generic Constraints

``` typescript
class Findable<U> {

}

function find<T>(n: T, s: Findable<T>) { // 在泛型中使用泛型
    // ...
}
```

### Using Class Types in Generics

``` typescript
class BeeKeeper {
    hasMask: boolean;
}

class ZooKeeper {
    nametag: string;
}

class Animal {
    numLegs: number;
}

class Bee extends Animal {
    keeper: BeeKeeper;
}

class Lion extends Animal {
    keeper: ZooKeeper;
}

function findKeeper<A extends Animal, K> (a: {new(): A;
    prototype: {keeper: K}}): K {

    return a.prototype.keeper;
}

findKeeper(Lion).nametag;  // typechecks! TODO 运行报错, 没看懂
```

# 常见问题 Common Errors

todo

# 混合 Mixins

todo 有点像多重继承

# 声明合并 Declaration Merging

todo 相当于分开写

# 类型推断 Type Inference

todo

# 类型比较 Type Compatibility

todo

# Writing .d.ts files

todo



<!--body-->