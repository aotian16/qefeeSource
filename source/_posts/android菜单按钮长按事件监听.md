title: android菜单按钮长按事件监听
date: 2014-03-17 22:22:47
tags: android
categories: android
---

<!--head-->

refer ["How to access menu button onLongClick in Android?"](http://stackoverflow.com/questions/10867033/how-to-access-menu-button-onlongclick-in-android "How to access menu button onLongClick in Android?")

**code**

<!--more-->

<!--body-->
``` java
package com.example.pj012_menulongclick;

import android.app.Activity;
import android.os.Bundle;
import android.view.KeyEvent;

public class MainActivity extends Activity {

	@Override
	protected void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
		setContentView(R.layout.activity_main);
	}

	@Override
	public boolean onKeyDown(int keyCode, KeyEvent event) {
		if (keyCode == KeyEvent.KEYCODE_MENU) {
			// call startTracking() to get LongPress event
			// 调用event.startTracking()来获取长按事件
			event.startTracking();
			String method = "onKeyDown";
			log(keyCode, event, method);

			return true;
		}

		return super.onKeyDown(keyCode, event);
	}

	@Override
	public boolean onKeyUp(int keyCode, KeyEvent event) {
		if (keyCode == KeyEvent.KEYCODE_MENU && event.isTracking()
				&& !event.isCanceled()) {
			// handle regular key press here
			// 处理常规(短按)事件
			String method = "onKeyUp";
			log(keyCode, event, method);

			return true;
		}

		return super.onKeyUp(keyCode, event);
	}

	@Override
	public boolean onKeyLongPress(int keyCode, KeyEvent event) {
		if (keyCode == KeyEvent.KEYCODE_MENU) {
			// Handle Long Press here
			// 处理长按事件
			String method = "onKeyLongPress";
			log(keyCode, event, method);

			return true;
		}

		return super.onKeyLongPress(keyCode, event);
	}

	private void log(int keyCode, KeyEvent event, String method) {
		System.out.println(method);
		System.out.println("  keyCode = " + keyCode);
		System.out.println("  event = " + event);
	}
}

```