title: android的ViewPager实现伪循环效果
categories: android
tags: viewpager
date: 2016-06-15 15:04:56

---

<!--head-->

### 原文

[android的ViewPager实现伪循环效果](http://qefee.com/2016/06/15/android%E7%9A%84ViewPager%E5%AE%9E%E7%8E%B0%E4%BC%AA%E5%BE%AA%E7%8E%AF%E6%95%88%E6%9E%9C/)

### 原理

网上看到`ViewPager`的循环效果， 大概有两种实现。

| No.  | Detail                                   |
| :--: | ---------------------------------------- |
|  1   | 在原始视图左右各添加一个view，当到达边界view的时候， 快速跳转到相对应的view， 达成循环的效果。**参考** [ViewPager实现左右无限循环效果](http://blog.csdn.net/oweixiao123/article/details/23459041) |
|  2   | 用一个比较大的列表view， 定位到中间， 让客户以为是无限的。         |

第一种实现方法， 到达边界的时候， 视图切换不太自然。

本文实现第二种方法。

### 代码如下

需要注意的地方都注释了，应该比较容易理解了。

<!--more-->

#### MainActivity.java

```java
package com.qefee.pj.testviewpager;

import android.graphics.Color;
import android.os.Bundle;
import android.support.v4.view.PagerAdapter;
import android.support.v4.view.ViewPager;
import android.support.v7.app.AppCompatActivity;
import android.util.Log;
import android.view.View;
import android.view.ViewGroup;
import android.widget.TextView;

public class MainActivity extends AppCompatActivity {
    /**
     * log tag for MainActivity
     */
    private static final String TAG = "MainActivity";

    ViewPager viewPager;

    String[] texts = {
            "hello",
            "world",
            "1",
            "2"
    };

    TextView[] textViews;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        int size = initSize();
        initTextViews(size);
        initViewPager();
    }

    private void initViewPager() {
        viewPager = (ViewPager) findViewById(R.id.viewPage);

        viewPager.setAdapter(new PagerAdapter() {
            @Override
            public int getCount() {
                return Integer.MAX_VALUE; // 取一个大数字
            }

            @Override
            public boolean isViewFromObject(View view, Object object) {
                return view == object;
            }

            @Override
            public Object instantiateItem(ViewGroup container, int position) {
                Log.i(TAG, "instantiateItem: instantiateItem + position = " + position);
                TextView t = textViews[position % textViews.length];
                container.addView(t);

                return t;
            }

            @Override
            public void destroyItem(ViewGroup container, int position, Object object) {
                Log.i(TAG, "destroyItem: + position = " + position);
                container.removeView((View) object);
            }
        });

        viewPager.setCurrentItem(texts.length * 1000000); // 取一个中间的大数字, 防止接近边界
    }

    private void initTextViews(int size) {
        TextView[] tvs = new TextView[size];

        for (int i = 0; i < tvs.length; i++) {
            tvs[i] = new TextView(this);
            tvs[i].setText(texts[i % texts.length]);

            int color = 0;

            switch (i % texts.length) {
                case 0:
                    color = Color.parseColor("#aaaaaa");
                    break;
                case 1:
                    color = Color.parseColor("#cccccc");
                    break;
                case 2:
                    color = Color.parseColor("#777777");
                    break;
                case 3:
                    color = Color.parseColor("#999999");
                    break;
                default:
                    break;
            }

            tvs[i].setBackgroundColor(color);
            ViewGroup.LayoutParams layoutParams = new ViewGroup.LayoutParams(ViewGroup.LayoutParams.MATCH_PARENT, ViewGroup.LayoutParams.MATCH_PARENT);
            tvs[i].setLayoutParams(layoutParams);
            textViews = tvs;
        }
    }

    private int initSize() {
        int size;
        if (texts.length > 3) {
            size = texts.length;
        } else {
            size = texts.length * 2; // 小于3个时候, 需要扩大一倍, 防止出错
        }
        return size;
    }
}
```

#### activity_main.xml

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
    tools:context="com.qefee.pj.testviewpager.MainActivity">

    <android.support.v4.view.ViewPager
        android:id="@+id/viewPage"
        android:layout_width="match_parent"
        android:layout_height="match_parent"/>
</RelativeLayout>
```



<!--body-->
