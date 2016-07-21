title: android的CoordinatorLayout的几个使用方法
categories: android
tags: CoordinatorLayout
date: 2016-07-21 17:02:29

---

<!--head-->

其实完全不太明白CoordinatorLayout，个人感觉就是可以动态布局。
原理请看参考文章。

### 效果图

![img](https://github.com/aotian16/Blog/blob/master/Study/Dev/Android/android%E7%9A%84CoordinatorLayout%E7%9A%84%E5%87%A0%E4%B8%AA%E4%BD%BF%E7%94%A8%E6%96%B9%E6%B3%95/android%E7%9A%84CoordinatorLayout%E7%9A%84%E5%87%A0%E4%B8%AA%E4%BD%BF%E7%94%A8%E6%96%B9%E6%B3%95.gif?raw=true)

1. 第一种，FloatingActionButton随着Snackbar移动
2. 第二种，AppBarLayout滚动消失与显示
3. 第三种，CollapsingToolbarLayout的展开与收缩

### 原文

[android的CoordinatorLayout的几个使用方法](http://qefee.com/2016/07/21/android%E7%9A%84CoordinatorLayout%E7%9A%84%E5%87%A0%E4%B8%AA%E4%BD%BF%E7%94%A8%E6%96%B9%E6%B3%95/)

### 代码

先添加依赖

```groovy
    compile 'com.android.support:appcompat-v7:24.1.0'
    compile 'com.android.support:design:24.1.0'
    compile 'com.android.support:palette-v7:24.1.0'
```

#### `AppBarLayoutActivity.java`

```java
package com.qefee.pj.testcoordinatorlayout;

import android.os.Bundle;
import android.support.design.widget.FloatingActionButton;
import android.support.design.widget.Snackbar;
import android.support.v4.content.ContextCompat;
import android.support.v7.app.AppCompatActivity;
import android.support.v7.widget.DefaultItemAnimator;
import android.support.v7.widget.LinearLayoutManager;
import android.support.v7.widget.OrientationHelper;
import android.support.v7.widget.RecyclerView;
import android.support.v7.widget.Toolbar;
import android.view.View;
import android.view.ViewGroup;
import android.widget.TextView;

public class AppBarLayoutActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_app_bar_layout);

        Toolbar toolbar = (Toolbar) findViewById(R.id.toolbar);
        setSupportActionBar(toolbar);

        RecyclerView recyclerView = (RecyclerView) findViewById(R.id.recyclerView);
        LinearLayoutManager layoutManager = new LinearLayoutManager(this);
        //设置布局管理器
        recyclerView.setLayoutManager(layoutManager);
        //设置为垂直布局，这也是默认的
        layoutManager.setOrientation(OrientationHelper.VERTICAL);
        //设置Adapter
        recyclerView.setAdapter(new RecyclerView.Adapter() {
            @Override
            public RecyclerView.ViewHolder onCreateViewHolder(ViewGroup parent, int viewType) {
                TextView tv = new TextView(AppBarLayoutActivity.this);
                ViewGroup.LayoutParams params = new ViewGroup.LayoutParams(
                        ViewGroup.LayoutParams.MATCH_PARENT,
                        ViewGroup.LayoutParams.WRAP_CONTENT
                );
                tv.setLayoutParams(params);

                RecyclerView.ViewHolder vh = new RecyclerView.ViewHolder(tv) {};
                return vh;
            }

            @Override
            public void onBindViewHolder(RecyclerView.ViewHolder holder, int position) {
                TextView tv = (TextView) holder.itemView;
                tv.setText("test --> " + position);
            }

            @Override
            public int getItemCount() {
                return 50;
            }
        });
        //设置分隔线
        //        recyclerView.addItemDecoration( new DividerGridItemDecoration(this ));
        //设置增加或删除条目的动画
        recyclerView.setItemAnimator(new DefaultItemAnimator());
    }
}
```

#### `CollapsingToolbarLayoutActivity.java`

```java
package com.qefee.pj.testcoordinatorlayout;

import android.graphics.Bitmap;
import android.graphics.BitmapFactory;
import android.os.Bundle;
import android.support.design.widget.CollapsingToolbarLayout;
import android.support.v7.app.ActionBar;
import android.support.v7.app.AppCompatActivity;
import android.support.v7.graphics.Palette;
import android.support.v7.widget.DefaultItemAnimator;
import android.support.v7.widget.LinearLayoutManager;
import android.support.v7.widget.OrientationHelper;
import android.support.v7.widget.RecyclerView;
import android.support.v7.widget.Toolbar;
import android.view.ViewGroup;
import android.widget.TextView;

public class CollapsingToolbarLayoutActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_collapsing_toolbar_layout);

        Toolbar toolbar = (Toolbar) findViewById(R.id.toolbar);
        setSupportActionBar(toolbar);

        ActionBar actionBar = getSupportActionBar();
        if (actionBar != null) {
            actionBar.setDisplayHomeAsUpEnabled(true);
        }
        final CollapsingToolbarLayout
                collapsingToolbar = (CollapsingToolbarLayout) findViewById(
                R.id.collapsing_toolbar);
        collapsingToolbar.setTitle(getString(R.string.app_name));
        collapsingToolbar.setExpandedTitleColor(getResources().getColor(android.R.color.transparent));

        Bitmap bitmap = BitmapFactory.decodeResource(getResources(), R.mipmap.ic_launcher);
        Palette.from(bitmap).generate(new Palette.PaletteAsyncListener() {
            @Override
            public void onGenerated(final Palette palette) {
                int defaultColor = getResources().getColor(R.color.colorPrimary);
                int defaultTitleColor = getResources().getColor(android.R.color.white);
                int bgColor = palette.getDarkVibrantColor(defaultColor);
                int titleColor = palette.getLightVibrantColor(defaultTitleColor);
                collapsingToolbar.setContentScrimColor(bgColor);
                collapsingToolbar.setCollapsedTitleTextColor(titleColor);
                collapsingToolbar.setExpandedTitleColor(titleColor);
            }
        });

        RecyclerView recyclerView = (RecyclerView) findViewById(R.id.recyclerView);
        LinearLayoutManager layoutManager = new LinearLayoutManager(this);
        //设置布局管理器
        recyclerView.setLayoutManager(layoutManager);
        //设置为垂直布局，这也是默认的
        layoutManager.setOrientation(OrientationHelper.VERTICAL);
        //设置Adapter
        recyclerView.setAdapter(new RecyclerView.Adapter() {
            @Override
            public RecyclerView.ViewHolder onCreateViewHolder(ViewGroup parent, int viewType) {
                TextView tv = new TextView(CollapsingToolbarLayoutActivity.this);
                ViewGroup.LayoutParams params = new ViewGroup.LayoutParams(
                        ViewGroup.LayoutParams.MATCH_PARENT,
                        ViewGroup.LayoutParams.WRAP_CONTENT
                );
                tv.setLayoutParams(params);

                RecyclerView.ViewHolder vh = new RecyclerView.ViewHolder(tv) {};
                return vh;
            }

            @Override
            public void onBindViewHolder(RecyclerView.ViewHolder holder, int position) {
                TextView tv = (TextView) holder.itemView;
                tv.setText("test --> " + position);
            }

            @Override
            public int getItemCount() {
                return 50;
            }
        });
        //设置分隔线
        //        recyclerView.addItemDecoration( new DividerGridItemDecoration(this ));
        //设置增加或删除条目的动画
        recyclerView.setItemAnimator(new DefaultItemAnimator());
    }
}
```

#### `FloatingActionButtonActivity.java`

```java
package com.qefee.pj.testcoordinatorlayout;

import android.os.Bundle;
import android.support.design.widget.FloatingActionButton;
import android.support.design.widget.Snackbar;
import android.support.v7.app.AppCompatActivity;
import android.support.v7.widget.Toolbar;
import android.view.View;

public class FloatingActionButtonActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_floating_action_button);
        Toolbar toolbar = (Toolbar) findViewById(R.id.toolbar);
        setSupportActionBar(toolbar);

        FloatingActionButton fab = (FloatingActionButton) findViewById(R.id.fab);
        fab.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                final Snackbar snackbar = Snackbar.make(view, "支持右滑消失", Snackbar.LENGTH_LONG);
                snackbar.setAction("点我试试", new View.OnClickListener() {
                    @Override
                    public void onClick(View view) {
                        snackbar.dismiss();
                    }
                }).show();
            }
        });
    }
}
```

#### `MainActivity.java`

```java
package com.qefee.pj.testcoordinatorlayout;

import android.content.Intent;
import android.os.Bundle;
import android.support.v7.app.AppCompatActivity;
import android.view.View;
import android.widget.Button;

public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        Button fabButton = (Button) findViewById(R.id.fabButton);
        Button ablButton = (Button) findViewById(R.id.ablButton);
        Button ctblButton = (Button) findViewById(R.id.ctblButton);

        fabButton.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                Intent intent = new Intent(MainActivity.this, FloatingActionButtonActivity.class);
                startActivity(intent);
            }
        });

        ablButton.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                Intent intent = new Intent(MainActivity.this, AppBarLayoutActivity.class);
                startActivity(intent);
            }
        });

        ctblButton.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                Intent intent = new Intent(MainActivity.this, CollapsingToolbarLayoutActivity.class);
                startActivity(intent);
            }
        });
    }
}
```

`activity_app_bar_layout.xml`

```xml
<?xml version="1.0" encoding="utf-8"?>
<android.support.design.widget.CoordinatorLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:fitsSystemWindows="true"
    tools:context="com.qefee.pj.testcoordinatorlayout.AppBarLayoutActivity">

    <android.support.design.widget.AppBarLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:theme="@style/AppTheme.AppBarOverlay">

        <android.support.v7.widget.Toolbar
            android:id="@+id/toolbar"
            android:layout_width="match_parent"
            android:layout_height="?attr/actionBarSize"
            android:background="?attr/colorPrimary"
            app:layout_scrollFlags="scroll|enterAlways"
            app:popupTheme="@style/AppTheme.PopupOverlay" />

    </android.support.design.widget.AppBarLayout>

    <android.support.v7.widget.RecyclerView
        android:id="@+id/recyclerView"
        app:layout_behavior="@string/appbar_scrolling_view_behavior"
        android:layout_width="match_parent"
        android:layout_height="match_parent" />

</android.support.design.widget.CoordinatorLayout>
```



`activity_collapsing_toolbar_layout.xml`

```xml
<?xml version="1.0" encoding="utf-8"?>
<android.support.design.widget.CoordinatorLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:fitsSystemWindows="true"
    tools:context="com.qefee.pj.testcoordinatorlayout.CollapsingToolbarLayoutActivity">

    <android.support.design.widget.AppBarLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:theme="@style/AppTheme.AppBarOverlay">

        <android.support.design.widget.CollapsingToolbarLayout
            android:id="@+id/collapsing_toolbar"
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            android:fitsSystemWindows="true"
            app:contentScrim="?attr/colorPrimary"
            app:layout_collapseParallaxMultiplier="0.6"
            app:expandedTitleMarginEnd="64dp"
            app:expandedTitleMarginStart="48dp"
            app:layout_scrollFlags="scroll|exitUntilCollapsed">

            <ImageView
                android:id="@+id/backdropImageView"
                android:layout_width="match_parent"
                android:layout_height="256dp"
                android:scaleType="centerCrop"
                android:fitsSystemWindows="true"
                android:src="@mipmap/ic_launcher"
                app:layout_collapseMode="parallax" />

            <android.support.v7.widget.Toolbar
                android:id="@+id/toolbar"
                android:layout_width="match_parent"
                android:layout_height="?attr/actionBarSize"
                app:popupTheme="@style/AppTheme.PopupOverlay"
                app:layout_collapseMode="pin" />
        </android.support.design.widget.CollapsingToolbarLayout>

    </android.support.design.widget.AppBarLayout>

    <android.support.v7.widget.RecyclerView
        android:id="@+id/recyclerView"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        app:layout_behavior="@string/appbar_scrolling_view_behavior" />

</android.support.design.widget.CoordinatorLayout>
```



`activity_floating_action_button.xml`

```xml
<?xml version="1.0" encoding="utf-8"?>
<android.support.design.widget.CoordinatorLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:fitsSystemWindows="true"
    tools:context="com.qefee.pj.testcoordinatorlayout.FloatingActionButtonActivity">

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="hello world!"/>

    <android.support.design.widget.FloatingActionButton
        android:id="@+id/fab"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_gravity="bottom|end"
        android:layout_margin="@dimen/fab_margin"
        android:src="@android:drawable/ic_dialog_email" />

</android.support.design.widget.CoordinatorLayout>
```



`activity_main.xml`

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:fitsSystemWindows="true"
    android:orientation="vertical"
    tools:context="com.qefee.pj.testcoordinatorlayout.MainActivity">

    <Button
        android:id="@+id/fabButton"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="FloatingActionButton"/>

    <Button
        android:id="@+id/ablButton"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="AppBarLayoutButton"/>

    <Button
        android:id="@+id/ctblButton"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="CollapsingToolbarLayoutButton"/>

</LinearLayout>
```



### 参考文章

* [android CoordinatorLayout使用](http://blog.csdn.net/xyz_lmn/article/details/48055919)

<!--more-->



<!--body-->
