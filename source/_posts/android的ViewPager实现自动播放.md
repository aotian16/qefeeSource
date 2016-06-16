title: android的ViewPager实现自动播放
categories: android
tags: viewpager
date: 2016-06-15 18:03:51

---

<!--head-->

上次实现了`ViewPager`的循环播放[android的ViewPager实现伪循环效果](http://qefee.com/2016/06/15/android%E7%9A%84ViewPager%E5%AE%9E%E7%8E%B0%E4%BC%AA%E5%BE%AA%E7%8E%AF%E6%95%88%E6%9E%9C/)， 这次来实现自动播放.

非常简单， 直接用`ScheduledExecutorService`就好了.

#### 原文

[android的ViewPager实现自动播放](http://qefee.com/2016/06/15/android%E7%9A%84ViewPager%E5%AE%9E%E7%8E%B0%E8%87%AA%E5%8A%A8%E6%92%AD%E6%94%BE/)

#### Code

直接看代码， 看注释.

<!--more-->

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

import java.util.concurrent.Executors;
import java.util.concurrent.ScheduledExecutorService;
import java.util.concurrent.TimeUnit;

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

    int currentItem;
    private ScheduledExecutorService executor;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        int size = initSize();
        initTextViews(size);
        initViewPager();
    }

    @Override
    protected void onResume() {
        super.onResume();

        startAutoScroll(); // activity激活时候自动播放
    }

    @Override
    protected void onPause() {
        super.onPause();

        stopAutoScroll(); // activity暂停时候停止自动播放
    }

    private void startAutoScroll() {
        stopAutoScroll();

        executor = Executors.newSingleThreadScheduledExecutor();
        Runnable command = new Runnable() {
            @Override
            public void run() {
                selectNextItem();
            }

            private void selectNextItem() {
                runOnUiThread(new Runnable() {
                    @Override
                    public void run() {
                        viewPager.setCurrentItem(++currentItem);
                    }
                });
            }
        };
        int delay = 2;
        int period = 2;
        TimeUnit timeUnit = TimeUnit.SECONDS;
        executor.scheduleAtFixedRate(command, delay, period, timeUnit);
    }

    private void stopAutoScroll() {
        if (executor != null) {
            executor.shutdownNow();
        }
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

        viewPager.addOnPageChangeListener(new ViewPager.OnPageChangeListener() {
            @Override
            public void onPageScrolled(int position, float positionOffset, int positionOffsetPixels) {

            }

            @Override
            public void onPageSelected(int position) {
                currentItem = position;

                startAutoScroll(); // 手动切换完成后恢复自动播放
            }

            @Override
            public void onPageScrollStateChanged(int state) {

            }
        });

        currentItem = texts.length * 1000000; // 取一个中间的大数字, 防止接近边界
        viewPager.setCurrentItem(currentItem);
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



<!--body-->
