title: Guava学习
categories:  java
tags: guava
date: 2016-04-15 21:10:40

---

<!--head-->

学习下google的Guava包.

写几个demo试试.

github地址: **[guava](https://github.com/google/guava)**

<!--more-->

<!--body-->

[TOC]

# 1, 基本工具 [Basic utilities]

## 1.1 使用和避免null：null是模棱两可的，会引起令人困惑的错误，有些时候它让人很不舒服。很多Guava工具类用快速失败拒绝null值，而不是盲目地接受

### 创建Optional实例（以下都是静态方法）：

| [Optional.of(T)](http://docs.guava-libraries.googlecode.com/git-history/release/javadoc/com/google/common/base/Optional.html#of(T)) | 创建指定引用的Optional实例，若引用为null则快速失败 |
| ---------------------------------------- | ------------------------------- |
| [Optional.absent()](http://docs.guava-libraries.googlecode.com/git-history/release/javadoc/com/google/common/base/Optional.html#absent()) | 创建引用缺失的Optional实例               |
| [Optional.fromNullable(T)](http://docs.guava-libraries.googlecode.com/git-history/release/javadoc/com/google/common/base/Optional.html#fromNullable(T)) | 创建指定引用的Optional实例，若引用为null则表示缺失 |

### 用Optional实例查询引用（以下都是非静态方法）：

| [boolean isPresent()](http://docs.guava-libraries.googlecode.com/git-history/release/javadoc/com/google/common/base/Optional.html#isPresent()) | 如果Optional包含非null的引用（引用存在），返回true        |
| ---------------------------------------- | ---------------------------------------- |
| [T get()](http://docs.guava-libraries.googlecode.com/git-history/release/javadoc/com/google/common/base/Optional.html#get()) | 返回Optional所包含的引用，若引用缺失，则抛出java.lang.IllegalStateException |
| [T or(T)](http://docs.guava-libraries.googlecode.com/git-history/release/javadoc/com/google/common/base/Optional.html#or(T)) | 返回Optional所包含的引用，若引用缺失，返回指定的值            |
| [T orNull()](http://docs.guava-libraries.googlecode.com/git-history/release/javadoc/com/google/common/base/Optional.html#orNull()) | 返回Optional所包含的引用，若引用缺失，返回null            |
| [Set asSet()](http://docs.guava-libraries.googlecode.com/git-history/release/javadoc/com/google/common/base/Optional.html#asSet()) | 返回Optional所包含引用的单例不可变集，如果引用存在，返回一个只有单一元素的集合，如果引用缺失，返回一个空集合。 |

### 测试代码

```java
package p;

import com.google.common.base.Optional;

import java.util.Iterator;
import java.util.Set;

public class C0010_Optional {
    public static void main(String[] args) {
        test1();
        test2();
        test3();
        test4();
        test5();
        test6();
    }

    /**
     * of.
     */
    private static void test1() {
        g.println("test1 start ------>");
        Optional<Integer> possible = Optional.of(5); // 创建指定引用的Optional实例，若引用为null则快速失败
//        Optional<Integer> possible = Optional.of(null); // 创建指定引用的Optional实例，若引用为null则快速失败
        printOptionalValue(possible);
        g.println("test1 end   ------<");
    }

    /**
     * absent.
     */
    private static void test2() {
        g.println("test2 start ------>");
        Optional<Integer> possible = Optional.absent(); // 创建引用缺失的Optional实例
        printOptionalValue(possible);
        g.println("test2 end   ------<");
    }

    /**
     * fromNullable.
     */
    private static void test3() {
        g.println("test3 start ------>");
        Integer n1 = null;
        Integer n2 = 4;
        Optional<Integer> possible1 = Optional.fromNullable(n1); // 创建指定引用的Optional实例，若引用为null则表示缺失
        printOptionalValue(possible1);
        Optional<Integer> possible2 = Optional.fromNullable(n2); // 创建指定引用的Optional实例，若引用为null则表示缺失
        printOptionalValue(possible2);
        g.println("test3 end   ------<");
    }

    /**
     * or.
     */
    private static void test4() {
        g.println("test4 start ------>");
        Integer n1 = null;
        Integer n2 = 5;
        Optional<Integer> possible1 = Optional.fromNullable(n1); // 创建指定引用的Optional实例，若引用为null则表示缺失
        Integer v1 = possible1.or(404); // 返回Optional所包含的引用，若引用缺失，返回指定的值
        g.println(v1);
        Optional<Integer> possible2 = Optional.fromNullable(n2); // 创建指定引用的Optional实例，若引用为null则表示缺失
        Integer v2 = possible2.or(404); // 返回Optional所包含的引用，若引用缺失，返回指定的值
        g.println(v2);
        g.println("test4 end   ------<");
    }

    /**
     * orNull.
     */
    private static void test5() {
        g.println("test5 start ------>");
        Integer n1 = null;
        Integer n2 = 5;
        Optional<Integer> possible1 = Optional.fromNullable(n1); // 创建指定引用的Optional实例，若引用为null则表示缺失
        Integer v1 = possible1.orNull(); // 返回Optional所包含的引用，若引用缺失，返回null
        g.println(v1);
        Optional<Integer> possible2 = Optional.fromNullable(n2); // 创建指定引用的Optional实例，若引用为null则表示缺失
        Integer v2 = possible2.orNull(); // 返回Optional所包含的引用，若引用缺失，返回null
        g.println(v2);
        g.println("test5 end   ------<");
    }

    /**
     * absent.
     */
    private static void test6() {
        g.println("test6 start ------>");
        Optional<Integer> possible1 = Optional.absent(); // 创建引用缺失的Optional实例
        final Set<Integer> integers1 = possible1.asSet(); // 返回Optional所包含引用的单例不可变集，如果引用存在，返回一个只有单一元素的集合，如果引用缺失，返回一个空集合。
        g.println(integers1.size());

        Optional<Integer> possible2 = Optional.of(5); // 创建引用缺失的Optional实例
        final Set<Integer> integers2 = possible2.asSet(); // 返回Optional所包含引用的单例不可变集，如果引用存在，返回一个只有单一元素的集合，如果引用缺失，返回一个空集合。
        Iterator<Integer> iterator2 = integers2.iterator();
        g.println(integers2.size());

        g.println("test6 end   ------<");
    }

    private static void printOptionalValue(Optional<Integer> possible) {
        if (possible.isPresent()) { // 如果Optional包含非null的引用（引用存在），返回true
            int v = possible.get(); // 返回Optional所包含的引用，若引用缺失，则抛出java.lang.IllegalStateException
            g.println(v);
        } else {
            g.println("可选值是空的");
        }
    }
}

```



## 1.2 前置条件: 让方法中的条件检查更简单

## 1.3 常见Object方法: 简化Object方法实现，如hashCode()和toString()

## 1.4 排序: Guava强大的”流畅风格比较器”

## 1.5 Throwables：简化了异常和错误的传播与检查



# 2, 集合[Collections]

# 3, 缓存[Caches]

# 4, 函数式风格[Functional idioms]

# 5, 并发[Concurrency]

# 6, 字符串处理[Strings]

# 7, 原生类型[Primitives]

# 8, 区间[Ranges]

# 9, I/O

# 10, 散列[Hash]

# 11, 事件总线[EventBus]

# 12, 数学运算[Math]

# 13, 反射[Reflection]





# 参考文章

- [Google Guava官方教程（中文版）](http://ifeve.com/google-guava/)


