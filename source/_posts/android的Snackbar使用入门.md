title: android的Snackbar使用入门
categories: android
tags: Snackbar
date: 2016-07-19 14:38:49

---

<!--head-->

**原文**

[android的Snackbar使用入门](http://qefee.com/2016/07/19/android%E7%9A%84Snackbar%E4%BD%BF%E7%94%A8%E5%85%A5%E9%97%A8/)

[TOC]

# `Snackbar`是什么

`Snackbar`就像一个高级版的`Toast`，具有反馈，用法也和`Toast`差不多。

**Look：**（借用别人的图，侵删）

![img](http://img.blog.csdn.net/20150114103321515)

# `Snackbar`的一些优点

* 具有反馈效果。点击按钮，消失或者显示都有相应的事件。
* 样式可以修改。虽然只能改改颜色来契合主题。

<!--more-->

<!--body-->

# `Snackbar`的使用方法

**加入依赖包**

app的gradle中加入依赖

```groovy
compile 'com.android.support:design:24.1.0'
```

代码如下

`MainActivity.java`

```java
package com.qefee.pj.testsnackbar;

import android.support.design.widget.Snackbar;
import android.support.v4.content.ContextCompat;
import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;
import android.util.Log;
import android.view.View;
import android.widget.Button;
import android.widget.Toast;

public class MainActivity extends AppCompatActivity {

    /**
     * log tag for MainActivity
     */
    private static final String TAG = "MainActivity";

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        Button showButton = (Button) findViewById(R.id.showButton);
        showButton.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                final Snackbar snackbar = Snackbar.
                        make(view, "测试title", Snackbar.LENGTH_LONG);
                snackbar.setAction("点我试试", new View.OnClickListener() { // 设置点击事件
                    @Override
                    public void onClick(View view) {
                        Toast.makeText(MainActivity.this, "点击了Snackbar", Toast.LENGTH_SHORT).show();
                        snackbar.dismiss();
                    }
                }).show();
            }
        });

        Button customButton = (Button) findViewById(R.id.customButton);
        customButton.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                final Snackbar snackbar = Snackbar.
                        make(view, "这里是标题", Snackbar.LENGTH_LONG);
                int buttonColor = ContextCompat.getColor(MainActivity.this, android.R.color.holo_orange_light);
                int backgroundColor = ContextCompat.getColor(MainActivity.this, android.R.color.holo_blue_light);
                snackbar.setActionTextColor(buttonColor); // 设置点击按钮的字体颜色
                snackbar.getView().setBackgroundColor(backgroundColor); // 设置点击Snackbar背景颜色
                snackbar.setCallback(new Snackbar.Callback() { // 设置回调函数
                    @Override
                    public void onDismissed(Snackbar snackbar, int event) { // 当Snackbar消失的时候回调
                        super.onDismissed(snackbar, event);

                        Log.i(TAG, "onDismissed: event = " + event);
                    }

                    @Override
                    public void onShown(Snackbar snackbar) { // 当Snackbar显示的时候回调
                        super.onShown(snackbar);

                        Log.i(TAG, "onShown: ");
                    }
                });
                snackbar.setAction("点我试试", new View.OnClickListener() { // 设置点击事件
                    @Override
                    public void onClick(View view) {
                        Toast.makeText(MainActivity.this, "点击了Snackbar", Toast.LENGTH_SHORT).show();
                        snackbar.dismiss();
                    }
                }).show();
            }
        });
    }
}
```

`activity_main.xml`

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:paddingBottom="@dimen/activity_vertical_margin"
    android:paddingLeft="@dimen/activity_horizontal_margin"
    android:paddingRight="@dimen/activity_horizontal_margin"
    android:paddingTop="@dimen/activity_vertical_margin"
    android:orientation="vertical"
    tools:context="com.qefee.pj.testsnackbar.MainActivity">

    <Button
        android:id="@+id/showButton"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="show" />
    <Button
        android:id="@+id/customButton"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="custom" />
</LinearLayout>
```



# 参考文章

* [还在用Toast？你Out啦，试试Snackbar吧！](http://www.devtf.cn/?p=798)
* [欢迎使用Snackbar](http://www.jianshu.com/p/f60e8d8271ea)
* [Android Design Support Library使用详解](http://blog.csdn.net/eclipsexys/article/details/46349721)