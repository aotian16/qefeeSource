title: ios中使用SegmentedControl来切换视图
categories: ios
tags: SegmentedControl
date: 2016-03-11 14:55:36

---

<!--head-->

[TOC]

# From

[ios中使用SegmentedControl来切换视图](http://qefee.com/2016/03/11/ios%E4%B8%AD%E4%BD%BF%E7%94%A8SegmentedControl%E6%9D%A5%E5%88%87%E6%8D%A2%E8%A7%86%E5%9B%BE/)

# 效果图

![run](http://qefee.qiniudn.com/20160311_ios%E4%B8%AD%E4%BD%BF%E7%94%A8SegmentedControl%E6%9D%A5%E5%88%87%E6%8D%A2%E8%A7%86%E5%9B%BE_run.gif)

<!--more-->



<!--body-->



# 设计图

![run](http://qefee.qiniudn.com/20160311_ios%E4%B8%AD%E4%BD%BF%E7%94%A8SegmentedControl%E6%9D%A5%E5%88%87%E6%8D%A2%E8%A7%86%E5%9B%BE_design.png)

# 结构与原理

## 视图结构

共有3个ViewController

1. A 父视图
2. B 子视图
3. C 子视图

## 切换视图原理

A包含上下两个部分, 

上面就是`SegmentedControl`, 来控制视图切换

下面的部分用来展示B, C子视图.



点击`SegmentedControl`时候通过addView和removeView来实现视图切换.

另外, 为了好看, 加了翻页的动画效果.

# 代码

注释比较多了, 应该一看就明白.
``` swift
import UIKit

class SegmentViewController: UIViewController {

    /// 容器view
    @IBOutlet weak var containerView: UIView!
    
    var leftViewController: LeftViewController!
    var rightViewController: RightViewController!
    
    
    override func viewDidLoad() {
        super.viewDidLoad()
        // Do any additional setup after loading the view, typically from a nib.
        
        if let sb = storyboard {
            leftViewController = sb.instantiateViewControllerWithIdentifier("leftViewController") as! LeftViewController

            switchViewController(from: nil, to: leftViewController)
        } else {
            print("storyboard is nil")
        }
    }
    
    func switchViewController(from fromVC: UIViewController?, to toVC: UIViewController?) {
        if let from = fromVC {
            from.willMoveToParentViewController(nil) // 通知from即将从父ViewController移除
            from.view.removeFromSuperview() // 移除from的view
            from.removeFromParentViewController() // 移除from的ViewController
        } else {
            print("fromVC is nil")
        }
        
        if let to = toVC {
            self.addChildViewController(to) // 添加to的ViewController到父ViewController
            to.view.frame = CGRectMake(0, 0, containerView.frame.width, containerView.frame.height) // 计算视图大小
            self.containerView.insertSubview(to.view, atIndex: 0) // 添加to的view到容器view
            to.didMoveToParentViewController(self) // 通知to已经添加到父ViewController
        } else {
            print("toVC is nil")
        }
    }

    override func didReceiveMemoryWarning() {
        super.didReceiveMemoryWarning()
        // Dispose of any resources that can be recreated.
        
        removeViewController()
    }
    
    /**
     当收到内存警告时候, 移除不必要的ViewController
     */
    func removeViewController() {
        if let leftVC = leftViewController {
            if let _ = leftVC.parentViewController {
                print("leftVC is using")
            } else {
                print("set leftVC = nil")
                leftViewController = nil
            }
        }
        
        if let rightVC = rightViewController {
            if let _ = rightVC.parentViewController {
                print("rightVC is using")
            } else {
                print("set rightVC = nil")
                rightViewController = nil
            }
        }
    }


    @IBAction func onSegmentValueChanged(sender: UISegmentedControl) {
        UIView.beginAnimations("xxx", context: nil)
        UIView.setAnimationDuration(0.4)
        UIView.setAnimationCurve(.EaseInOut)
        
        switch sender.selectedSegmentIndex {
        case 0:
            UIView.setAnimationTransition(.FlipFromRight, forView: self.containerView, cache: true)
            
            if let leftVC = leftViewController {
                switchViewController(from: rightViewController, to: leftVC)
            } else {
                if let sb = storyboard {
                    leftViewController = sb.instantiateViewControllerWithIdentifier("leftViewController") as! LeftViewController
                    switchViewController(from: rightViewController, to: leftViewController)
                } else {
                    print("storyboard is nil")
                }
            }
        default:
            UIView.setAnimationTransition(.FlipFromLeft, forView: self.containerView, cache: true)
            
            if let rightVC = rightViewController {
                switchViewController(from: leftViewController, to: rightVC)
            } else {
                if let sb = storyboard {
                    rightViewController = sb.instantiateViewControllerWithIdentifier("rightViewController") as! RightViewController
                    switchViewController(from: leftViewController, to: rightViewController)
                } else {
                    print("storyboard is nil")
                }
            }
        }
        
        UIView.commitAnimations()
    }
}


```

