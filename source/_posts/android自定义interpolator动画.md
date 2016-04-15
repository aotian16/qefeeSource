title: android自定义Interpolator动画
date: 2014-03-11 23:10:16
tags: android
categories: android
---

<!--head-->

android提供了很多的Tween animation补间动画。

比如:

* LinearInterpolator
* AccelerateInterpolator
* CycleInterpolator
* ...(很多不一一列举)

可以查看以下官网地址

* [Interpolator](http://developer.android.com/reference/android/view/animation/Interpolator.html "Interpolator")
* [animation-resource](http://developer.android.com/guide/topics/resources/animation-resource.html "animation-resource")

自定义Interpolator也很简单，实现Interpolator接口就行了。
用Animation.setInterpolator()设置自定义插入帧。

下面附上例子,实现的是闪现动画。

<!--more-->

<!--body-->

**Interpolator**

``` java
package com.example.pj010_imageviewanim.view.anim;

import android.view.animation.Interpolator;

/**
 * 自定义插入帧.(实现Interpolator接口)
 */
public class FlashInterpolator implements Interpolator {

	@Override
	public float getInterpolation(float input) {
		// 前半动画不变，后半动画完全变化
		// 以此达到闪现动画目的
		if (input < 0.5f) {
			return 0;
		} else {
			return 1;
		}
	}
}

```

**动画XML**

使用alpha来设置透明度，duration设置时间4秒，repeatCount设置为重复，repeatMode设置为从头开始。

``` xml
<?xml version="1.0" encoding="utf-8"?>
<set xmlns:android="http://schemas.android.com/apk/res/android">

    <alpha
        android:duration="4000"
        android:fillAfter="true"
        android:fromAlpha="1"
        android:repeatCount="infinite"
        android:repeatMode="restart"
        android:toAlpha="0.01" >
    </alpha>

</set>
```

**activity**

``` java
package com.example.pj010_imageviewanim;

import com.example.pj010_imageviewanim.view.anim.FlashInterpolator;

import android.app.Activity;
import android.os.Bundle;
import android.view.animation.Animation;
import android.view.animation.AnimationUtils;
import android.widget.ImageView;

public class MainActivity extends Activity {

	ImageView imageView1;
	Animation anim;
	
	@Override
	protected void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
		setContentView(R.layout.activity_main);
		
		imageView1 = (ImageView) findViewById(R.id.imageView1);
		anim = AnimationUtils.loadAnimation(this, R.anim.anim);
		// 创建自定义插入帧
		FlashInterpolator flashInterpolator = new FlashInterpolator();
		// 用Animation.setInterpolator()设置自定义插入帧
		anim.setInterpolator(flashInterpolator);
		imageView1.startAnimation(anim);
	}

}

```