title: android屏幕录制和转gif
categories: android
tags: screenrecord
date: 2016-07-20 16:20:06

---

<!--head-->

`screenrecord`命令可以用于android设备的屏幕录制（Android 4.4 (API level 19) 以上）。

**原文**

[android屏幕录制和转gif](http://qefee.com/2016/07/20/android%E5%B1%8F%E5%B9%95%E5%BD%95%E5%88%B6%E5%92%8C%E8%BD%ACgif/)

<!--more-->

<!--body-->

# `screenrecord`命令详细介绍

输入以下命令就会打印出`screenrecord`命令的详细帮助信息。

```shell
adb shell screenrecord --help
```

大概翻译一下

```
命令格式: screenrecord [options] <filename>

版本号 v1.2.  录制屏幕命令，文件格式为mp4.

选项:
--size WIDTHxHEIGHT
    设置视频大小, e.g. "1280x720".  默认是设备尺寸.
--bit-rate RATE
    设置视频的比特率.  
    可以设置为两种单位：
    第一种 比特 比如 '4000000'
    第二种 兆 比如 '4M'
    
    '4000000'等效于'4M'
    默认4M
--bugreport
    错误报告
--time-limit TIME
    设置录制时间，单位为秒.  默认时间180.
--verbose
    在命令行打印相关信息.
--help
    显示帮助信息.

使用 Ctrl-C 提前停止录屏.
```

# `screenrecord`命令demo

1，开始录屏

```shell
adb shell screenrecord --verbose /sdcard/s.mp4
```

2，停止录屏

Ctrl-C

3，拉取视频文件到本地

```shell
adb pull /sdcard/s.mp4
```

# 转gif

推荐使用[licecap](http://www.cockos.com/licecap/)

# 参考文章

* [Android手机如何录制屏幕及转GIF](https://www.aswifter.com/2015/07/10/android-record-video-to-gif/?utm_source=tuicool&utm_medium=referral)

* [Recording a device screen](https://developer.android.com/studio/command-line/shell.html)