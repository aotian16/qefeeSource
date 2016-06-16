title: android的ViewPager实现加载网络图片并自动轮播
categories: android
tags: viewpager
date: 2016-06-16 19:15:42

---

<!--head-->

弄个图好看点。

![gif](https://github.com/aotian16/Blog/blob/master/Study/Dev/Android/android%E7%9A%84ViewPager%E5%AE%9E%E7%8E%B0%E5%8A%A0%E8%BD%BD%E7%BD%91%E7%BB%9C%E5%9B%BE%E7%89%87%E5%B9%B6%E8%87%AA%E5%8A%A8%E8%BD%AE%E6%92%AD/viewPager.gif?raw=true)

阅读本文之前， 先看下前面打底的两篇文章。

* [android的ViewPager实现伪循环效果](http://qefee.com/2016/06/15/android%E7%9A%84ViewPager%E5%AE%9E%E7%8E%B0%E4%BC%AA%E5%BE%AA%E7%8E%AF%E6%95%88%E6%9E%9C/)
* [android的ViewPager实现自动播放](http://qefee.com/2016/06/15/android%E7%9A%84ViewPager%E5%AE%9E%E7%8E%B0%E8%87%AA%E5%8A%A8%E6%92%AD%E6%94%BE/)

前面已经实现了循环效果和自动播放， 现在我们来实现加载网络图片。

使用的是第三方库 facebook的[**Fresco**](https://github.com/facebook/fresco)。



### 原文

[android的ViewPager实现加载网络图片并自动轮播](http://localhost:4000/2016/06/16/android%E7%9A%84ViewPager%E5%AE%9E%E7%8E%B0%E5%8A%A0%E8%BD%BD%E7%BD%91%E7%BB%9C%E5%9B%BE%E7%89%87%E5%B9%B6%E8%87%AA%E5%8A%A8%E8%BD%AE%E6%92%AD/)

### 代码

同以前一样， 注意点用注释的形式给出。

这次需要注意下的地方有

* `build.gradle`中加入Fresco
* `AndroidManifest.xml`中加入访问网络的权限
* `App.java`中初始化Fresco

<!--more-->

#### AndroidManifest.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.qefee.pj.testviewpager">

    <!--网络权限-->
    <uses-permission android:name="android.permission.INTERNET"/>

    <application
        android:name=".App"
        android:allowBackup="true"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:supportsRtl="true"
        android:theme="@style/AppTheme">
        <activity android:name=".MainActivity">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />

                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
    </application>

</manifest>
```

#### App.java

```java
package com.qefee.pj.testviewpager;

import android.app.Application;

import com.facebook.drawee.backends.pipeline.Fresco;

public class App extends Application {
    @Override
    public void onCreate() {
        super.onCreate();

        Fresco.initialize(this); // 初始化Fresco
    }
}
```



#### MainActivity.java

```java
package com.qefee.pj.testviewpager;

import android.net.Uri;
import android.os.Bundle;
import android.support.v4.view.PagerAdapter;
import android.support.v4.view.ViewPager;
import android.support.v7.app.AppCompatActivity;
import android.util.Log;
import android.view.View;
import android.view.ViewGroup;

import com.facebook.drawee.view.SimpleDraweeView;

import java.util.concurrent.Executors;
import java.util.concurrent.ScheduledExecutorService;
import java.util.concurrent.TimeUnit;

public class MainActivity extends AppCompatActivity {
    /**
     * log tag for MainActivity
     */
    private static final String TAG = "MainActivity";

    ViewPager viewPager;

    String[] imageUris = {
            "https://pic4.zhimg.com/03b2d57be62b30f158f48f388c8f3f33_b.png",
            "https://pic1.zhimg.com/4373a4f045e5e9ae16ebd6a624bf6228_b.png",
            "https://pic2.zhimg.com/0364e17a1561f48793993d8bf1cdc785_b.png",
            "https://pic2.zhimg.com/55fa74ff3eba164ed1db2037df1a8311_b.png",
            "https://pic4.zhimg.com/5dc30569c06e7c6266c9809f6eb80a7b_b.jpg"
    };

    SimpleDraweeView[] simpleDraweeViews;

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
                SimpleDraweeView t = simpleDraweeViews[position % simpleDraweeViews.length];
                container.addView(t);

                Uri uri = Uri.parse(imageUris[position % simpleDraweeViews.length]);
                t.setImageURI(uri);

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

        currentItem = imageUris.length * 1000000; // 取一个中间的大数字, 防止接近边界
        viewPager.setCurrentItem(currentItem);
    }

    private void initTextViews(int size) {
        SimpleDraweeView[] tvs = new SimpleDraweeView[size];

        for (int i = 0; i < tvs.length; i++) {
            tvs[i] = new SimpleDraweeView(this);
            tvs[i].getHierarchy().setPlaceholderImage(R.mipmap.ic_launcher);

            ViewGroup.LayoutParams layoutParams = new ViewGroup.LayoutParams(ViewGroup.LayoutParams.MATCH_PARENT, ViewGroup.LayoutParams.MATCH_PARENT);
            tvs[i].setLayoutParams(layoutParams);
            simpleDraweeViews = tvs;
        }
    }

    private int initSize() {
        int size;
        if (imageUris.length > 3) {
            size = imageUris.length;
        } else {
            size = imageUris.length * 2; // 小于3个时候, 需要扩大一倍, 防止出错
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

#### build.gradle  (app)

```javascript
apply plugin: 'com.android.application'

android {
    compileSdkVersion 23
    buildToolsVersion "23.0.3"

    defaultConfig {
        applicationId "com.qefee.pj.testviewpager"
        minSdkVersion 15
        targetSdkVersion 23
        versionCode 1
        versionName "1.0"
    }
    buildTypes {
        release {
            minifyEnabled false
            proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'
        }
    }
}

dependencies {
    compile fileTree(dir: 'libs', include: ['*.jar'])
    testCompile 'junit:junit:4.12'
    compile 'com.android.support:appcompat-v7:23.4.0'

//    加入Fresco
    compile 'com.facebook.fresco:fresco:0.10.0'
}
```



<!--body-->
