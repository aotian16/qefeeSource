title: android中ImageView切换图片
date: 2014-03-10 22:37:29
tags: android
categories: android
---

<!--head-->

主要是ImageView的setImageBitmap和setImageDrawable方法。

* setImageBitmap使用外部图片
* setImageDrawable使用drawable资源

参考文章[切换imageView的几种办法](http://blog.csdn.net/h3c4lenovo/article/details/7669455 "切换imageView的几种办法")

**代码如下**

<!--more-->

<!--body-->

``` java
package com.example.pj008_imageview;

import android.app.Activity;
import android.os.Bundle;
import android.util.Log;
import android.view.View;
import android.widget.Button;
import android.widget.ImageView;

public class MainActivity extends Activity {

	ImageView imageView1;
	Button nextBtn;

	int clickCounter;
	int imageNum;

	@Override
	protected void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
		setContentView(R.layout.activity_main);

		imageView1 = (ImageView) findViewById(R.id.imageView1);
		nextBtn = (Button) findViewById(R.id.nextBtn);

		nextBtn.setOnClickListener(new View.OnClickListener() {

			@Override
			public void onClick(View v) {
				clickCounter++;
				imageNum = clickCounter % 4;

				switch (imageNum) {
				case 0:
					imageView1.setImageDrawable(getResources().getDrawable(
							R.drawable.left));
					break;
				case 1:
					imageView1.setImageDrawable(getResources().getDrawable(
							R.drawable.right));
					break;
				case 2:
					imageView1.setImageDrawable(getResources().getDrawable(
							R.drawable.up));
					break;
				case 3:
					imageView1.setImageDrawable(getResources().getDrawable(
							R.drawable.down));
					break;

				default:
					Log.e("Error", "imageNum error.");
					break;
				}

			}
		});
	}
}

```