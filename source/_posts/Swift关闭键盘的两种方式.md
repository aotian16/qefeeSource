title: Swift关闭键盘的两种方式
categories: qefee
tags: qefee
date: 2016-05-13 15:45:48

---

<!--head-->

学习中，记录下。

**from** [Swift关闭键盘的两种方式](http://qefee.com/2016/05/13/Swift%E5%85%B3%E9%97%AD%E9%94%AE%E7%9B%98%E7%9A%84%E4%B8%A4%E7%A7%8D%E6%96%B9%E5%BC%8F/)

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
