title: android中用Fresco实现圆角图片和圆形图片
categories: qefee
tags: qefee
date: 2016-11-25 14:36:12

---

<!--head-->
## 效果图
![img](http://qefee.qiniudn.com/20161125_android中用Fresco实现圆角图片和圆形图片.png)

<!--more-->



<!--body-->

## 代码

需要注意的地方用注释给出。

可以在github上下载源码。[点我试试](https://github.com/aotian16/AndroidStudy/tree/master/projects/TestFresco)。

```java
Uri uri = Uri.parse("https://pic4.zhimg.com/03b2d57be62b30f158f48f388c8f3f33_b.png");
        SimpleDraweeView commonImageView = (SimpleDraweeView) findViewById(R.id.commonImageView);
        commonImageView.setImageURI(uri);

        SimpleDraweeView circleImageView = (SimpleDraweeView) findViewById(R.id.circleImageView);
        circleImageView.setImageURI(uri);

        SimpleDraweeView roundedImageView = (SimpleDraweeView) findViewById(R.id.roundedImageView);
        roundedImageView.setImageURI(uri);
```

```xml
<LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:orientation="vertical">
        <LinearLayout
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:orientation="horizontal">
            <TextView
                android:layout_width="100dp"
                android:layout_height="wrap_content"
                android:layout_gravity="center"
                android:text="common image" />
            <com.facebook.drawee.view.SimpleDraweeView
                android:id="@+id/commonImageView"
                android:layout_width="130dp"
                android:layout_height="130dp"
                fresco:actualImageScaleType="centerCrop"
                fresco:placeholderImage="@mipmap/ic_launcher"
                fresco:placeholderImageScaleType="centerCrop" />
        </LinearLayout>
        <LinearLayout
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:orientation="horizontal">
            <TextView
                android:layout_width="100dp"
                android:layout_height="wrap_content"
                android:layout_gravity="center"
                android:text="circle image" />
            <com.facebook.drawee.view.SimpleDraweeView
                android:id="@+id/circleImageView"
                android:layout_width="130dp"
                android:layout_height="130dp"
                fresco:actualImageScaleType="centerCrop"
                fresco:placeholderImage="@mipmap/ic_launcher"
                fresco:placeholderImageScaleType="centerCrop"
                fresco:roundAsCircle="true" />
        </LinearLayout>
        <LinearLayout
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:orientation="horizontal">
            <TextView
                android:layout_width="100dp"
                android:layout_height="wrap_content"
                android:layout_gravity="center"
                android:text="rounded image" />
            <com.facebook.drawee.view.SimpleDraweeView
                android:id="@+id/roundedImageView"
                android:layout_width="130dp"
                android:layout_height="130dp"
                fresco:actualImageScaleType="centerCrop"
                fresco:placeholderImage="@mipmap/ic_launcher"
                fresco:placeholderImageScaleType="centerCrop"
                fresco:roundedCornerRadius="25dp" />
        </LinearLayout>
    </LinearLayout>
```

## 参考文章

- [fresco-cn圆角和圆圈](https://www.fresco-cn.org/docs/rounded-corners-and-circles.html)

## 原文

- [android中用RecyclerView实现滑动删除与切换item](http://qefee.com/2016/11/25/android%E4%B8%AD%E7%94%A8Fresco%E5%AE%9E%E7%8E%B0%E5%9C%86%E8%A7%92%E5%9B%BE%E7%89%87%E5%92%8C%E5%9C%86%E5%BD%A2%E5%9B%BE%E7%89%87/)