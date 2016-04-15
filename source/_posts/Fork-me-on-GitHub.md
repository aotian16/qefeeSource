title: Fork me on GitHub
categories: qefee
tags: qefee
date: 2016-04-15 16:43:31

---

<!--head-->

在首页上添加了Fork me on GitHub图标.

貌似更加高大上了.O(∩_∩)O~

### 修改文件:

- themes\landscape\layout\layout.ejs

### 修改内容:

`body`末尾添加以下内容, 注意自己修改**github地址**以及**图标位置**.

```html
  <a href="https://github.com/aotian16">
  <img style="position: absolute; top: 300px; right: 0; border: 0; z-index: 100;" src="https://camo.githubusercontent.com/365986a132ccd6a44c23a9169022c0b5c890c387/68747470733a2f2f73332e616d617a6f6e6177732e636f6d2f6769746875622f726962626f6e732f666f726b6d655f72696768745f7265645f6161303030302e706e67" alt="Fork me on GitHub" data-canonical-src="https://s3.amazonaws.com/github/ribbons/forkme_right_red_aa0000.png">
  </a>
```

### 参考文章

- [Hexo博客主题从Jacman切换到NexT.Mist](http://codepub.cn/2016/03/20/Hexo-blog-theme-switching-from-Jacman-to-NexT-Mist/)



<!--more-->



<!--body-->
