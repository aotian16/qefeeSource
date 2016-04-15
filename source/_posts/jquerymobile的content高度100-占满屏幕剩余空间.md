title: 'jquerymobile的content高度100%占满屏幕剩余空间'

categories: jquerymobile

tags: jquerymobile

date: 2016-01-26 10:56:45

---

<!--head-->

[TOC]

# Why

为了让content占满空间, 我们需要一段js代码.

**<图片来自参考文章>**

## before

![image](https://jqmtricks.files.wordpress.com/2014/02/before.png?w=338&h=600)

## after

![image](https://jqmtricks.files.wordpress.com/2014/02/after.png?w=338&h=600)

<!--more-->

# How

在你的初始化方法中调用`initHeightSetting`, 传入第一个page的id就可以了.

## code

``` javascript
/// 设置高度的函数
function setHeight(nextPage) {
    var screen = $.mobile.getScreenHeight();
    var header = nextPage.children(".ui-header").hasClass("ui-header-fixed") ? nextPage.children(".ui-header").outerHeight() - 1 : nextPage.children(".ui-header").outerHeight();
    var footer = nextPage.children(".ui-footer").hasClass("ui-footer-fixed") ? nextPage.children(".ui-footer").outerHeight() - 1 : nextPage.children(".ui-footer").outerHeight()
    var contentCurrent = nextPage.children(".ui-content").outerHeight() - nextPage.children(".ui-content").height();
    var content = screen - header - footer - contentCurrent;
    nextPage.children(".ui-content").height(content);
}

/// 初始化高度设置函数.
function initHeightSetting(yourFirstPageId) {
    // 这段代码在初始化的时候执行.设置第一个页面的高度
    setHeight($(yourFirstPageId));

    // 在页面显示前会执行下面代码.设置高度
    $( "body" ).on( "pagecontainershow", function( event, ui ) {
        var nextPage = $(ui.toPage[0]);
        setHeight(nextPage);
    });
}
```

# Refer

[CONTENT “DIV” HEIGHT – FILL PAGE HEIGHT](https://jqmtricks.wordpress.com/2014/02/06/content-div-height-fill-page-height/)

<!--body-->

