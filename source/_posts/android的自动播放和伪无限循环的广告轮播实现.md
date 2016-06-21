title: android的自动播放和伪无限循环的广告轮播实现
categories: android
tags: banner
date: 2016-06-21 20:26:21

---

<!--head-->

上次我们实现了[android的ViewPager实现加载网络图片并自动轮播](http://blog.csdn.net/aotian16/article/details/51694231)。

![gif](https://github.com/aotian16/Blog/blob/master/Study/Dev/Android/android%E7%9A%84ViewPager%E5%AE%9E%E7%8E%B0%E5%8A%A0%E8%BD%BD%E7%BD%91%E7%BB%9C%E5%9B%BE%E7%89%87%E5%B9%B6%E8%87%AA%E5%8A%A8%E8%BD%AE%E6%92%AD/viewPager.gif?raw=true)



**原文** [android的自动播放和伪无限循环的广告轮播实现](http://qefee.com/2016/06/21/android%E7%9A%84%E8%87%AA%E5%8A%A8%E6%92%AD%E6%94%BE%E5%92%8C%E4%BC%AA%E6%97%A0%E9%99%90%E5%BE%AA%E7%8E%AF%E7%9A%84%E5%B9%BF%E5%91%8A%E8%BD%AE%E6%92%AD%E5%AE%9E%E7%8E%B0/)

这次我们剥离出代码， 自定义一个view来简单化。

源码可以在github上找到。

* [**Banner**](https://github.com/aotian16/Banner)

简单使用说明如下：

<!--more-->

### 1, Application中初始化Fresco

```java
public class App extends Application {
    @Override
    public void onCreate() {
        super.onCreate();

        Fresco.initialize(this); // init Fresco
    }
}
```

### 2, 添加网络权限

```xml
<uses-permission android:name="android.permission.INTERNET"/>
```

### 3, Layout中定义

```xml
<com.qefee.pj.banner.view.BannerView
        android:id="@+id/bannerView"
        android:layout_width="match_parent"
        android:layout_height="match_parent"/>
```

### 4, 代码中使用

```java
public class MainActivity extends AppCompatActivity {
    BannerView bannerView;

    String[] imageUris = {
            "https://pic4.zhimg.com/03b2d57be62b30f158f48f388c8f3f33_b.png",
            "https://pic1.zhimg.com/4373a4f045e5e9ae16ebd6a624bf6228_b.png",
            "https://pic2.zhimg.com/0364e17a1561f48793993d8bf1cdc785_b.png",
            "https://pic2.zhimg.com/55fa74ff3eba164ed1db2037df1a8311_b.png",
            "https://pic4.zhimg.com/5dc30569c06e7c6266c9809f6eb80a7b_b.jpg"
    };

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        bannerView = (BannerView) findViewById(R.id.bannerView);
        bannerView.init(imageUris);
    }

    @Override
    protected void onResume() {
        super.onResume();

        bannerView.startAutoScroll(); // auto scroll when resume
    }

    @Override
    protected void onPause() {
        super.onPause();

        bannerView.stopAutoScroll(); // stop scroll when pause
    }
}
```



<!--body-->
