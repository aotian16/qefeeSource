title: android的Touch事件传递机制学习
categories: android
tags: android
date: 2016-06-07 20:39:04

---

<!--head-->

[TOC]

# Reffer Methods

ViewGroup子类的touch事件相关方法：

| No.  | Name                  | Detail                         |
| :--: | --------------------- | ------------------------------ |
|  1   | dispatchTouchEvent    | 分发点击事件。true表示准备处理事件。           |
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

import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;
import android.util.Log;
import android.view.MotionEvent;

public class MainActivity extends AppCompatActivity {

    /**
     * log tag for MainActivity
     */
    private static final String TAG = "MainActivity";

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
    }

    @Override
    public boolean dispatchTouchEvent(MotionEvent ev) {
        int action = ev.getAction();
        Log.i(TAG, " dispatchTouchEvent: action = " + action);
        boolean b = super.dispatchTouchEvent(ev);
        Log.i(TAG, " dispatchTouchEvent: return = " + b);
        return b;
    }

    @Override
    public boolean onTouchEvent(MotionEvent ev) {
        int action = ev.getAction();
        Log.i(TAG, " onTouchEvent: action = " + action);
        boolean b = super.onTouchEvent(ev);
        Log.i(TAG, " onTouchEvent: return = " + b);
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

## No.1

### action

点击view2

### log

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

todo…..