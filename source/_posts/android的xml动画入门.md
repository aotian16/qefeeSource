title: android的xml动画入门
categories: android
tags: animate
date: 2016-07-28 16:07:50

---

<!--head-->

简单用xml实现几种动画效果。建议阅读参考文章。

[TOC]

## 原文

[android的xml动画入门](http://qefee.com/2016/07/28/android%E7%9A%84xml%E5%8A%A8%E7%94%BB%E5%85%A5%E9%97%A8/)

## 参考文章

[【Android 基础】Animation 动画介绍和实现](http://www.cnblogs.com/yc-755909659/p/4290114.html)

## xml几种动画介绍

| No.  |   Name    | Detail |
| :--: | :-------: | :----: |
|  1   |   alpha   |  透明度   |
|  2   |   scale   |   缩放   |
|  3   | translate |   位移   |
|  4   |  rotate   |   旋转   |

**效果图**

![img](http://qefee.qiniudn.com/20160729_android%E7%9A%84xml%E5%8A%A8%E7%94%BB%E5%85%A5%E9%97%A8.gif)

<!--more-->

<!--body-->

## 代码如下

### `MainActivity.java`

```java
package com.qefee.pj.testanim;

import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;
import android.view.View;
import android.view.animation.Animation;
import android.view.animation.AnimationUtils;
import android.widget.Button;
import android.widget.ImageView;
import android.widget.Toast;

public class MainActivity extends AppCompatActivity {

    ImageView imageView;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        imageView = (ImageView) findViewById(R.id.imageView);

        Button alphaButton = (Button) findViewById(R.id.alphaButton);
        Button scaleButton = (Button) findViewById(R.id.scaleButton);
        Button transButton = (Button) findViewById(R.id.transButton);
        Button rotateButton = (Button) findViewById(R.id.rotateButton);

        doAnim(alphaButton, R.anim.anim_alpha, R.anim.anim_alpha_inverse); // 透明度
        doAnim(scaleButton, R.anim.anim_scale, R.anim.anim_scale_inverse); // 缩放
        doAnim(transButton, R.anim.anim_trans, R.anim.anim_trans_inverse); // 位移
        doAnim(rotateButton, R.anim.anim_rotate, R.anim.anim_rotate_inverse); // 旋转
    }

    private void doAnim(Button scaleButton, final int anim, final int animInverse) {
        scaleButton.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                Animation animation = AnimationUtils.loadAnimation(MainActivity.this, anim);

                // 设置动画监听
                animation.setAnimationListener(new Animation.AnimationListener() {
                    @Override
                    public void onAnimationStart(Animation animation) {
                        Toast.makeText(MainActivity.this, "动画开始了", Toast.LENGTH_SHORT).show();
                    }

                    @Override
                    public void onAnimationEnd(Animation animation) {
                        Toast.makeText(MainActivity.this, "动画结束了", Toast.LENGTH_SHORT).show();

                        // 动画结束后执行恢复动画
                        Animation animation1 = AnimationUtils.loadAnimation(MainActivity.this, animInverse);
                        imageView.startAnimation(animation1);
                    }

                    @Override
                    public void onAnimationRepeat(Animation animation) {
                        Toast.makeText(MainActivity.this, "动画重播了", Toast.LENGTH_SHORT).show();
                    }
                });
                imageView.startAnimation(animation);
            }
        });
    }
}

```

### `activity_main.xml`

```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:paddingBottom="@dimen/activity_vertical_margin"
    android:paddingLeft="@dimen/activity_horizontal_margin"
    android:paddingRight="@dimen/activity_horizontal_margin"
    android:paddingTop="@dimen/activity_vertical_margin"
    tools:context="com.qefee.pj.testanim.MainActivity">

    <RelativeLayout
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:layout_above="@+id/buttonsLayout"
        android:layout_alignParentEnd="true"
        android:layout_alignParentLeft="true"
        android:layout_alignParentRight="true"
        android:layout_alignParentStart="true"
        android:layout_alignParentTop="true">
        <ImageView
            android:id="@+id/imageView"
            android:layout_width="40dp"
            android:layout_height="40dp"
            android:layout_centerInParent="true"
            android:src="@mipmap/ic_launcher"/>
    </RelativeLayout>

    <LinearLayout
        android:id="@+id/buttonsLayout"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_alignParentBottom="true"
        android:layout_alignParentEnd="true"
        android:layout_alignParentLeft="true"
        android:layout_alignParentRight="true"
        android:layout_alignParentStart="true"
        android:orientation="horizontal"
        android:weightSum="4">

        <Button
            android:id="@+id/alphaButton"
            android:layout_width="0dp"
            android:layout_height="wrap_content"
            android:layout_weight="1"
            android:text="alpha" />

        <Button
            android:id="@+id/scaleButton"
            android:layout_width="0dp"
            android:layout_height="wrap_content"
            android:layout_weight="1"
            android:text="scale" />

        <Button
            android:id="@+id/transButton"
            android:layout_width="0dp"
            android:layout_height="wrap_content"
            android:layout_weight="1"
            android:text="trans" />

        <Button
            android:id="@+id/rotateButton"
            android:layout_width="0dp"
            android:layout_height="wrap_content"
            android:layout_weight="1"
            android:text="rotate" />
    </LinearLayout>
</RelativeLayout>

```



### `anim_alpha.xml`

```xml
<?xml version="1.0" encoding="utf-8"?>
<set xmlns:android="http://schemas.android.com/apk/res/android"
    android:fillAfter="true"
    android:duration="1500"
    android:interpolator="@android:anim/accelerate_decelerate_interpolator"
    android:shareInterpolator="true">

    <!--
    fillAfter: 如果为true,动画结束后保留动画结果。否则恢复。
    duration: 动画时间。
    interpolator: 动画效果。
    shareInterpolator: 是否与子元素共享动画效果。
    -->

    <alpha
        android:fromAlpha="1.0"
        android:toAlpha="0.1"/>
</set>
```

### `anim_alpha_inverse.xml`

```xml
<?xml version="1.0" encoding="utf-8"?>
<set xmlns:android="http://schemas.android.com/apk/res/android"
    android:fillAfter="true"
    android:duration="1500"
    android:interpolator="@android:anim/accelerate_decelerate_interpolator"
    android:shareInterpolator="true">

    <!--
    fillAfter: 如果为true,动画结束后保留动画结果。否则恢复。
    duration: 动画时间。
    interpolator: 动画效果。
    shareInterpolator: 是否与子元素共享动画效果。
    -->

    <alpha
        android:fromAlpha="0.1"
        android:toAlpha="1.0" />
</set>
```

### `anim_rotate.xml`

```xml
<?xml version="1.0" encoding="utf-8"?>
<set xmlns:android="http://schemas.android.com/apk/res/android"
    android:fillAfter="true"
    android:duration="1500"
    android:interpolator="@android:anim/accelerate_decelerate_interpolator"
    android:shareInterpolator="true">

    <!--
    fillAfter: 如果为true,动画结束后保留动画结果。否则恢复。
    duration: 动画时间。
    interpolator: 动画效果。
    shareInterpolator: 是否与子元素共享动画效果。
    -->

    <rotate
        android:fromDegrees="0"
        android:toDegrees="180"
        android:pivotX="50%"
        android:pivotY="50%"/>

    <!--
    pivotX: x支点
    pivotY: y支点
    -->
</set>
```

### `anim_rotate_inverse.xml`

```xml
<?xml version="1.0" encoding="utf-8"?>
<set xmlns:android="http://schemas.android.com/apk/res/android"
    android:fillAfter="true"
    android:duration="1500"
    android:interpolator="@android:anim/accelerate_decelerate_interpolator"
    android:shareInterpolator="true">

    <!--
    fillAfter: 如果为true,动画结束后保留动画结果。否则恢复。
    duration: 动画时间。
    interpolator: 动画效果。
    shareInterpolator: 是否与子元素共享动画效果。
    -->

    <rotate
        android:fromDegrees="180"
        android:toDegrees="360"
        android:pivotX="50%"
        android:pivotY="50%"/>

    <!--
    pivotX: x支点
    pivotY: y支点
    -->
</set>
```

### `anim_scale.xml`

```xml
<?xml version="1.0" encoding="utf-8"?>
<set xmlns:android="http://schemas.android.com/apk/res/android"
    android:fillAfter="true"
    android:duration="1500"
    android:interpolator="@android:anim/accelerate_decelerate_interpolator"
    android:shareInterpolator="true">

    <!--
    fillAfter: 如果为true,动画结束后保留动画结果。否则恢复。
    duration: 动画时间。
    interpolator: 动画效果。
    shareInterpolator: 是否与子元素共享动画效果。
    -->

    <scale
        android:fromXScale="1.0"
        android:fromYScale="1.0"
        android:toXScale="2.0"
        android:toYScale="2.0"
        android:pivotX="50%"
        android:pivotY="50%" />

    <!--
    pivotX: x支点
    pivotY: y支点
    -->
</set>
```

### `anim_scale_inverse.xml`

```xml
<?xml version="1.0" encoding="utf-8"?>
<set xmlns:android="http://schemas.android.com/apk/res/android"
    android:fillAfter="true"
    android:duration="1500"
    android:interpolator="@android:anim/accelerate_decelerate_interpolator"
    android:shareInterpolator="true">

    <!--
    fillAfter: 如果为true,动画结束后保留动画结果。否则恢复。
    duration: 动画时间。
    interpolator: 动画效果。
    shareInterpolator: 是否与子元素共享动画效果。
    -->

    <scale
        android:fromXScale="2.0"
        android:fromYScale="2.0"
        android:toXScale="1.0"
        android:toYScale="1.0"
        android:pivotX="50%"
        android:pivotY="50%" />

    <!--
    pivotX: x支点
    pivotY: y支点
    -->
</set>
```

### `anim_trans.xml`

```xml
<?xml version="1.0" encoding="utf-8"?>
<set xmlns:android="http://schemas.android.com/apk/res/android"
    android:fillAfter="true"
    android:duration="1500"
    android:interpolator="@android:anim/accelerate_decelerate_interpolator"
    android:shareInterpolator="true">

    <!--
    fillAfter: 如果为true,动画结束后保留动画结果。否则恢复。
    duration: 动画时间。
    interpolator: 动画效果。
    shareInterpolator: 是否与子元素共享动画效果。
    -->

    <translate
        android:fromXDelta="0"
        android:fromYDelta="0"
        android:toXDelta="100"
        android:toYDelta="100" />
</set>
```

### `anim_trans_inverse.xml`

```xml
<?xml version="1.0" encoding="utf-8"?>
<set xmlns:android="http://schemas.android.com/apk/res/android"
    android:fillAfter="true"
    android:duration="1500"
    android:interpolator="@android:anim/accelerate_decelerate_interpolator"
    android:shareInterpolator="true">

    <!--
    fillAfter: 如果为true,动画结束后保留动画结果。否则恢复。
    duration: 动画时间。
    interpolator: 动画效果。
    shareInterpolator: 是否与子元素共享动画效果。
    -->

    <translate
        android:fromXDelta="100"
        android:fromYDelta="100"
        android:toXDelta="0"
        android:toYDelta="0" />
</set>
```

