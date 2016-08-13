title: android图片复制小工具
categories: tools
tags: tools
date: 2016-08-13 14:30:52

---

<!--head-->

用java做了个安卓图片复制的小工具，自己动手，丰衣足食。

[TOC]

<!--more-->

<!--body-->

## 原因

用sketch做好图后，导出的图片在一个文件夹内，命名大概如下：

1. my@mdpi.png
2. my@hdpi.png
3. my@xhdpi.png
4. my@xxhdpi.png

而安卓的图片的结构大概是这样的：

res

- drawable-mdpi
- drawable-hdpi    
- drawable-xhdpi
- drawable-xxhdpi

也就是说，图片复制的时候需要一个一个分开来放，比较麻烦。

如图所示：

![img](http://qefee.qiniudn.com/20160813_android%E5%9B%BE%E7%89%87%E5%A4%8D%E5%88%B6%E5%B0%8F%E5%B7%A5%E5%85%B7_%E5%9B%BE%E7%A4%BA.png)

## 代码

所以就用java写了个jar来自动完成。

代码如下：

```java
package com.qefee.pj.drawablecopy;

import java.io.File;
import java.io.FileFilter;
import java.io.IOException;

public class Application {
    public static void main(String[] args) {

        if (args.length >= 2) {
            String drawableFolderString = args[0];
            File drawableFolder = new File(drawableFolderString);

            String toDirString = args[1];

            File toDir = new File(toDirString);

            FileFilter fileFilter = new FileFilter() {
                public boolean accept(File pathname) {
                    boolean pngFlag = pathname.getName().endsWith(".png");
                    boolean jpgFlag = pathname.getName().endsWith(".jpg");
                    return pngFlag || jpgFlag;
                }
            };
            DrawableCopyWalker directoryWalker = new DrawableCopyWalker(fileFilter, toDir);
            try {
                directoryWalker.start(drawableFolder);
            } catch (IOException e) {
                e.printStackTrace();
            }
        } else {
            echo("参数错误");
        }
    }

    private static void echo(String msg) {
        System.out.println(msg);
    }
}
```

```java
package com.qefee.pj.drawablecopy;

import org.apache.commons.io.DirectoryWalker;
import org.apache.commons.io.FileUtils;
import org.apache.commons.lang3.StringUtils;

import java.io.File;
import java.io.FileFilter;
import java.io.IOException;
import java.util.ArrayList;
import java.util.Collection;

public class DrawableCopyWalker extends DirectoryWalker {

    private File toDir;

    DrawableCopyWalker(FileFilter filter, File toDir) {
        super(filter, -1);

        this.toDir = toDir;
    }

    @Override
    protected void handleFile(File file, int depth, Collection results) throws IOException {
        super.handleFile(file, depth, results);

        String fileName = file.getName();


        String mdpi = "@mdpi";
        String hdpi = "@hdpi";
        String xhdpi = "@xhdpi";
        String xxhdpi = "@xxhdpi";
        String xxxhdpi = "@xxxhdpi";

        // app icon
        if (fileName.contains("ic_launcher")) {
            if (fileName.contains(mdpi)) {
                copyImage(file, fileName, mdpi, toDir, "mipmap-mdpi");
            } else if (fileName.contains(hdpi)) {
                copyImage(file, fileName, hdpi, toDir, "mipmap-hdpi");
            } else if (fileName.contains(xhdpi)) {
                copyImage(file, fileName, xhdpi, toDir, "mipmap-xhdpi");
            } else if (fileName.contains(xxhdpi)) {
                copyImage(file, fileName, xxhdpi, toDir, "mipmap-xxhdpi");
            } else if (fileName.contains(xxxhdpi)) {
                copyImage(file, fileName, xxxhdpi, toDir, "mipmap-xxxhdpi");
            } else {
                System.out.println("文件未处理 : " + file.getAbsolutePath());
            }
        } else {
            // 普通图片
            if (fileName.contains(mdpi)) {
                copyImage(file, fileName, mdpi, toDir, "drawable-mdpi");
            } else if (fileName.contains(hdpi)) {
                copyImage(file, fileName, hdpi, toDir, "drawable-hdpi");
            } else if (fileName.contains(xhdpi)) {
                copyImage(file, fileName, xhdpi, toDir, "drawable-xhdpi");
            } else if (fileName.contains(xxhdpi)) {
                copyImage(file, fileName, xxhdpi, toDir, "drawable-xxhdpi");
            } else {
                System.out.println("文件未处理 : " + file.getAbsolutePath());
            }
        }
    }

    private void copyImage(File file, String fileName, String mdpi, File toDir, String child) throws IOException {
        File mdpiDir = new File(toDir, child);
        String newFileName = StringUtils.remove(fileName, mdpi);
        File newFile = new File(mdpiDir, newFileName);
        FileUtils.copyFile(file, newFile);
    }

    void start(File startDirectory) throws IOException {
        ArrayList<File> dirs = new ArrayList<File>();
        this.walk(startDirectory, dirs);
    }

}
```

maven文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.qefee.pj</groupId>
    <artifactId>TestMaven</artifactId>
    <version>1.0-SNAPSHOT</version>

    <dependencies>
        <!-- https://mvnrepository.com/artifact/org.apache.commons/commons-lang3 -->
        <dependency>
            <groupId>org.apache.commons</groupId>
            <artifactId>commons-lang3</artifactId>
            <version>3.4</version>
        </dependency>
        <!-- https://mvnrepository.com/artifact/commons-io/commons-io -->
        <dependency>
            <groupId>commons-io</groupId>
            <artifactId>commons-io</artifactId>
            <version>2.5</version>
        </dependency>

    </dependencies>

</project>
```

## 使用方法

导出jar后再命令行中执行

其中,`fromFolder`是图片文件夹，`toFolder`指向安卓的res文件夹

```shell
java -jar copyImage.jar fromFolder toFolder(res)
```