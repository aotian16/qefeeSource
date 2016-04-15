title: java8新语法学习
categories:  java
tags: java
date: 2016-04-13 21:25:29

---

<!--head-->

From: [java8新语法学习](http://qefee.com/2016/04/13/java8%E6%96%B0%E8%AF%AD%E6%B3%95%E5%AD%A6%E4%B9%A0/)

简单用代码学习下java8新增的语法.
详细强烈推荐阅读参考文章.

<!--more-->



<!--body-->

## 一. 扩展方法 default method

```java
package com.qefee.dev.java;

public class Java8DefaultMethod {
    public static void main(String[] args) {
        MsgPrinter msgPrinter = new MsgPrinter() {
            @Override
            public String getMsg() {
                return "hello world";
            }
        };

        msgPrinter.printMsg();
    }
}

interface MsgPrinter {
    String getMsg();

    /**
     * 扩展方法.
     * 用default来定义, 有实现体.
     * (本来接口是不允许有实现了的方法的)
     */
    public default void printMsg() {
        String str = getMsg();
        System.out.println(str);
    }
}
```

## 二. Lambda表达式

```java
package com.qefee.dev.java;

import java.util.Arrays;
import java.util.Collections;
import java.util.Comparator;
import java.util.List;

/**
 * Created by tj861 on 2016/4/11.
 */
public class Java8Lambda {
    public static void main(String[] args) {
        List<Integer> list1 = Arrays.asList(1, 3, 2, 5, 4);
        List<Integer> list2 = Arrays.asList(1, 3, 2, 5, 4);

        /*
         * 新的Lambda写法, 代码更加简洁. (对比下面的原来的写法)
         */
        Collections.sort(list1, (o1, o2) -> o1 - o2);
        /*
         * 原来的写法.
         */
        Collections.sort(list2, new Comparator<Integer>() {
            @Override
            public int compare(Integer o1, Integer o2) {
                return o2 - o1;
            }
        });

        System.out.println(list1);
        System.out.println(list2);
    }
}

```

## 三. 函数式接口 Functional Interface

```java
package com.qefee.dev.java;

public class Java8FunctionalInterface {
    public static void main(String[] args) {
        /**
         * 使用Lambda简写.
         */
        Java8FunctionalInterface.printSum((m, n) -> m + n, 2, 3);
        Java8FunctionalInterface.printSum((m, n) -> m - n, 2, 3);
        Java8FunctionalInterface.printSum((m, n) -> m * n, 2, 3);
        Java8FunctionalInterface.printSum((m, n) -> m / n, 2, 3);
    }

    /**
     * 方法接受一个函数式接口对象
     * @param f 函数
     * @param m 参数1
     * @param n 参数2
     */
    private static void printSum(TwoVarFunctionalInterface f, int m, int n) {
        System.out.println(f.foo(m,n));
    }
}

/**
 * 二元接口.
 * 用FunctionalInterface标识函数式接口.
 * 函数式接口只能有一个抽象方法.
 */
@FunctionalInterface
interface TwoVarFunctionalInterface {
    int foo(int m, int n);
}
```

## 四. 方法引用Method References

```java
package com.qefee.dev.java;

public class Java8MethodReferences {
    public static void main(String[] args) {

        /**
         * 可以通过函数式接口来引用方法.
         * 引用符号 ::
         *
         * 有几种引用方法.
         * 具体请参考以下链接
         * <a href='http://blog.csdn.net/kimylrong/article/details/47255123'>Java 8之方法引用(Method References)</a>
         * <a href='http://blog.csdn.net/wwwsssaaaddd/article/details/37573517'>java8 - 方法引用(method referrance)</a>
         *
         * 1. 引用静态方法 ContainingClass::staticMethodName
         * 2. 引用特定对象的实例方法 containingObject::instanceMethodName
         * 3. 引用特定类型的任意对象的实例方法  ContainingType::methodName
         * 4. 引用构造函数 ClassName::new
         */
        TwoVarFunctionalInterface1 f = Java8MethodReferences::staticAddMethod;

        System.out.println(f.foo(2,3));

    }

    private static int staticAddMethod(int a, int b) {
        return a + b;
    }
}

/**
 * 二元接口.
 * 用FunctionalInterface标识函数式接口.
 * 函数式接口只能有一个方法.
 */
@FunctionalInterface
interface TwoVarFunctionalInterface1 {
    int foo(int m, int n);
}

```



## 参考文章

- [Java 8简明教程](http://www.importnew.com/10360.html)
- [Java 8之方法引用(Method References)](http://blog.csdn.net/kimylrong/article/details/47255123)
- [java8 - 方法引用(method referrance)](http://blog.csdn.net/wwwsssaaaddd/article/details/37573517)
- [深入理解Java 8 Lambda（语言篇——lambda，方法引用，目标类型和默认方法）](http://www.cnblogs.com/figure9/archive/2014/10/24/4048421.html)
