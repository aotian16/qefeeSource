title: android的ImageView动画自动播放
categories: qefee
tags: qefee
date: 2016-07-27 16:33:20

---

<!--head-->

ImageView的动画在部分手机上会自动播放，比较奇怪，就在网上找了找解决办法。
<!--more-->
<!--body-->
转自

* [animation-list-animationdrawable-autostart](http://stackoverflow.com/questions/14220517/animation-list-animationdrawable-autostart)

自动播放:

```
<ImageView
  android:layout_width="wrap_content"
  android:layout_height="wrap_content"
  android:src="@drawable/wave"
  android:layout_centerHorizontal="true"
  />
```

不会自动播放:

```
<ImageView
  android:layout_width="wrap_content"
  android:layout_height="wrap_content"
  android:background="@drawable/wave"
  android:layout_centerHorizontal="true"
  />
```

设置 animation-list drawble 作为background, 需要自己调用`AnimationDrawable.start()`.希望能帮到你.