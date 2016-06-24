title: android自定义view的自定义属性
categories: android
tags: android
date: 2016-06-24 11:28:46

---

<!--head-->

在android自定义view中， 可以使用自定义的属性来扩展功能。

**原文**

* [android自定义view的自定义属性](http://qefee.com/2016/06/24/android%E8%87%AA%E5%AE%9A%E4%B9%89view%E7%9A%84%E8%87%AA%E5%AE%9A%E4%B9%89%E5%B1%9E%E6%80%A7/)

[TOC]

<!--more-->

<!--body-->

# 自定义属性的步骤

## 1，定义属性文件

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <declare-styleable name="MyTextViewAttr">
        <attr name="name" format="string" />
        <attr name="age" format="integer" />
        <attr name="married" format="boolean" />
        <attr name="sex" format="enum">
            <enum name="male" value="0" />
            <enum name="female" value="1" />
        </attr>
        <attr name="mycolor" format="color|reference" />
        <attr name="myheight" format="dimension" />
    </declare-styleable>
</resources>
```

其中，

`declare-styleable.name`是我们的自定义属性的命名空间，代码中使用会作为前缀。

`attr.name`是属性名。

`attr.format`是数据格式。

**各个属性格式含义如下表**

| format    | detail  |
| --------- | ------- |
| boolean   | 布尔值     |
| enum      | 枚举      |
| reference | xml资源id |
| color     | 颜色      |
| dimension | 尺寸      |
| flag      | 位或运算    |
| float     | 浮点数     |
| fraction  | 百分数     |
| integer   | 整数      |
| string    | 字符串     |

## 2，在xml中使用自定义属性

```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    xmlns:myAttrs="http://schemas.android.com/apk/res-auto"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context="com.qefee.pj.mytextview.MainActivity">

    <com.qefee.pj.mytextview.view.MyTextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Hello World!"
        myAttrs:name = "tom"
        myAttrs:age = "20"
        myAttrs:married = "true"
        myAttrs:sex = "male"
        myAttrs:mycolor = "#66ccff"
        myAttrs:myheight = "60dp" />
</RelativeLayout>
```

`xmlns:myAttrs`这一行是引入我们的自定义属性，`myAttrs`这个名字可以随意，就是下面使用的自定义属性的前缀。

`myAttrs:name`使用自定义属性。`myAttrs`就是上面定义的前缀了，`name`是在自定义属性名。

自定义属性的数据格式参考1。

## 3, 在自定义view中使用自定义属性

```java
package com.qefee.pj.mytextview.view;

import android.content.Context;
import android.content.res.TypedArray;
import android.graphics.drawable.Drawable;
import android.util.AttributeSet;
import android.util.Log;
import android.widget.TextView;

import com.qefee.pj.mytextview.R;

public class MyTextView extends TextView {

    /**
     * log tag for MyTextView
     */
    private static final String TAG = "MyTextView";

    String name;
    int age;
    boolean married;
    String sex;
    Drawable color;
    float height;


    public MyTextView(Context context) {
        super(context);
    }

    public MyTextView(Context context, AttributeSet attrs) {
        super(context, attrs);

        showMyAttrs(context, attrs);
    }

    public MyTextView(Context context, AttributeSet attrs, int defStyleAttr) {
        super(context, attrs, defStyleAttr);

        showMyAttrs(context, attrs);
    }

    private void showMyAttrs(Context context, AttributeSet attrs) {
        TypedArray typedArray = context.obtainStyledAttributes(attrs, R.styleable.MyTextViewAttr);
        name = typedArray.getString(R.styleable.MyTextViewAttr_name);
        age = typedArray.getInt(R.styleable.MyTextViewAttr_age, 0);
        married = typedArray.getBoolean(R.styleable.MyTextViewAttr_married, false);
        sex = typedArray.getString(R.styleable.MyTextViewAttr_sex);
        color = typedArray.getDrawable(R.styleable.MyTextViewAttr_mycolor);
        height = typedArray.getDimension(R.styleable.MyTextViewAttr_myheight, 0f);

        typedArray.recycle();

        Log.i(TAG, "MyTextView: length = " + typedArray.length());
        Log.i(TAG, "MyTextView: name = " + name);
        Log.i(TAG, "MyTextView: age = " + age);
        Log.i(TAG, "MyTextView: married = " + married);
        Log.i(TAG, "MyTextView: sex = " + sex);
        Log.i(TAG, "MyTextView: color = " + color);
        Log.i(TAG, "MyTextView: height = " + height);
    }
}
```

`MyTextViewAttr`就是1中自定义属性的命名空间。

`MyTextViewAttr_name`引用自定义属性要加上前缀。

# 参考文章

* [PullScrollView详解（一）——自定义控件属性](http://blog.csdn.net/harvic880925/article/details/46537767)