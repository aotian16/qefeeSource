title: android如何让layout以及其子视图都无效化
date: 2014-02-05 19:39:15
categories: android
tags: android
---

<!--head-->


遇到一个需求，让layout中以及其子视图都无效化。
嵌套比较多，一个一个设置就麻烦了。然后就在网上找解决方案，还真有人有同样的需求。

**链接**

**[Disabling all child views inside the layout](http://stackoverflow.com/questions/19800422/disabling-all-child-views-inside-the-layout)**

<!--more-->

<!--body-->
主要代码为一个递归方法,稍微改了下，加了个参数

```
private static void setEnabled(ViewGroup layout, boolean flag) {
	layout.setEnabled(flag);
	for (int i = 0; i < layout.getChildCount(); i++) {
		View child = layout.getChildAt(i);
		if (child instanceof ViewGroup) {
			setEnabled((ViewGroup) child, flag);
		} else {
			child.setEnabled(flag);
		}
	}
}
```