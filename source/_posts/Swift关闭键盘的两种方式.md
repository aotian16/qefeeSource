title: Swift关闭键盘的两种方式
categories: qefee
tags: qefee
date: 2016-05-13 15:45:48

---

<!--head-->

学习中，记录下。

### 方法一

对单个的`UITextField`调用`resignFirstResponder`方法， 使其失去第一响应者

```swift
sender.resignFirstResponder()
```



### 方法二

对`UIViewController`，重写`touchesBegan`， 并调用`endEditing`方法

```swift
override func touchesBegan(touches: Set<UITouch>, withEvent event: UIEvent?) {
        view.endEditing(true)
    }
```

### 参考

* [Close iOS Keyboard by touching anywhere using Swift](http://stackoverflow.com/questions/24126678/close-ios-keyboard-by-touching-anywhere-using-swift)



<!--more-->



<!--body-->
