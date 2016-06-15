title: android的Touch事件传递机制学习
categories: android
tags: android
date: 2016-06-07 20:39:04

---

<!--head-->

[TOC]

**原文地址**

[android的Touch事件传递机制学习](http://qefee.com/2016/06/07/android%E7%9A%84Touch%E4%BA%8B%E4%BB%B6%E4%BC%A0%E9%80%92%E6%9C%BA%E5%88%B6%E5%AD%A6%E4%B9%A0/)

# Reffer Methods

ViewGroup子类的touch事件相关方法：

| No.  | Name                  | Detail                         |
| :--: | --------------------- | ------------------------------ |
|  1   | dispatchTouchEvent    | 分发点击事件。true表示自己处理事件。           |
|  2   | onInterceptTouchEvent | 拦截点击事件。true表示拦截事件，并不再往子view传播。 |
|  3   | onTouchEvent          | 处理点击事件。true表示已处理事件。            |

# Page Image

![PageImage](https://github.com/aotian16/Blog/blob/master/Study/Dev/Android/android%E7%9A%84Touch%E4%BA%8B%E4%BB%B6%E4%BC%A0%E9%80%92%E6%9C%BA%E5%88%B6%E5%AD%A6%E4%B9%A0/PageImage.png?raw=true)

<!--more-->

<!--body-->

# Code

## MainActivity.java

```java
package com.qefee.pj.testtouchevent;

import android.content.Context;
import android.util.AttributeSet;
import android.util.Log;
import android.view.MotionEvent;
import android.widget.LinearLayout;

public class MyLinearLayout extends LinearLayout {

    public static int id = 0;

    private int currentId = id;

    /**
     * log tag for MyLinearLayout
     */
    private static final String TAG = "MyLinearLayout";

    public MyLinearLayout(Context context) {
        super(context);
        currentId = id++;
    }

    public MyLinearLayout(Context context, AttributeSet attrs) {
        super(context, attrs);
        currentId = id++;
    }

    public MyLinearLayout(Context context, AttributeSet attrs, int defStyleAttr) {
        super(context, attrs, defStyleAttr);
        currentId = id++;
    }

    @Override
    public boolean dispatchTouchEvent(MotionEvent ev) {
        int action = ev.getAction();
        Log.i(TAG, currentId + " dispatchTouchEvent: action = " + action);
        boolean b = super.dispatchTouchEvent(ev);
//        b = currentId == 1;
        Log.i(TAG, currentId + " dispatchTouchEvent: return = " + b);
        return b;
    }

    @Override
    public boolean onInterceptTouchEvent(MotionEvent ev) {
        int action = ev.getAction();
        Log.i(TAG, currentId + " onInterceptTouchEvent: action = " + action);
        boolean b = super.onInterceptTouchEvent(ev);
//        b = currentId == 1;
        Log.i(TAG, currentId + " onInterceptTouchEvent: return = " + b);
        return b;
    }

    @Override
    public boolean onTouchEvent(MotionEvent ev) {
        int action = ev.getAction();
        Log.i(TAG, currentId + " onTouchEvent: action = " + action);
        boolean b = super.onTouchEvent(ev);
//        b = currentId == 1;
        Log.i(TAG, currentId + " onTouchEvent: return = " + b);
        return b;
    }
}
```

## MyLinearLayout.java

```java
package com.qefee.pj.testtouchevent;

import android.content.Context;
import android.util.AttributeSet;
import android.util.Log;
import android.view.MotionEvent;
import android.widget.LinearLayout;

/**
 * MyLinearLayout.
 * <p/>
 * <br>
 * (16/6/7)
 *
 * @author tongjin
 */
public class MyLinearLayout extends LinearLayout {

    public static int id = 0;

    private int currentId = id;

    /**
     * log tag for MyLinearLayout
     */
    private static final String TAG = "MyLinearLayout";

    public MyLinearLayout(Context context) {
        super(context);
        currentId = id++;
    }

    public MyLinearLayout(Context context, AttributeSet attrs) {
        super(context, attrs);
        currentId = id++;
    }

    public MyLinearLayout(Context context, AttributeSet attrs, int defStyleAttr) {
        super(context, attrs, defStyleAttr);
        currentId = id++;
    }

    @Override
    public boolean dispatchTouchEvent(MotionEvent ev) {
        int action = ev.getAction();
        Log.i(TAG, currentId + " dispatchTouchEvent: action = " + action);
        boolean b = super.dispatchTouchEvent(ev);
//        boolean b = currentId == 1;
        Log.i(TAG, currentId + " dispatchTouchEvent: return = " + b);
        return b;
    }

    @Override
    public boolean onInterceptTouchEvent(MotionEvent ev) {
        int action = ev.getAction();
        Log.i(TAG, currentId + " onInterceptTouchEvent: action = " + action);
        boolean b = super.onInterceptTouchEvent(ev);
//        boolean b = currentId == 1;
        Log.i(TAG, currentId + " onInterceptTouchEvent: return = " + b);
        return b;
    }

    @Override
    public boolean onTouchEvent(MotionEvent ev) {
        int action = ev.getAction();
        Log.i(TAG, currentId + " onTouchEvent: action = " + action);
        boolean b = super.onTouchEvent(ev);
//        boolean b = currentId == 1;
        Log.i(TAG, currentId + " onTouchEvent: return = " + b);
        return b;
    }
}
```

## activity_main.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:orientation="vertical"
    android:layout_height="match_parent"
    tools:context="com.qefee.pj.testtouchevent.MainActivity">

    <com.qefee.pj.testtouchevent.MyLinearLayout
        android:layout_width="300dp"
        android:layout_height="300dp"
        android:padding="10dp"
        android:clickable="false"
        android:layout_gravity="center"
        android:background="@android:color/holo_red_light">
        <com.qefee.pj.testtouchevent.MyLinearLayout
            android:layout_width="200dp"
            android:layout_height="200dp"
            android:padding="10dp"
            android:clickable="false"
            android:layout_gravity="center"
            android:background="@android:color/holo_green_light">
            <com.qefee.pj.testtouchevent.MyLinearLayout
                android:layout_width="100dp"
                android:layout_height="100dp"
                android:padding="10dp"
                android:clickable="false"
                android:layout_gravity="center"
                android:background="@android:color/holo_blue_light">

            </com.qefee.pj.testtouchevent.MyLinearLayout>
        </com.qefee.pj.testtouchevent.MyLinearLayout>
    </com.qefee.pj.testtouchevent.MyLinearLayout>
</LinearLayout>
```

# Touch Event Dispatch

下面几个demo使用可以看出事件的传递方法。

自己也可以动手修改下试试。

## No.1 使用原始事件1

### action

点击view2。就是最上面那个框。

### log

从日志可以看出，在没有打断的情况下，点击事件是一层层往下传递的，然后一层层往上传回。

```
06-08 14:30:10.285 9506-9506/com.qefee.pj.testtouchevent I/MainActivity:  dispatchTouchEvent: action = 0
06-08 14:30:10.285 9506-9506/com.qefee.pj.testtouchevent I/MyLinearLayout: 0 dispatchTouchEvent: action = 0
06-08 14:30:10.292 9506-9506/com.qefee.pj.testtouchevent I/MyLinearLayout: 0 onInterceptTouchEvent: action = 0
06-08 14:30:10.295 9506-9506/com.qefee.pj.testtouchevent I/MyLinearLayout: 0 onInterceptTouchEvent: return = false
06-08 14:30:10.295 9506-9506/com.qefee.pj.testtouchevent I/MyLinearLayout: 1 dispatchTouchEvent: action = 0
06-08 14:30:10.295 9506-9506/com.qefee.pj.testtouchevent I/MyLinearLayout: 1 onInterceptTouchEvent: action = 0
06-08 14:30:10.295 9506-9506/com.qefee.pj.testtouchevent I/MyLinearLayout: 1 onInterceptTouchEvent: return = false
06-08 14:30:10.295 9506-9506/com.qefee.pj.testtouchevent I/MyLinearLayout: 2 dispatchTouchEvent: action = 0
06-08 14:30:10.295 9506-9506/com.qefee.pj.testtouchevent I/MyLinearLayout: 2 onInterceptTouchEvent: action = 0
06-08 14:30:10.299 9506-9506/com.qefee.pj.testtouchevent I/MyLinearLayout: 2 onInterceptTouchEvent: return = false
06-08 14:30:10.299 9506-9506/com.qefee.pj.testtouchevent I/MyLinearLayout: 2 onTouchEvent: action = 0
06-08 14:30:10.302 9506-9506/com.qefee.pj.testtouchevent I/MyLinearLayout: 2 onTouchEvent: return = false
06-08 14:30:10.302 9506-9506/com.qefee.pj.testtouchevent I/MyLinearLayout: 2 dispatchTouchEvent: return = false
06-08 14:30:10.302 9506-9506/com.qefee.pj.testtouchevent I/MyLinearLayout: 1 onTouchEvent: action = 0
06-08 14:30:10.302 9506-9506/com.qefee.pj.testtouchevent I/MyLinearLayout: 1 onTouchEvent: return = false
06-08 14:30:10.302 9506-9506/com.qefee.pj.testtouchevent I/MyLinearLayout: 1 dispatchTouchEvent: return = false
06-08 14:30:10.305 9506-9506/com.qefee.pj.testtouchevent I/MyLinearLayout: 0 onTouchEvent: action = 0
06-08 14:30:10.305 9506-9506/com.qefee.pj.testtouchevent I/MyLinearLayout: 0 onTouchEvent: return = false
06-08 14:30:10.305 9506-9506/com.qefee.pj.testtouchevent I/MyLinearLayout: 0 dispatchTouchEvent: return = false
06-08 14:30:10.305 9506-9506/com.qefee.pj.testtouchevent I/MainActivity:  onTouchEvent: action = 0
06-08 14:30:10.305 9506-9506/com.qefee.pj.testtouchevent I/MainActivity:  onTouchEvent: return = false
06-08 14:30:10.309 9506-9506/com.qefee.pj.testtouchevent I/MainActivity:  dispatchTouchEvent: return = false
06-08 14:30:10.312 9506-9506/com.qefee.pj.testtouchevent I/MainActivity:  dispatchTouchEvent: action = 1
06-08 14:30:10.312 9506-9506/com.qefee.pj.testtouchevent I/MainActivity:  onTouchEvent: action = 1
06-08 14:30:10.312 9506-9506/com.qefee.pj.testtouchevent I/MainActivity:  onTouchEvent: return = false
06-08 14:30:10.312 9506-9506/com.qefee.pj.testtouchevent I/MainActivity:  dispatchTouchEvent: return = false

```

### image

![TouchEvent0](https://github.com/aotian16/Blog/blob/master/Study/Dev/Android/android%E7%9A%84Touch%E4%BA%8B%E4%BB%B6%E4%BC%A0%E9%80%92%E6%9C%BA%E5%88%B6%E5%AD%A6%E4%B9%A0/TouchEvent0.png?raw=true)

## No.2 使用原始事件2

### action

点击view1，就是中间那个框。

### log

点击view2也是同样情况。

```
06-12 17:45:48.553 11255-11255/com.qefee.pj.testtouchevent I/MainActivity:  dispatchTouchEvent: action = 0
06-12 17:45:48.553 11255-11255/com.qefee.pj.testtouchevent I/MyLinearLayout: 0 dispatchTouchEvent: action = 0
06-12 17:45:48.553 11255-11255/com.qefee.pj.testtouchevent I/MyLinearLayout: 0 onInterceptTouchEvent: action = 0
06-12 17:45:48.553 11255-11255/com.qefee.pj.testtouchevent I/MyLinearLayout: 0 onInterceptTouchEvent: return = false
06-12 17:45:48.553 11255-11255/com.qefee.pj.testtouchevent I/MyLinearLayout: 1 dispatchTouchEvent: action = 0
06-12 17:45:48.553 11255-11255/com.qefee.pj.testtouchevent I/MyLinearLayout: 1 onInterceptTouchEvent: action = 0
06-12 17:45:48.553 11255-11255/com.qefee.pj.testtouchevent I/MyLinearLayout: 1 onInterceptTouchEvent: return = false
06-12 17:45:48.553 11255-11255/com.qefee.pj.testtouchevent I/MyLinearLayout: 1 onTouchEvent: action = 0
06-12 17:45:48.553 11255-11255/com.qefee.pj.testtouchevent I/MyLinearLayout: 1 onTouchEvent: return = false
06-12 17:45:48.553 11255-11255/com.qefee.pj.testtouchevent I/MyLinearLayout: 1 dispatchTouchEvent: return = false
06-12 17:45:48.553 11255-11255/com.qefee.pj.testtouchevent I/MyLinearLayout: 0 onTouchEvent: action = 0
06-12 17:45:48.553 11255-11255/com.qefee.pj.testtouchevent I/MyLinearLayout: 0 onTouchEvent: return = false
06-12 17:45:48.553 11255-11255/com.qefee.pj.testtouchevent I/MyLinearLayout: 0 dispatchTouchEvent: return = false
06-12 17:45:48.553 11255-11255/com.qefee.pj.testtouchevent I/MainActivity:  onTouchEvent: action = 0
06-12 17:45:48.553 11255-11255/com.qefee.pj.testtouchevent I/MainActivity:  onTouchEvent: return = false
06-12 17:45:48.553 11255-11255/com.qefee.pj.testtouchevent I/MainActivity:  dispatchTouchEvent: return = false
06-12 17:45:48.573 11255-11255/com.qefee.pj.testtouchevent I/MainActivity:  dispatchTouchEvent: action = 2
06-12 17:45:48.573 11255-11255/com.qefee.pj.testtouchevent I/MainActivity:  onTouchEvent: action = 2
06-12 17:45:48.573 11255-11255/com.qefee.pj.testtouchevent I/MainActivity:  onTouchEvent: return = false
06-12 17:45:48.573 11255-11255/com.qefee.pj.testtouchevent I/MainActivity:  dispatchTouchEvent: return = false
06-12 17:45:48.670 11255-11255/com.qefee.pj.testtouchevent I/MainActivity:  dispatchTouchEvent: action = 1
06-12 17:45:48.670 11255-11255/com.qefee.pj.testtouchevent I/MainActivity:  onTouchEvent: action = 1
06-12 17:45:48.670 11255-11255/com.qefee.pj.testtouchevent I/MainActivity:  onTouchEvent: return = false
06-12 17:45:48.670 11255-11255/com.qefee.pj.testtouchevent I/MainActivity:  dispatchTouchEvent: return = false
```



### image

![touch event](https://github.com/aotian16/Blog/blob/master/Study/Dev/Android/android%E7%9A%84Touch%E4%BA%8B%E4%BB%B6%E4%BC%A0%E9%80%92%E6%9C%BA%E5%88%B6%E5%AD%A6%E4%B9%A0/TouchEvent2.png?raw=true)

## No.3

这里的流程图和No.2一样。区别是， 3是由于`onInterceptTouchEvent`打断才不再继续传递事件，而2是只点击了view1.

由于`onInterceptTouchEvent`的打断，事件在view1中不再往下传递，而是view1自己处理。

### action

1，(原始代码基础上)修改`onInterceptTouchEvent`的代码

```java
    @Override
    public boolean onInterceptTouchEvent(MotionEvent ev) {
        int action = ev.getAction();
        Log.i(TAG, currentId + " onInterceptTouchEvent: action = " + action);
        boolean b = super.onInterceptTouchEvent(ev);
        b = currentId == 1;
        Log.i(TAG, currentId + " onInterceptTouchEvent: return = " + b);
        return b;
    }
```

2，点击view2

### log

```
06-12 18:59:57.198 23353-23353/com.qefee.pj.testtouchevent I/MainActivity:  dispatchTouchEvent: action = 0
06-12 18:59:57.202 23353-23353/com.qefee.pj.testtouchevent I/MyLinearLayout: 0 dispatchTouchEvent: action = 0
06-12 18:59:57.205 23353-23353/com.qefee.pj.testtouchevent I/MyLinearLayout: 0 onInterceptTouchEvent: action = 0
06-12 18:59:57.205 23353-23353/com.qefee.pj.testtouchevent I/MyLinearLayout: 0 onInterceptTouchEvent: return = false
06-12 18:59:57.208 23353-23353/com.qefee.pj.testtouchevent I/MyLinearLayout: 1 dispatchTouchEvent: action = 0
06-12 18:59:57.208 23353-23353/com.qefee.pj.testtouchevent I/MyLinearLayout: 1 onInterceptTouchEvent: action = 0
06-12 18:59:57.208 23353-23353/com.qefee.pj.testtouchevent I/MyLinearLayout: 1 onInterceptTouchEvent: return = true
06-12 18:59:57.208 23353-23353/com.qefee.pj.testtouchevent I/MyLinearLayout: 1 onTouchEvent: action = 0
06-12 18:59:57.212 23353-23353/com.qefee.pj.testtouchevent I/MyLinearLayout: 1 onTouchEvent: return = false
06-12 18:59:57.212 23353-23353/com.qefee.pj.testtouchevent I/MyLinearLayout: 1 dispatchTouchEvent: return = false
06-12 18:59:57.218 23353-23353/com.qefee.pj.testtouchevent I/MyLinearLayout: 0 onTouchEvent: action = 0
06-12 18:59:57.218 23353-23353/com.qefee.pj.testtouchevent I/MyLinearLayout: 0 onTouchEvent: return = false
06-12 18:59:57.218 23353-23353/com.qefee.pj.testtouchevent I/MyLinearLayout: 0 dispatchTouchEvent: return = false
06-12 18:59:57.218 23353-23353/com.qefee.pj.testtouchevent I/MainActivity:  onTouchEvent: action = 0
06-12 18:59:57.218 23353-23353/com.qefee.pj.testtouchevent I/MainActivity:  onTouchEvent: return = false
06-12 18:59:57.218 23353-23353/com.qefee.pj.testtouchevent I/MainActivity:  dispatchTouchEvent: return = false
06-12 18:59:57.245 23353-23353/com.qefee.pj.testtouchevent I/MainActivity:  dispatchTouchEvent: action = 1
06-12 18:59:57.248 23353-23353/com.qefee.pj.testtouchevent I/MainActivity:  onTouchEvent: action = 1
06-12 18:59:57.248 23353-23353/com.qefee.pj.testtouchevent I/MainActivity:  onTouchEvent: return = false
06-12 18:59:57.248 23353-23353/com.qefee.pj.testtouchevent I/MainActivity:  dispatchTouchEvent: return = false
```



### image

![TouchEvent0](https://github.com/aotian16/Blog/blob/master/Study/Dev/Android/android%E7%9A%84Touch%E4%BA%8B%E4%BB%B6%E4%BC%A0%E9%80%92%E6%9C%BA%E5%88%B6%E5%AD%A6%E4%B9%A0/TouchEvent1.png?raw=true)

## No.4

view1的`onTouchEvent`返回`true`表示处理了该事件。

### action

1，(原始代码基础上)修改`onTouchEvent`的代码

```java
    @Override
    public boolean onTouchEvent(MotionEvent ev) {
        int action = ev.getAction();
        Log.i(TAG, currentId + " onTouchEvent: action = " + action);
        boolean b = super.onTouchEvent(ev);
        b = currentId == 1;
        Log.i(TAG, currentId + " onTouchEvent: return = " + b);
        return b;
    }
```

2，点击view2

### log

```
06-12 19:07:57.783 28831-28831/com.qefee.pj.testtouchevent I/MainActivity:  dispatchTouchEvent: action = 0
06-12 19:07:57.786 28831-28831/com.qefee.pj.testtouchevent I/MyLinearLayout: 0 dispatchTouchEvent: action = 0
06-12 19:07:57.786 28831-28831/com.qefee.pj.testtouchevent I/MyLinearLayout: 0 onInterceptTouchEvent: action = 0
06-12 19:07:57.786 28831-28831/com.qefee.pj.testtouchevent I/MyLinearLayout: 0 onInterceptTouchEvent: return = false
06-12 19:07:57.786 28831-28831/com.qefee.pj.testtouchevent I/MyLinearLayout: 1 dispatchTouchEvent: action = 0
06-12 19:07:57.786 28831-28831/com.qefee.pj.testtouchevent I/MyLinearLayout: 1 onInterceptTouchEvent: action = 0
06-12 19:07:57.786 28831-28831/com.qefee.pj.testtouchevent I/MyLinearLayout: 1 onInterceptTouchEvent: return = false
06-12 19:07:57.786 28831-28831/com.qefee.pj.testtouchevent I/MyLinearLayout: 2 dispatchTouchEvent: action = 0
06-12 19:07:57.786 28831-28831/com.qefee.pj.testtouchevent I/MyLinearLayout: 2 onInterceptTouchEvent: action = 0
06-12 19:07:57.786 28831-28831/com.qefee.pj.testtouchevent I/MyLinearLayout: 2 onInterceptTouchEvent: return = false
06-12 19:07:57.786 28831-28831/com.qefee.pj.testtouchevent I/MyLinearLayout: 2 onTouchEvent: action = 0
06-12 19:07:57.789 28831-28831/com.qefee.pj.testtouchevent I/MyLinearLayout: 2 onTouchEvent: return = false
06-12 19:07:57.789 28831-28831/com.qefee.pj.testtouchevent I/MyLinearLayout: 2 dispatchTouchEvent: return = false
06-12 19:07:57.789 28831-28831/com.qefee.pj.testtouchevent I/MyLinearLayout: 1 onTouchEvent: action = 0
06-12 19:07:57.789 28831-28831/com.qefee.pj.testtouchevent I/MyLinearLayout: 1 onTouchEvent: return = true
06-12 19:07:57.789 28831-28831/com.qefee.pj.testtouchevent I/MyLinearLayout: 1 dispatchTouchEvent: return = true
06-12 19:07:57.789 28831-28831/com.qefee.pj.testtouchevent I/MyLinearLayout: 0 dispatchTouchEvent: return = true
06-12 19:07:57.789 28831-28831/com.qefee.pj.testtouchevent I/MainActivity:  dispatchTouchEvent: return = true
06-12 19:07:57.813 28831-28831/com.qefee.pj.testtouchevent I/MainActivity:  dispatchTouchEvent: action = 1
06-12 19:07:57.816 28831-28831/com.qefee.pj.testtouchevent I/MyLinearLayout: 0 dispatchTouchEvent: action = 1
06-12 19:07:57.816 28831-28831/com.qefee.pj.testtouchevent I/MyLinearLayout: 0 onInterceptTouchEvent: action = 1
06-12 19:07:57.816 28831-28831/com.qefee.pj.testtouchevent I/MyLinearLayout: 0 onInterceptTouchEvent: return = false
06-12 19:07:57.816 28831-28831/com.qefee.pj.testtouchevent I/MyLinearLayout: 1 dispatchTouchEvent: action = 1
06-12 19:07:57.819 28831-28831/com.qefee.pj.testtouchevent I/MyLinearLayout: 1 onTouchEvent: action = 1
06-12 19:07:57.819 28831-28831/com.qefee.pj.testtouchevent I/MyLinearLayout: 1 onTouchEvent: return = true
06-12 19:07:57.819 28831-28831/com.qefee.pj.testtouchevent I/MyLinearLayout: 1 dispatchTouchEvent: return = true
06-12 19:07:57.819 28831-28831/com.qefee.pj.testtouchevent I/MyLinearLayout: 0 dispatchTouchEvent: return = true
06-12 19:07:57.819 28831-28831/com.qefee.pj.testtouchevent I/MainActivity:  dispatchTouchEvent: return = true
```



### image

略。同No.1

# 参考文章

* [Android Touch事件传递机制解析](http://www.cnblogs.com/jqyp/archive/2012/04/25/2469758.html)

* [Android dispatchTouchEvent介绍](http://mobile.51cto.com/abased-374715.htm)