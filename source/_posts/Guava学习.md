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

Optional最常用的一些操作被罗列如下：

### 创建Optional实例（以下都是静态方法）：

| 方法                                       | 详细                              |
| ---------------------------------------- | ------------------------------- |
| [Optional.of(T)](http://docs.guava-libraries.googlecode.com/git-history/release/javadoc/com/google/common/base/Optional.html#of(T)) | 创建指定引用的Optional实例，若引用为null则快速失败 |
| [Optional.absent()](http://docs.guava-libraries.googlecode.com/git-history/release/javadoc/com/google/common/base/Optional.html#absent()) | 创建引用缺失的Optional实例               |
| [Optional.fromNullable(T)](http://docs.guava-libraries.googlecode.com/git-history/release/javadoc/com/google/common/base/Optional.html#fromNullable(T)) | 创建指定引用的Optional实例，若引用为null则表示缺失 |


### 用Optional实例查询引用（以下都是非静态方法）：

| 方法                                       | 详细                                       |
| ---------------------------------------- | ---------------------------------------- |
| [boolean isPresent()](http://docs.guava-libraries.googlecode.com/git-history/release/javadoc/com/google/common/base/Optional.html#isPresent()) | 如果Optional包含非null的引用（引用存在），返回true        |
| [T get()](http://docs.guava-libraries.googlecode.com/git-history/release/javadoc/com/google/common/base/Optional.html#get()) | 返回Optional所包含的引用，若引用缺失，则抛出java.lang.IllegalStateException |
| [T or(T)](http://docs.guava-libraries.googlecode.com/git-history/release/javadoc/com/google/common/base/Optional.html#or(T)) | 返回Optional所包含的引用，若引用缺失，返回指定的值            |
| [T orNull()](http://docs.guava-libraries.googlecode.com/git-history/release/javadoc/com/google/common/base/Optional.html#orNull()) | 返回Optional所包含的引用，若引用缺失，返回null            |
| [Set asSet()](http://docs.guava-libraries.googlecode.com/git-history/release/javadoc/com/google/common/base/Optional.html#asSet()) | 返回Optional所包含引用的单例不可变集，如果引用存在，返回一个只有单一元素的集合，如果引用缺失，返回一个空集合。 |

### 测试代码

```java
package p;

import static p.g.*;
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
        println("test1 start ------>");
        Optional<Integer> possible = Optional.of(5); // 创建指定引用的Optional实例，若引用为null则快速失败
//        Optional<Integer> possible = Optional.of(null); // 创建指定引用的Optional实例，若引用为null则快速失败
        printOptionalValue(possible);
        println("test1 end   ------<");
    }

    /**
     * absent.
     */
    private static void test2() {
        println("test2 start ------>");
        Optional<Integer> possible = Optional.absent(); // 创建引用缺失的Optional实例
        printOptionalValue(possible);
        println("test2 end   ------<");
    }

    /**
     * fromNullable.
     */
    private static void test3() {
        println("test3 start ------>");
        Integer n1 = null;
        Integer n2 = 4;
        Optional<Integer> possible1 = Optional.fromNullable(n1); // 创建指定引用的Optional实例，若引用为null则表示缺失
        printOptionalValue(possible1);
        Optional<Integer> possible2 = Optional.fromNullable(n2); // 创建指定引用的Optional实例，若引用为null则表示缺失
        printOptionalValue(possible2);
        println("test3 end   ------<");
    }

    /**
     * or.
     */
    private static void test4() {
        println("test4 start ------>");
        Integer n1 = null;
        Integer n2 = 5;
        Optional<Integer> possible1 = Optional.fromNullable(n1); // 创建指定引用的Optional实例，若引用为null则表示缺失
        Integer v1 = possible1.or(404); // 返回Optional所包含的引用，若引用缺失，返回指定的值
        println(v1);
        Optional<Integer> possible2 = Optional.fromNullable(n2); // 创建指定引用的Optional实例，若引用为null则表示缺失
        Integer v2 = possible2.or(404); // 返回Optional所包含的引用，若引用缺失，返回指定的值
        println(v2);
        println("test4 end   ------<");
    }

    /**
     * orNull.
     */
    private static void test5() {
        println("test5 start ------>");
        Integer n1 = null;
        Integer n2 = 5;
        Optional<Integer> possible1 = Optional.fromNullable(n1); // 创建指定引用的Optional实例，若引用为null则表示缺失
        Integer v1 = possible1.orNull(); // 返回Optional所包含的引用，若引用缺失，返回null
        println(v1);
        Optional<Integer> possible2 = Optional.fromNullable(n2); // 创建指定引用的Optional实例，若引用为null则表示缺失
        Integer v2 = possible2.orNull(); // 返回Optional所包含的引用，若引用缺失，返回null
        println(v2);
        println("test5 end   ------<");
    }

    /**
     * absent.
     */
    private static void test6() {
        println("test6 start ------>");
        Optional<Integer> possible1 = Optional.absent(); // 创建引用缺失的Optional实例
        final Set<Integer> integers1 = possible1.asSet(); // 返回Optional所包含引用的单例不可变集，如果引用存在，返回一个只有单一元素的集合，如果引用缺失，返回一个空集合。
        println(integers1.size());

        Optional<Integer> possible2 = Optional.of(5); // 创建引用缺失的Optional实例
        final Set<Integer> integers2 = possible2.asSet(); // 返回Optional所包含引用的单例不可变集，如果引用存在，返回一个只有单一元素的集合，如果引用缺失，返回一个空集合。
        Iterator<Integer> iterator2 = integers2.iterator();
        println(integers2.size());

        println("test6 end   ------<");
    }

    private static void printOptionalValue(Optional<Integer> possible) {
        if (possible.isPresent()) { // 如果Optional包含非null的引用（引用存在），返回true
            int v = possible.get(); // 返回Optional所包含的引用，若引用缺失，则抛出java.lang.IllegalStateException
            println(v);
        } else {
            println("可选值是空的");
        }
    }
}
```



## 1.2 前置条件: 让方法中的条件检查更简单

| **方法声明（不包括额外参数）******                    | **描述******                               | **检查失败时抛出的异常******        |
| ---------------------------------------- | ---------------------------------------- | ------------------------- |
| [checkArgument(boolean)](http://docs.guava-libraries.googlecode.com/git-history/release/javadoc/com/google/common/base/Preconditions.html#checkArgument(boolean)) | 检查boolean是否为true，用来检查传递给方法的参数。           | IllegalArgumentException  |
| [checkNotNull(T)](http://docs.guava-libraries.googlecode.com/git-history/release/javadoc/com/google/common/base/Preconditions.html#checkNotNull(T)) | 检查value是否为null，该方法直接返回value，因此可以内嵌使用checkNotNull。 | NullPointerException      |
| [checkState(boolean)](http://docs.guava-libraries.googlecode.com/git-history/release/javadoc/com/google/common/base/Preconditions.html#checkState(boolean)) | 用来检查对象的某些状态。                             | IllegalStateException     |
| [checkElementIndex(int index, int size)](http://docs.guava-libraries.googlecode.com/git-history/release/javadoc/com/google/common/base/Preconditions.html#checkElementIndex(int, int)) | 检查index作为索引值对某个列表、字符串或数组是否有效。index>=0 && index<size * | IndexOutOfBoundsException |
| [checkPositionIndex(int index, int size)](http://docs.guava-libraries.googlecode.com/git-history/release/javadoc/com/google/common/base/Preconditions.html#checkPositionIndex(int, int)) | 检查index作为位置值对某个列表、字符串或数组是否有效。index>=0 && index<=size * | IndexOutOfBoundsException |
| [checkPositionIndexes(int start, int end, int size)](http://docs.guava-libraries.googlecode.com/git-history/release/javadoc/com/google/common/base/Preconditions.html#checkPositionIndexes(int, int, int)) | 检查[start, end]表示的位置范围对某个列表、字符串或数组是否有效*   | IndexOutOfBoundsException |

### 测试代码

```java
package p;

import static com.google.common.base.Preconditions.*;
import static p.g.*;

public class C0020_CheckArgument {
    public static void main(String[] args) {
        testCheckArgument(true, false);
        testCheckNotNull("hello", null);
        testCheckState(true, false);

        testCheckElementIndex(-1, 2);
        testCheckElementIndex(1, 2);
        testCheckElementIndex(3, 2);

        testCheckPositionIndex(-1, 2);
        testCheckPositionIndex(1, 2);
        testCheckPositionIndex(3, 2);

        testCheckPositionIndexes(-1, 2, 5);
        testCheckPositionIndexes(1, 2, 5);
        testCheckPositionIndexes(1, 6, 5);
    }

    /**
     * checkPositionIndexes(int start, int end, int size)
     */
    private static void testCheckPositionIndexes(int start, int end, int size) {
        println("testCheckPositionIndexes start ------>");
        try {
            checkPositionIndexes(start, end, size);
        } catch (Exception e) {
            println(e.getMessage());
        }
        println("testCheckPositionIndexes end   ------<");
    }

    /**
     * checkPositionIndex(int index, int size)
     */
    private static void testCheckPositionIndex(int index, int size) {
        println("testCheckPositionIndex start ------>");
        try {
            checkPositionIndex(index, size, "index error");
        } catch (Exception e) {
            println(e.getMessage());
        }
        println("testCheckPositionIndex end   ------<");
    }

    /**
     * checkElementIndex(int index, int size)
     */
    private static void testCheckElementIndex(int index, int size) {
        println("testCheckElementIndex start ------>");
        try {
            checkElementIndex(index, size, "index error");
        } catch (Exception e) {
            println(e.getMessage());
        }
        println("testCheckElementIndex end   ------<");
    }

    /**
     * checkState(boolean)
     */
    private static void testCheckState(boolean v1, boolean v2) {
        println("testCheckState start ------>");
        try {
            checkState(v1, "v1 true(v = %s)", v1);
            checkState(v2, "v2 false(v = %s)", v2);
        } catch (Exception e) {
            println(e.getMessage());
        }
        println("testCheckState end   ------<");
    }

    /**
     * checkNotNull(T)
     */
    private static void testCheckNotNull(String v1, String v2) {
        println("testCheckArgument start ------>");
        try {
            checkNotNull(v1, "v1 not null(v = %s)", v1);
            checkNotNull(v2, "v2 null(v = %s)", v2);
        } catch (Exception e) {
            println(e.getMessage());
        }
        println("testCheckArgument end   ------<");
    }

    /**
     * checkArgument(boolean)
     */
    private static void testCheckArgument(boolean v1, boolean v2) {
        println("testCheckArgument start ------>");
        try {
            checkArgument(v1, "v1 true(f = %s)", v1);
            checkArgument(v2, "v2 false(f = %s)", v2);
        } catch (Exception e) {
            println(e.getMessage());
        }
        println("testCheckArgument end   ------<");
    }
}
```



## 1.3 常见Object方法: 简化Object方法实现，如hashCode()和toString()
// TODO

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
