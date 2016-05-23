title: swift简单动画demo
categories: qefee
tags: qefee
date: 2016-05-15 23:06:06

---

**from:**
[swift简单动画demo](http://qefee.com/2016/05/15/swift%E7%AE%80%E5%8D%95%E5%8A%A8%E7%94%BBdemo/)

<!--head-->

# 动画GIF

| No.  | name(cn) | name(en)    |                   gif                    |
| :--: | -------- | ----------- | :--------------------------------------: |
|  1   | 缩放动画     | Scale       | ![scale](https://github.com/aotian16/Blog/blob/master/Study/Dev/Swift/swift%E7%AE%80%E5%8D%95%E5%8A%A8%E7%94%BBdemo/animScale.gif?raw=true) |
|  2   | 位移动画     | Translation | ![scale](https://github.com/aotian16/Blog/blob/master/Study/Dev/Swift/swift%E7%AE%80%E5%8D%95%E5%8A%A8%E7%94%BBdemo/animTranslation.gif?raw=true) |
|  3   | 旋转动画     | Rotation    | ![scale](https://github.com/aotian16/Blog/blob/master/Study/Dev/Swift/swift%E7%AE%80%E5%8D%95%E5%8A%A8%E7%94%BBdemo/animRotation.gif?raw=true) |
|  4   | 颜色动画     | Color       | ![scale](https://github.com/aotian16/Blog/blob/master/Study/Dev/Swift/swift%E7%AE%80%E5%8D%95%E5%8A%A8%E7%94%BBdemo/animColor.gif?raw=true) |
|  5   | 弹簧动画     | Spring      | ![scale](https://github.com/aotian16/Blog/blob/master/Study/Dev/Swift/swift%E7%AE%80%E5%8D%95%E5%8A%A8%E7%94%BBdemo/animSpring.gif?raw=true) |
|  6   | 组合动画     | Concat      | ![scale](https://github.com/aotian16/Blog/blob/master/Study/Dev/Swift/swift%E7%AE%80%E5%8D%95%E5%8A%A8%E7%94%BBdemo/animConcat.gif?raw=true) |

<!--more-->

# 代码

```swift
import UIKit

class ViewController: UIViewController {
    
    
    
    var rect: UIView!
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        initrectView()
    }
    
    var i: Int = 0
    
    @IBAction func onNextButtonClick(sender: UIButton) {
        switch i%6 {
        case 0:
            showScaleAnim() // 1. 缩放动画
        case 1:
            showTranslationAnim() // 2. 位移动画
        case 2:
            showRotationAnim() // 3. 旋转动画
        case 3:
            showColorAnim() // 4. 颜色动画
        case 4:
            showSpringAnim() // 5. 弹簧动画
        default:
            showConcatAnim() // 6. 组合动画
        }
        
        i += 1
    }
    
    
    /**
     初始化动画对象
     */
    func initrectView() {
        let rectFrame = CGRectMake(50, 50, 100, 100)
        rect = UIView(frame: rectFrame)
        
        rect.backgroundColor = UIColor.cyanColor()
        
        self.view.addSubview(rect)
    }
    
    func resumeRectView() -> Void {
        rect.transform = CGAffineTransformIdentity
    }
    
    /**
     普通动画. 缩放
     */
    func showScaleAnim() {
        
        let duration: NSTimeInterval = 3 // 动画持续时间
        let delay: NSTimeInterval = 0 // 动画延迟时间
        let options: UIViewAnimationOptions = UIViewAnimationOptions.CurveEaseInOut // 动画选项
        let animations: () -> Void = {() -> Void in
            self.rect.transform = CGAffineTransformMakeScale(2, 2) // 缩放动画, x 放大 2, y 放大 2
        }
        UIView.animateWithDuration(duration, delay: delay, options: options, animations: animations) { (finish) -> Void in
            self.resumeRectView()
            print(finish)
        }
    }
    
    /**
     普通动画. 位移
     */
    func showTranslationAnim() {
        let duration: NSTimeInterval = 3 // 动画持续时间
        let delay: NSTimeInterval = 0 // 动画延迟时间
        let options: UIViewAnimationOptions = UIViewAnimationOptions.CurveEaseInOut // 动画选项
        let animations: () -> Void = {() -> Void in
            self.rect.transform = CGAffineTransformMakeTranslation(0, 100) // 位移动画, x 移动 0, y 移动 100
        }
        UIView.animateWithDuration(duration, delay: delay, options: options, animations: animations) { (finish) -> Void in
            self.resumeRectView()
            print(finish)
        }
    }
    
    /**
     普通动画. 旋转
     */
    func showRotationAnim() {
        let duration: NSTimeInterval = 3 // 动画持续时间
        let delay: NSTimeInterval = 0 // 动画延迟时间
        let options: UIViewAnimationOptions = UIViewAnimationOptions.CurveEaseInOut // 动画选项
        let animations: () -> Void = {() -> Void in
            self.rect.transform = CGAffineTransformMakeRotation(CGFloat(M_PI_2)) // 旋转动画, 旋转pi
        }
        UIView.animateWithDuration(duration, delay: delay, options: options, animations: animations) { (finish) -> Void in
            self.resumeRectView()
            print(finish)
        }
    }
    
    /**
     普通动画. 颜色
     */
    func showColorAnim() {
        let duration: NSTimeInterval = 3 // 动画持续时间
        let delay: NSTimeInterval = 0 // 动画延迟时间
        let options: UIViewAnimationOptions = UIViewAnimationOptions.CurveEaseInOut // 动画选项
        let animations: () -> Void = {() -> Void in
            self.rect.backgroundColor = UIColor.greenColor() // 颜色动画, 变色为绿色
        }
        UIView.animateWithDuration(duration, delay: delay, options: options, animations: animations) { (finish) -> Void in
            self.resumeRectView()
            print(finish)
        }
    }
    
    /**
     弹簧动画, 一个球变大
     */
    func showSpringAnim() {
        let duration: NSTimeInterval = 3 // 动画持续时间
        let delay: NSTimeInterval = 0 // 动画延迟时间
        let damping: CGFloat = 0.3 // 阻尼. 越大感觉越有粘性
        let velocity: CGFloat = 0 // 初速度
        let options: UIViewAnimationOptions = UIViewAnimationOptions.CurveEaseInOut // 动画选项
        let animations: () -> Void = {() -> Void in
            self.rect.transform = CGAffineTransformMakeTranslation(0, 150) // 位移动画
        }
        
        UIView.animateWithDuration(duration, delay: delay, usingSpringWithDamping: damping, initialSpringVelocity: velocity, options: options, animations: animations) { (finish) -> Void in
            self.resumeRectView()
            print(finish)
        }
    }
    
    /**
     组合动画. 缩放 + 位移 + 颜色
     */
    func showConcatAnim() {
        let duration: NSTimeInterval = 3 // 动画持续时间
        let delay: NSTimeInterval = 0 // 动画延迟时间
        let options: UIViewAnimationOptions = UIViewAnimationOptions.CurveEaseInOut // 动画选项
        let animations: () -> Void = {() -> Void in
            
            let anim1 = CGAffineTransformMakeScale(2, 2) // 缩放动画, x 放大 2, y 放大 2
            let anim2 = CGAffineTransformMakeTranslation(150, 50) // 位移动画, x 移动 150, y 移动 50
            
            let concatAnim = CGAffineTransformConcat(anim1, anim2) // 组合动画, 缩放和位移动画一起组合成新的动画
            
            self.rect.backgroundColor = UIColor.greenColor() // 颜色动画
            
            self.rect.transform = concatAnim
        }
        UIView.animateWithDuration(duration, delay: delay, options: options, animations: animations) { (finish) -> Void in
            self.resumeRectView()
            print(finish)
        }
    }
}
```



<!--body-->
