title: android中用RecyclerView实现滑动删除与切换item
categories: qefee
tags: qefee
date: 2016-11-23 10:47:34

---

<!--head-->

[TOC]

## 效果图

![img](http://qefee.qiniudn.com/20161123_android中用RecyclerView实现滑动删除与切换item.gif)

<!--more-->

<!--body-->

## 代码

需要注意的地方用注释给出。

可以在github上下载源码。[点我试试](https://github.com/aotian16/AndroidStudy/tree/master/projects/TestRecyclerView)。


### RecyclerView1Activity.java

```java
package com.qefee.pj.testrecyclerview;

import android.content.Context;
import android.os.Bundle;
import android.support.design.widget.FloatingActionButton;
import android.support.design.widget.Snackbar;
import android.support.v7.app.ActionBar;
import android.support.v7.app.AppCompatActivity;
import android.support.v7.widget.DefaultItemAnimator;
import android.support.v7.widget.DividerItemDecoration;
import android.support.v7.widget.LinearLayoutManager;
import android.support.v7.widget.RecyclerView;
import android.support.v7.widget.Toolbar;
import android.support.v7.widget.helper.ItemTouchHelper;
import android.view.LayoutInflater;
import android.view.Menu;
import android.view.MenuItem;
import android.view.View;
import android.view.ViewGroup;
import android.widget.TextView;
import android.widget.Toast;

import java.util.ArrayList;
import java.util.Collections;
import java.util.List;
import java.util.Random;

public class RecyclerView1Activity extends AppCompatActivity {

    /**
     * 数据集合.
     */
    private List<String> dataList;
    /**
     * RecyclerView的适配器.
     */
    private MyAdapter adapter;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_recycler_view1);
        initActionBar();
        initFloatingActionButton();

        initDataList();
        initRecyclerView();
    }

    private void initFloatingActionButton() {
        FloatingActionButton fab = (FloatingActionButton) findViewById(R.id.fab);
        fab.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                Snackbar.make(view, "Replace with your own action", Snackbar.LENGTH_LONG)
                        .setAction("Action", null).show();
            }
        });
    }

    private void initActionBar() {
        Toolbar toolbar = (Toolbar) findViewById(R.id.toolbar);
        setSupportActionBar(toolbar);
        ActionBar actionBar = getSupportActionBar();

        if (actionBar != null) {
            actionBar.setDisplayHomeAsUpEnabled(true);
        }
    }

    private void initRecyclerView() {
        RecyclerView recyclerView = (RecyclerView) findViewById(R.id.recyclerView);
        recyclerView.setLayoutManager(new LinearLayoutManager(this)); // 布局管理器
        adapter = new MyAdapter(this, dataList);
        recyclerView.setAdapter(adapter);
        recyclerView.addItemDecoration(new DividerItemDecoration(this, DividerItemDecoration.VERTICAL)); // 分割线
        recyclerView.setItemAnimator(new DefaultItemAnimator()); // 动画效果
        MyItemTouchHelperCallback callback = new MyItemTouchHelperCallback(adapter); // 监听touch事件，拖拽和滑动等
        ItemTouchHelper itemTouchHelper = new ItemTouchHelper(callback);
        itemTouchHelper.attachToRecyclerView(recyclerView); // 绑定到RecyclerView
    }

    private void initDataList() {
        dataList = new ArrayList<>();

        for (int i = 0; i < 20; i++) {
            dataList.add(String.format("item %d", i));
        }
    }

    class MyItemTouchHelperCallback extends ItemTouchHelper.Callback {

        MyAdapter adapter;

        MyItemTouchHelperCallback(MyAdapter adapter) {
            this.adapter = adapter;
        }

        @Override
        public boolean isLongPressDragEnabled() {
            return true;
        }

        @Override
        public boolean isItemViewSwipeEnabled() {
            return true;
        }

        @Override
        public int getMovementFlags(RecyclerView recyclerView, RecyclerView.ViewHolder viewHolder) {
            int dragFlags = ItemTouchHelper.UP | ItemTouchHelper.DOWN; // 拖拽方向
            int swipeFlags = ItemTouchHelper.START | ItemTouchHelper.END; // 滑动方向
            return makeMovementFlags(dragFlags, swipeFlags);
        }

        @Override
        public boolean onMove(RecyclerView recyclerView, RecyclerView.ViewHolder viewHolder, RecyclerView.ViewHolder target) {
            int from = viewHolder.getAdapterPosition();
            int to = target.getAdapterPosition();
            Collections.swap(adapter.dataList, from, to); // 移动item后要交换数据
            adapter.notifyItemMoved(from, to); // 通知适配器数据已经移动
            return true;
        }

        @Override
        public void onSwiped(RecyclerView.ViewHolder viewHolder, int direction) {
            switch (direction) {
                case ItemTouchHelper.START:
                case ItemTouchHelper.END:
                    int adapterPosition = viewHolder.getAdapterPosition();
                    adapter.dataList.remove(adapterPosition); // 滑动item后删除数据
                    adapter.notifyItemRemoved(adapterPosition); // 通知适配器数据已经删除
                    break;
                default:
                    break;
            }
        }
    }


    class MyAdapter extends RecyclerView.Adapter<MyAdapter.MyViewHolder> {

        Context context;
        List<String> dataList;

        MyAdapter(Context context, List<String> dataList) {
            this.context = context;
            this.dataList = dataList;
        }

        @Override
        public MyViewHolder onCreateViewHolder(ViewGroup parent, int viewType) {

            LayoutInflater inflater = LayoutInflater.from(context);
            View view = inflater.inflate(android.R.layout.simple_list_item_1, parent, false);
            MyViewHolder vh = new MyViewHolder(view);

            return vh;
        }

        @Override
        public void onBindViewHolder(MyViewHolder holder, int position) {
            holder.text1.setText(dataList.get(position));
        }

        @Override
        public int getItemCount() {
            return dataList.size();
        }

        class MyViewHolder extends RecyclerView.ViewHolder {

            TextView text1;

            MyViewHolder(View itemView) {
                super(itemView);

                text1 = (TextView) itemView;

                text1.setOnClickListener(new View.OnClickListener() {
                    @Override
                    public void onClick(View view) {
                        Toast.makeText(RecyclerView1Activity.this, String.format("text1 on click. index = %d", getAdapterPosition()), Toast.LENGTH_SHORT).show();
                    }
                });

                text1.setOnLongClickListener(new View.OnLongClickListener() {
                    @Override
                    public boolean onLongClick(View view) {
                        Toast.makeText(RecyclerView1Activity.this, String.format("text1 on long click. index = %d", getAdapterPosition()), Toast.LENGTH_SHORT).show();
                        return true;
                    }
                });
            }
        }
    }

    @Override
    public boolean onCreateOptionsMenu(Menu menu) {
        // Inflate the menu; this adds items to the action bar if it is present.
        getMenuInflater().inflate(R.menu.menu_recycler_view_1, menu);
        return true;
    }

    @Override
    public boolean onOptionsItemSelected(MenuItem item) {
        // Handle action bar item clicks here. The action bar will
        // automatically handle clicks on the Home/Up button, so long
        // as you specify a parent activity in AndroidManifest.xml.
        int id = item.getItemId();

        //noinspection SimplifiableIfStatement
        switch (id) {
            case R.id.itemAdd:


                if (dataList.size() > 0) {
                    Random random = new Random();
                    int nextInt = random.nextInt(dataList.size());
                    dataList.add(nextInt, String.format("这里是添加项 %d", nextInt));

                    adapter.notifyItemInserted(nextInt);
                    return true;
                } else {
                    int i = 0;
                    dataList.add(i, String.format("这里是添加项 %d", i));

                    adapter.notifyItemInserted(i);
                }

                return true;
            case R.id.itemDelete:

                if (dataList.size() > 0) {
                    Random random1 = new Random();
                    int nextInt1 = random1.nextInt(dataList.size());
                    dataList.remove(nextInt1);

                    adapter.notifyItemRemoved(nextInt1);
                    return true;
                } else {
                    Toast.makeText(this, "没有可删的了", Toast.LENGTH_SHORT).show();
                }
            default:
                break;
        }

        return super.onOptionsItemSelected(item);
    }

}

```
### activity_recycler_view1.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<android.support.design.widget.CoordinatorLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:fitsSystemWindows="true"
    tools:context="com.qefee.pj.testrecyclerview.RecyclerView1Activity">

    <android.support.design.widget.AppBarLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:theme="@style/AppTheme.AppBarOverlay">

        <android.support.v7.widget.Toolbar
            android:id="@+id/toolbar"
            android:layout_width="match_parent"
            android:layout_height="?attr/actionBarSize"
            android:background="?attr/colorPrimary"
            app:popupTheme="@style/AppTheme.PopupOverlay" />

    </android.support.design.widget.AppBarLayout>

    <include layout="@layout/content_recycler_view1" />

    <android.support.design.widget.FloatingActionButton
        android:id="@+id/fab"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_gravity="bottom|end"
        android:layout_margin="@dimen/fab_margin"
        app:srcCompat="@android:drawable/ic_dialog_email" />

</android.support.design.widget.CoordinatorLayout>

```
### content_recycler_view1.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/content_recycler_view1"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:paddingBottom="@dimen/activity_vertical_margin"
    android:paddingLeft="@dimen/activity_horizontal_margin"
    android:paddingRight="@dimen/activity_horizontal_margin"
    android:paddingTop="@dimen/activity_vertical_margin"
    app:layout_behavior="@string/appbar_scrolling_view_behavior"
    tools:context="com.qefee.pj.testrecyclerview.RecyclerView1Activity"
    tools:showIn="@layout/activity_recycler_view1">

    <android.support.v7.widget.RecyclerView
        android:id="@+id/recyclerView"
        android:layout_width="match_parent"
        android:layout_height="match_parent" />

</RelativeLayout>
```
## 参考文章

- [RecyclerView的拖动和滑动 第一部分 ：基本的ItemTouchHelper示例](http://www.jcodecraeer.com/a/anzhuokaifa/androidkaifa/2015/0630/3123.html)

- [Android RecyclerView 使用完全解析 体验艺术般的控件](http://blog.csdn.net/lmj623565791/article/details/45059587)

## 原文

- [android中用RecyclerView实现滑动删除与切换item](http://qefee.com/2016/11/23/android%E4%B8%AD%E7%94%A8RecyclerView%E5%AE%9E%E7%8E%B0%E6%BB%91%E5%8A%A8%E5%88%A0%E9%99%A4%E4%B8%8E%E5%88%87%E6%8D%A2item/)

