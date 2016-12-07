title: android中用DialogFragment实现底部弹框
categories: qefee
tags: qefee
date: 2016-12-01 16:31:52

---

<!--head-->

## 效果图

![img](http://qefee.qiniudn.com/20161201_android_DialogFragment_bottom.gif)

<!--more-->

<!--body-->

## 代码

需要注意的地方用注释给出。

可以在github上下载源码。[点我试试](https://github.com/aotian16/AndroidStudy/tree/master/projects/TestDialogFragment)。



### Dialog2

Dialog2继承自DialogFragment

```java
package com.qefee.pj.testdialogfragment.dialog;

import android.app.DialogFragment;
import android.graphics.drawable.ColorDrawable;
import android.os.Bundle;
import android.support.annotation.Nullable;
import android.support.v4.content.ContextCompat;
import android.util.Log;
import android.view.Gravity;
import android.view.LayoutInflater;
import android.view.View;
import android.view.ViewGroup;
import android.view.Window;
import android.view.WindowManager;
import android.view.animation.Animation;
import android.view.animation.TranslateAnimation;
import android.widget.Button;
import android.widget.Toast;

import com.qefee.pj.testdialogfragment.R;

public class Dialog2 extends DialogFragment {
    private static final String TAG = "Dialog2";

    public static Dialog2 newInstance() {
        return new Dialog2();
    }

    @Override
    public void onStart() {
        super.onStart();

        Log.i(TAG, "onStart: ");

        Window window = getDialog().getWindow();
        WindowManager.LayoutParams params = window.getAttributes();
        params.gravity = Gravity.BOTTOM; // 显示在底部
        params.width = WindowManager.LayoutParams.MATCH_PARENT; // 宽度填充满屏
        window.setAttributes(params);

        // 这里用透明颜色替换掉系统自带背景
        int color = ContextCompat.getColor(getActivity(), android.R.color.transparent);
        window.setBackgroundDrawable(new ColorDrawable(color));
    }

    @Nullable
    @Override
    public View onCreateView(LayoutInflater inflater, ViewGroup container, Bundle savedInstanceState) {

        Log.i(TAG, "onCreateView: ");

        getDialog().requestWindowFeature(Window.FEATURE_NO_TITLE); // 不显示标题栏

        final View dialogView = inflater.inflate(R.layout.dialog_2, container, false);

        Button okButton = (Button) dialogView.findViewById(R.id.okButton);
        Button cancelButton = (Button) dialogView.findViewById(R.id.cancelButton);

        okButton.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                Toast.makeText(getActivity(), "OK clicked", Toast.LENGTH_SHORT).show();
                startDownAnimation(dialogView);
            }
        });

        cancelButton.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                Toast.makeText(getActivity(), "Cancel clicked", Toast.LENGTH_SHORT).show();
                startDownAnimation(dialogView);
            }
        });

        startUpAnimation(dialogView);

        return dialogView;
    }

    private void startUpAnimation(View view) {
        Animation slide = new TranslateAnimation(Animation.RELATIVE_TO_SELF, 0.0f,
                Animation.RELATIVE_TO_SELF, 0.0f, Animation.RELATIVE_TO_SELF,
                1.0f, Animation.RELATIVE_TO_SELF, 0.0f);

        slide.setDuration(400);
        slide.setFillAfter(true);
        slide.setFillEnabled(true);
        view.startAnimation(slide);
    }

    private void startDownAnimation(View view) {
        Animation slide = new TranslateAnimation(Animation.RELATIVE_TO_SELF, 0.0f,
                Animation.RELATIVE_TO_SELF, 0.0f, Animation.RELATIVE_TO_SELF,
                0.0f, Animation.RELATIVE_TO_SELF, 1.0f);

        slide.setDuration(400);
        slide.setFillAfter(true);
        slide.setFillEnabled(true);
        slide.setAnimationListener(new Animation.AnimationListener() {
            @Override
            public void onAnimationStart(Animation animation) {

            }

            @Override
            public void onAnimationEnd(Animation animation) {
                dismiss();
            }

            @Override
            public void onAnimationRepeat(Animation animation) {

            }
        });
        view.startAnimation(slide);
    }
}

```
### dialog_2

很简单的样式文件

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:background="@color/colorAccent">

    <TextView
        android:id="@+id/titleTextView"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="this is title" />

    <TextView
        android:id="@+id/contentTextView"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="this is content" />

    <Button
        android:id="@+id/okButton"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="OK" />

    <Button
        android:id="@+id/cancelButton"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="cancel" />

</LinearLayout>

```
### 使用方法

```java
Dialog2.newInstance().show(getFragmentManager(), "dialogTag2");
```

## 参考文章

- [使用DialogFragment实现底部菜单](http://www.jianshu.com/p/8d420b668eda)
- [Android 官方推荐 : DialogFragment 创建对话框](http://blog.csdn.net/lmj623565791/article/details/37815413)
- [详细解读DialogFragment](http://www.cnblogs.com/mercuryli/archive/2016/04/09/5372496.html)

## 原文

- [android中用DialogFragment实现底部弹框](http://qefee.com/2016/12/01/android%E4%B8%AD%E7%94%A8DialogFragment%E5%AE%9E%E7%8E%B0%E5%BA%95%E9%83%A8%E5%BC%B9%E6%A1%86/)

