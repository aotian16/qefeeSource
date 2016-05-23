title: android录音和播放录音
categories: android
tags: record
date: 2016-05-23 19:00:00

---

<!--head-->

android中使用`MediaRecorder`录音， 然后使用`MediaPlayer`播放录音.

[TOC]

<!--more-->

# From

[android录音和播放录音](http://qefee.com/2016/05/23/android%E5%BD%95%E9%9F%B3%E5%92%8C%E6%92%AD%E6%94%BE%E5%BD%95%E9%9F%B3/)

# Source

## activity_main.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:paddingBottom="@dimen/activity_vertical_margin"
    android:paddingLeft="@dimen/activity_horizontal_margin"
    android:paddingRight="@dimen/activity_horizontal_margin"
    android:paddingTop="@dimen/activity_vertical_margin"
    tools:context="com.qefee.pj.testrecord.MainActivity">


    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:orientation="vertical">
        
        <Button
            android:id="@+id/startRecordButton"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:text="@string/startRecord"/>

        <Button
            android:id="@+id/stopRecordButton"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:text="@string/stopRecord"/>
        
        <Button
            android:id="@+id/playRecordButton"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:text="@string/playRecord"/>

        <Button
            android:id="@+id/stopPlayRecordButton"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:text="@string/stopPlayRecord"/>

        <Button
            android:id="@+id/recordInfoButton"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:text="@string/recordInfo"/>

        <TextView
            android:id="@+id/recordInfoTextView"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:text="@string/recordInfo"/>
    </LinearLayout>
</RelativeLayout>
```

## MainActivity.java

```java
package com.qefee.pj.testrecord;

import android.media.MediaPlayer;
import android.media.MediaRecorder;
import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;
import android.util.Log;
import android.view.View;
import android.widget.Button;
import android.widget.TextView;

import java.io.File;
import java.io.IOException;

public class MainActivity extends AppCompatActivity {

    private static final String TAG = "MainActivity";

    /**
     * 开始录音按钮.
     */
    Button startRecordButton;
    /**
     * 停止录音按钮.
     */
    Button stopRecordButton;
    /**
     * 播放录音按钮.
     */
    Button playRecordButton;
    /**
     * 停止播放录音按钮.
     */
    Button stopPlayRecordButton;
    /**
     * 录音信息按钮.
     */
    Button recordInfoButton;
    /**
     * 录音信息TextView.
     */
    TextView recordInfoTextView;

    /**
     * 播放器对象.
     */
    private MediaPlayer mediaPlayer;
    /**
     * 录音器对象.
     */
    private MediaRecorder mediaRecorder;

    /**
     * 录音文件名.
     */
    private String recordFileName;
    /**
     * 录音文件.
     */
    private File recordFile;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        recordFileName = "r.3gp";
        recordFile = new File(getExternalCacheDir(), recordFileName);

        startRecordButton = (Button) findViewById(R.id.startRecordButton);
        stopRecordButton = (Button) findViewById(R.id.stopRecordButton);
        playRecordButton = (Button) findViewById(R.id.playRecordButton);
        stopPlayRecordButton = (Button) findViewById(R.id.stopPlayRecordButton);
        recordInfoButton = (Button) findViewById(R.id.recordInfoButton);

        recordInfoTextView = (TextView) findViewById(R.id.recordInfoTextView);

        startRecordButton.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                try {
                    mediaRecorder = new MediaRecorder();
                    mediaRecorder.setAudioSource(MediaRecorder.AudioSource.DEFAULT);
                    mediaRecorder.setOutputFormat(MediaRecorder.OutputFormat.THREE_GPP);
                    mediaRecorder.setOutputFile(recordFile.getAbsolutePath());
                    mediaRecorder.setAudioEncoder(MediaRecorder.AudioEncoder.AMR_NB);
                    mediaRecorder.prepare();
                    mediaRecorder.start();

                    Log.i(TAG, "onClick: 录音开始");
                } catch (IOException e) {
                    Log.e(TAG, "onClick: 录音失败", e);
                } catch (IllegalStateException e) {
                    Log.e(TAG, "onClick: 录音失败", e);
                }
            }
        });

        stopRecordButton.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {

                if (mediaRecorder != null) {
                    try {
                        mediaRecorder.stop();
                        mediaRecorder.release();
                        mediaRecorder = null;

                        Log.i(TAG, "onClick: 录音停止");
                    } catch (IllegalStateException e) {
                        Log.e(TAG, "onClick: 录音状态不正确", e);
                    }
                } else {
                    Log.i(TAG, "onClick: 录音器没有准备好");
                }
            }
        });

        playRecordButton.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {

                if (recordFile.exists()) {
                    try {
                        mediaPlayer = new MediaPlayer();

                        mediaPlayer.setDataSource(recordFile.getAbsolutePath());
                        mediaPlayer.prepare();
                        mediaPlayer.start();

                        Log.i(TAG, "onClick: 播放录音开始");
                    } catch (IOException e) {
                        Log.e(TAG, "onClick: 播放录音失败", e);
                    }
                } else {
                    Log.i(TAG, "onClick: 没有录音文件");
                }


            }
        });

        stopPlayRecordButton.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                if (mediaPlayer != null) {
                    mediaPlayer.release();
                    mediaPlayer = null;

                    Log.i(TAG, "onClick: 停止播放录音");
                } else {
                    Log.i(TAG, "onClick: 播放器没有准备好");
                }
            }
        });

        recordInfoButton.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                if (mediaPlayer != null) {
                    int milliseconds = mediaPlayer.getDuration();

                    recordInfoTextView.setText(String.valueOf(milliseconds));

                    Log.i(TAG, "onClick: 获取录音信息");
                } else {
                    Log.i(TAG, "onClick: 播放器没有准备好");
                }
            }
        });
    }
}
```

## strings.xml

```xml
<resources>
    <string name="app_name">TestRecord</string>

    <string name="startRecord">开始录音</string>
    <string name="stopRecord">停止录音</string>
    <string name="playRecord">播放录音</string>
    <string name="stopPlayRecord">停播录音</string>
    <string name="recordInfo">录音信息</string>
</resources>
```

## AndroidManifest.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.qefee.pj.testrecord">

    <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
    <uses-permission android:name="android.permission.MOUNT_UNMOUNT_FILESYSTEMS"/>
    <uses-permission android:name="android.permission.RECORD_AUDIO" />

    <application
        android:allowBackup="true"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:supportsRtl="true"
        android:theme="@style/AppTheme">
        <activity android:name=".MainActivity">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />

                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
    </application>

</manifest>
```

# reference

 [[Android\] 录音与播放录音实现](http://blog.csdn.net/cxf7394373/article/details/8313980)



<!--body-->
