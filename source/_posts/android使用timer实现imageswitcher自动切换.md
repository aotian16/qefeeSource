title: android使用Timer实现ImageSwitcher自动切换
date: 2014-03-06 22:23:53
tags: android
categories: android
---

<!--head-->

没啥技术含量,只是记录下.

**代码如下**

<!--more-->

<!--body-->

``` java
package com.example.pj007_imageswitcher;

import java.lang.ref.WeakReference;
import java.util.Timer;
import java.util.TimerTask;

import android.app.Activity;
import android.os.Bundle;
import android.os.Handler;
import android.os.Message;
import android.view.View;
import android.widget.Button;
import android.widget.ImageSwitcher;

public class MainActivity extends Activity {

	private static final int IMAGE_SWITCH = 1234;

	Button btnNext;
	Button btnStop;
	ImageSwitcher imageSwitcher1;

	// TimerTask不是UI线程，需要用到handler来处理ui变更
	Handler handler;

	// 自定义handler
	static class MyHandler extends Handler {
		// 通过弱引用来避免以下警告。
		// (This Handler class should be static or leaks might occur )
		WeakReference<MainActivity> mActivity;

		MyHandler(MainActivity activity) {
			mActivity = new WeakReference<MainActivity>(activity);
		}

		@Override
		public void handleMessage(Message msg) {
			MainActivity theActivity = mActivity.get();
			switch (msg.what) {
			case IMAGE_SWITCH:
				// 切换图片
				theActivity.imageSwitcher1.showNext();
				break;
			default:

			}
		}
	}

	Timer timer;

	@Override
	protected void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
		setContentView(R.layout.activity_main);

		handler = new MyHandler(this);

		btnNext = (Button) findViewById(R.id.btnNext);
		btnStop = (Button) findViewById(R.id.btnStop);
		imageSwitcher1 = (ImageSwitcher) findViewById(R.id.imageSwitcher1);

		btnNext.setOnClickListener(new View.OnClickListener() {

			@Override
			public void onClick(View v) {
				if (timer != null) {
					timer.cancel();
				}

				timer = new Timer(true);

				// timer执行周期任务，延迟0ms，周期200ms
				timer.schedule(new TimerTask() {

					@Override
					public void run() {
						handler.sendEmptyMessage(IMAGE_SWITCH);
					}
				}, 0, 200);
			}
		});

		btnStop.setOnClickListener(new View.OnClickListener() {

			@Override
			public void onClick(View v) {
				if (timer != null) {
					timer.cancel();
				}
			}
		});
	}
}

```