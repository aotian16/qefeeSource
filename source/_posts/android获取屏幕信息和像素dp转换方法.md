title: android获取屏幕信息和像素dp转换方法
date: 2014-03-05 23:46:35
tags: android
categories: android
---

<!--head-->

android获取屏幕信息和像素dp转换方法.用到了DisplayMetrics类.

参考文章[Android获取屏幕分辨率 dp pix转换](http://blog.csdn.net/stonesharp/article/details/7997852)

**直接看代码**

<!--more-->

<!--body-->

``` java
package com.example.pj006_displaymetrics;

import android.app.Activity;
import android.content.Context;
import android.os.Bundle;
import android.util.DisplayMetrics;
import android.widget.TextView;

public class MainActivity extends Activity {
	TextView msg;
	@Override
	protected void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
		setContentView(R.layout.activity_main);
		
		DisplayMetrics displayMetrics = this.getResources().getDisplayMetrics();
		
		float density = displayMetrics.density;
		int densityDpi = displayMetrics.densityDpi;
		int heightPixels = displayMetrics.heightPixels;
		int widthPixels = displayMetrics.widthPixels;
		float scaledDensity = displayMetrics.scaledDensity;
		float xdpi = displayMetrics.xdpi;
		float ydpi = displayMetrics.ydpi;
		
		msg=(TextView) findViewById(R.id.msg);
		
		msg.setText("屏幕密度density="+density+"\n"+
				"densityDpi="+densityDpi+"\n"+
				"像素高heightPixels="+heightPixels+"\n"+
				"像素宽widthPixels="+widthPixels+"\n"+
				"屏幕密度scaledDensity="+scaledDensity+"\n"+
				"xdpi="+xdpi+"\n"+
				"ydpi="+ydpi+"\n");
	}

	/**
	 * dp转像素
	 * 
	 * @param context
	 * @param dipValue
	 * @return 像素
	 */
	public static int dip2px(Context context, float dipValue) {
		final float scale = context.getResources().getDisplayMetrics().density;
		return (int) (dipValue * scale + 0.5f);
	}

	/**
	 * 像素转dp
	 * 
	 * @param context
	 * @param pxValue
	 * @return dp
	 */
	public static int px2dip(Context context, float pxValue) {
		final float scale = context.getResources().getDisplayMetrics().density;
		return (int) (pxValue / scale + 0.5f);
	}
}
```