title: android的EditText显示隐藏密码时候的全角问题
categories: android
tags: android
date: 2016-06-08 10:48:42

---

<!--head-->

切换EditText的密码为显示和隐藏的时候， 会有全角半角切换的问题。

原因是EditText是密码格式的时候， 默认是全角`MONOSPACE`的。

所以需要我们手动修改为`SANS_SERIF`。

**原文地址**

[Edittext password属性为true导致hint全角有关问题](http://www.myexception.cn/mobile/1817116.html)

<!--more-->

**代码**

MainActivity.java

```java
package com.qefee.pj.testtypeface;

import android.graphics.Typeface;
import android.os.Bundle;
import android.support.v7.app.AppCompatActivity;
import android.text.Editable;
import android.text.InputType;
import android.text.Selection;
import android.view.View;
import android.widget.Button;
import android.widget.EditText;

public class MainActivity extends AppCompatActivity {

    EditText editText;
    Button submitButton;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        editText = (EditText) findViewById(R.id.editText);
        submitButton = (Button) findViewById(R.id.submitButton);

        editText.setTypeface(Typeface.SANS_SERIF);

        submitButton.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                lookPwd(flag, editText);
                flag = !flag;
            }
        });
    }

    boolean flag;

    public void lookPwd(boolean hidePwd, EditText et) {
        if (hidePwd) {
            // 文本以密码形式显示
            et.setInputType(InputType.TYPE_CLASS_TEXT | InputType.TYPE_TEXT_VARIATION_PASSWORD);
            et.setTypeface(Typeface.SANS_SERIF);
            setSelection(et);
        } else {
            // 文本正常显示
            et.setInputType(InputType.TYPE_TEXT_VARIATION_VISIBLE_PASSWORD);
            et.setTypeface(Typeface.SANS_SERIF);
            setSelection(et);
        }
    }

    /*
     * 输入框光标一直在输入文本后面
     * @param et EditText
     */
    private void setSelection(EditText et) {
        Editable text = et.getText();
        Selection.setSelection(text, text.length());
    }
}
```

activity_main.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    tools:context="com.qefee.pj.testtypeface.MainActivity">

    <EditText
        android:id="@+id/editText"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:hint="12345abc,-"
        android:inputType="textPassword"
        android:typeface="sans"/>

    <Button
        android:id="@+id/submitButton"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="显示/隐藏 密码"/>
</LinearLayout>
```



<!--body-->
