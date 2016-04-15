title: android调整控件大小以及位置
date: 2014-02-15 17:49:35
tags: android
categories: android
---

<!--head-->

android中调整控件大小以及位置的方法,AbsoluteLayout已经废弃了

**看代码**

<!--more-->

<!--body-->
```java
package com.qefee.pj017_absolutelayout;

import android.app.Activity;
import android.os.Bundle;
import android.view.View;
import android.widget.AbsoluteLayout;
import android.widget.Button;
import android.widget.ImageView;
import android.widget.TextView;

public class MainActivity extends Activity {
	AbsoluteLayout container;
	TextView textView1;
	ImageView imageView1;
	Button btnUp;
	Button btnDown;
	Button btnBig;
	Button btnSmall;

	@Override
	protected void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
		setContentView(R.layout.activity_main);

		getViews();
		setListeners();

	}

	float x;
	float y;

	private void setListeners() {
		btnUp.setOnClickListener(new View.OnClickListener() {

			@Override
			public void onClick(View v) {
				x += 10;
				y += 10;
				// 调整相对于父控件的位置
				container.setTranslationX(x);
				container.setTranslationY(y);
			}
		});
		btnDown.setOnClickListener(new View.OnClickListener() {

			@Override
			public void onClick(View v) {
				x -= 10;
				y -= 10;
				// 调整相对于父控件的位置
				container.setTranslationX(x);
				container.setTranslationY(y);
			}
		});
		btnBig.setOnClickListener(new View.OnClickListener() {

			@Override
			public void onClick(View v) {

				AbsoluteLayout.LayoutParams layoutParams = (AbsoluteLayout.LayoutParams) container
						.getLayoutParams();
				// 调整大小
				layoutParams.width = container.getMeasuredWidth() + 10;
				layoutParams.height = container.getMeasuredHeight() + 10;
				container.setLayoutParams(layoutParams);
			}
		});
		btnSmall.setOnClickListener(new View.OnClickListener() {

			@Override
			public void onClick(View v) {

				AbsoluteLayout.LayoutParams layoutParams = (AbsoluteLayout.LayoutParams) container
						.getLayoutParams();
				// 调整大小
				layoutParams.width = container.getMeasuredWidth() - 10;
				layoutParams.height = container.getMeasuredHeight() - 10;
				container.setLayoutParams(layoutParams);
			}
		});
	}

	private void getViews() {
		container = (AbsoluteLayout) findViewById(R.id.container);
		textView1 = (TextView) findViewById(R.id.textView1);
		imageView1 = (ImageView) findViewById(R.id.imageView1);
		btnUp = (Button) findViewById(R.id.btnUp);
		btnDown = (Button) findViewById(R.id.btnDown);
		btnBig = (Button) findViewById(R.id.btnBig);
		btnSmall = (Button) findViewById(R.id.btnSmall);
	}
}
```