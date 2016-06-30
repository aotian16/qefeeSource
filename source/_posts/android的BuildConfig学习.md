title: android的BuildConfig学习
categories: android
tags: BuildConfig
date: 2016-06-28 14:17:20

---

<!--head-->

[TOC]

本文是自学BuildConfig的一些小知识点，希望对你有所帮助。

**原文**

[android的BuildConfig学习](http://qefee.com/2016/06/28/android%E7%9A%84BuildConfig%E5%AD%A6%E4%B9%A0/)



<!--more-->

<!--body-->

# BuildConfig是什么？

BuildConfig是android在编译过程中自动生成的一个配置文件。

在不同的编译模式下会生成不同的变量，我们可以利用这些变量来方便不同编译环境下的开发，比如日志的打印（开发环境下可以打印Verbose一级，发布环境下可以打印Warn一级）。

# BuildConfig有哪些变量？

没有自己变动过gradle文件的话，自动生成的BuildConfig一般如下文所示。

```java
public final class BuildConfig {
  public static final boolean DEBUG = Boolean.parseBoolean("true");
  public static final String APPLICATION_ID = "com.xxxx.xxx.xx";
  public static final String BUILD_TYPE = "debug";
  public static final String FLAVOR = "";
  public static final int VERSION_CODE = 1;
  public static final String VERSION_NAME = "1.0";
}
```

# BuildConfig在哪里？

如图所示

![path](https://github.com/aotian16/Blog/blob/master/Study/Dev/Android/android%E7%9A%84BuildConfig%E5%AD%A6%E4%B9%A0/android%E7%9A%84BuildConfig%E5%AD%A6%E4%B9%A0.png?raw=true)

# BuildConfig如何使用？

同java常量。

```java
public class MainActivity extends AppCompatActivity {
    
    TextView msgText;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        String lineSep = System.getProperty("line.separator", "\n");

        msgText = (TextView) findViewById(R.id.msgText);

        String DEBUG = "DEBUG = "
                + BuildConfig.DEBUG
                + lineSep;
        String APPLICATION_ID = "APPLICATION_ID = "
                + BuildConfig.APPLICATION_ID
                + lineSep;
        String BUILD_TYPE = "BUILD_TYPE = "
                + BuildConfig.BUILD_TYPE
                + lineSep;
        String FLAVOR = "FLAVOR = "
                + BuildConfig.FLAVOR
                + lineSep;
        String VERSION_CODE = "VERSION_CODE = "
                + BuildConfig.VERSION_CODE
                + lineSep;
        String VERSION_NAME = "VERSION_NAME = "
                + BuildConfig.VERSION_NAME;

        String msg = DEBUG
                + APPLICATION_ID
                + BUILD_TYPE
                + FLAVOR
                + VERSION_CODE
                + VERSION_NAME;

        msgText.setText(msg);
    }
}
```



# BuildConfig可以添加其他变量吗？

可以的。

在app模块的build.gradle中（不是Project的），有个buildTypes节点，我们修改如下。

```javascript
    buildTypes {
        debug {
            buildConfigField "int", "myInt", "0"
            buildConfigField "String", "myStr", "\"hello\""
            buildConfigField "boolean", "myFlag", "true"

        }
        release {
            buildConfigField "int", "myInt", "1"
            buildConfigField "String", "myStr", "\"world\""
            buildConfigField "boolean", "myFlag", "true"

            minifyEnabled false
            proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'
        }
    }
```

其中`myInt`，`myStr`，`myFlag`是我们自己定义的，编译后的`BuildConfig`为

```java
public final class BuildConfig {
  public static final boolean DEBUG = Boolean.parseBoolean("true");
  public static final String APPLICATION_ID = "com.qefee.pj.testbuildconfig";
  public static final String BUILD_TYPE = "debug";
  public static final String FLAVOR = "";
  public static final int VERSION_CODE = 1;
  public static final String VERSION_NAME = "1.0";
  // Fields from build type: debug
  public static final boolean myFlag = true;
  public static final int myInt = 0;
  public static final String myStr = "hello";
}
```

可以看到系统已经为我们生成了.

# 参考文章

* [Android BuildConfig.DEBUG的使用](http://blog.csdn.net/icewst/article/details/39048881)

* [Pro Tip: Android BuildConfig](http://buccaneer.io/2015/10/22/pro-tip-android-buildconfig/)


