title: flexbox布局入门

date: 2016-01-06 11:06:22

tags: flexbox

categories: html

---

<!--head-->



照着参考文章写了几个demo,也算入门了吧.

本文就是罗列下属性, 详细请看参考文章

<!--more-->

<!--body-->

[TOC]

### 参考文章

1. [Flex 布局教程：语法篇](http://www.ruanyifeng.com/blog/2015/07/flex-grammar.html)
2. [Flex 布局教程：实例篇](http://www.ruanyifeng.com/blog/2015/07/flex-examples.html)
3. [A Visual Guide to CSS3 Flexbox Properties](https://scotch.io/tutorials/a-visual-guide-to-css3-flexbox-properties)

### 布局结构

#### 布局结构参考图

![image](https://cask.scotch.io/2015/04/CSS3-Flexbox-Model.jpg)

#### 布局结构名词解释

| name           | detail                 |
| -------------- | ---------------------- |
| flex           | Flexible Box, 意为"弹性布局" |
| flex container | Flex容器, 采用Flex布局的元素    |
| flex item      | Flex项目, Flex容器内的元素     |
| main axis      | 主轴, 轴之1/2              |
| cross axis     | 交叉轴, 轴之2/2, 垂直于主轴      |
| main start     | 主轴的开始位置                |
| main end       | 主轴的结束位置                |
| cross start    | 交叉轴的开始位置               |
| cross end      | 交叉轴的结束位置               |
| main size      | 单个项目占据的主轴空间            |
| cross size     | 单个项目占据的交叉轴空间           |

### Flex容器的属性

| name            | detail                                   |
| --------------- | ---------------------------------------- |
| flex-direction  | 主轴的方向                                    |
| flex-wrap       | 换行                                       |
| flex-flow       | `flex-direction`属性和`flex-wrap`属性的简写形式，默认值为`row nowrap` |
| justify-content | 主轴上的对齐方式                                 |
| align-items     | 交叉轴上如何对齐                                 |
| align-content   | 多根轴线的对齐方式(类似于`justify-content`属性)。如果项目只有一根轴线，该属性不起作用 |

**参考代码**

``` css
        .flex-container {
            flex-direction: row;
            flex-wrap: wrap;
            justify-content: center;
            align-items: center;
            align-content: center;
        }
```

#### flex-direction属性

| name           | detail       | image                                    |
| -------------- | ------------ | ---------------------------------------- |
| row            | 默认值,    从左到右 | ![image](https://cask.scotch.io/2015/04/flexbox-flex-direction-row.jpg) |
| row-reverse    | 从右到左         | ![image](https://cask.scotch.io/2015/04/flexbox-flex-direction-row-reverse.jpg) |
| column         | 从上到下         | ![image](https://cask.scotch.io/2015/04/flexbox-flex-direction-column.jpg) |
| column-reverse | 从下到上         | ![image](https://cask.scotch.io/2015/04/flexbox-flex-direction-column-reverse.jpg) |

#### flex-wrap属性

| name         | detail  | image                                    |
| ------------ | ------- | ---------------------------------------- |
| nowrap       | 默认值,不换行 | ![image](https://cask.scotch.io/2015/04/flexbox-flex-wrap-nowrap.jpg) |
| wrap         | 换行      | ![image](https://cask.scotch.io/2015/04/flexbox-flex-wrap-wrap.jpg) |
| wrap-reverse | 反向换行    | ![image](https://cask.scotch.io/2015/04/flexbox-flex-wrap-wrap-reverse.jpg) |

#### flex-flow属性

`flex-flow`属性是`flex-direction`属性和`flex-wrap`属性的简写形式，默认值为`row nowrap`。

#### justify-content属性

| name          | detail                             | image                                    |
| ------------- | ---------------------------------- | ---------------------------------------- |
| flex-start    | 默认值, 主轴起点对齐                        | ![image](https://cask.scotch.io/2015/04/flexbox-justify-content-flex-start.jpg) |
| flex-end      | 主轴终点对齐                             | ![image](https://cask.scotch.io/2015/04/flexbox-justify-content-flex-end.jpg) |
| center        | 主轴居中对齐                             | ![image](https://cask.scotch.io/2015/04/flexbox-justify-content-center.jpg) |
| space-between | 两端对齐，项目之间的间隔都相等                    | ![image](https://cask.scotch.io/2015/04/flexbox-justify-content-space-between.jpg) |
| space-around  | 每个项目两侧的间隔相等。所以，项目之间的间隔比项目与边框的间隔大一倍 | ![image](https://cask.scotch.io/2015/04/flexbox-justify-content-space-around.jpg) |

#### align-items属性

| name       | detail        | image                                    |
| ---------- | ------------- | ---------------------------------------- |
| stretch    | 默认值, 拉伸       | ![image](https://cask.scotch.io/2015/04/flexbox-align-items-stretch.jpg) |
| flex-start | 交叉轴起点对齐       | ![image](https://cask.scotch.io/2015/04/flexbox-align-items-flex-start.jpg) |
| flex-end   | 交叉轴终点对齐       | ![image](https://cask.scotch.io/2015/04/flexbox-align-items-flex-end.jpg) |
| center     | 交叉轴居中对齐       | ![image](https://cask.scotch.io/2015/04/flexbox-align-items-center.jpg) |
| baseline   | 项目的第一行文字的基线对齐 | ![image](https://cask.scotch.io/2015/04/flexbox-align-items-baseline.jpg) |

#### align-content属性

| name          | detail                              | image                                    |
| ------------- | ----------------------------------- | ---------------------------------------- |
| stretch       | 默认值, 拉伸                             | ![image](https://cask.scotch.io/2015/04/flexbox-align-content-stretch.jpg) |
| flex-start    | 交叉轴起点对齐                             | ![image](https://cask.scotch.io/2015/04/flexbox-align-content-flex-start.jpg) |
| flex-end      | 交叉轴终点对齐                             | ![image](https://cask.scotch.io/2015/04/flexbox-align-content-flex-end.jpg) |
| center        | 交叉轴居中对齐                             | ![image](https://cask.scotch.io/2015/04/flexbox-align-content-center.jpg) |
| space-between | 与交叉轴两端对齐，轴线之间的间隔平均分布                | ![image](https://cask.scotch.io/2015/04/flexbox-align-content-space-between.jpg) |
| space-around  | 每根轴线两侧的间隔都相等。所以，轴线之间的间隔比轴线与边框的间隔大一倍 | ![image](https://cask.scotch.io/2015/04/flexbox-align-content-space-around.jpg) |

### Flex项目的属性

| name        | detail                                   |
| ----------- | ---------------------------------------- |
| order       | 定义项目的排列顺序。                               |
| flex-grow   | `flex-grow`属性定义项目的放大比例，默认为`0`，即如果存在剩余空间，也不放大。 |
| flex-shrink | `flex-shrink`属性定义了项目的缩小比例，默认为1，即如果空间不足，该项目将缩小。 |
| flex-basis  | `flex-basis`属性定义了在分配多余空间之前，项目占据的主轴空间（main size）。 |
| flex        | `flex-grow`, `flex-shrink` 和 `flex-basis`的简写，默认值为`0 1 auto`。 |
| align-self  | 允许单个项目有与其他项目不一样的对齐方式，可覆盖`align-items`属性。默认值为`auto`，表示继承父元素的`align-items`属性，如果没有父元素，则等同于`stretch`。 |

#### order属性

数值越小，排列越靠前，默认为0

![image](https://cask.scotch.io/2015/04/flexbox-order.jpg)

#### flex-grow属性

如果所有项目的`flex-grow`属性都为1，则它们将等分剩余空间（如果有的话）。如果一个项目的`flex-grow`属性为2，其他项目都为1，则前者占据的剩余空间将比其他项多一倍。

![image](https://cask.scotch.io/2015/04/flexbox-flex-grow-1.jpg)

![image](https://cask.scotch.io/2015/04/flexbox-flex-grow-2.jpg)

#### flex-shrink属性

如果所有项目的`flex-shrink`属性都为1，当空间不足时，都将等比例缩小。如果一个项目的`flex-shrink`属性为0，其他项目都为1，则空间不足时，前者不缩小。

负值对该属性无效。

![image](https://cask.scotch.io/2015/04/flexbox-flex-shrink.jpg)

#### flex-basis属性

浏览器根据这个属性，计算主轴是否有多余空间。它的默认值为`auto`，即项目的本来大小。它可以设为跟`width`或`height`属性一样的值（比如350px），则项目将占据固定空间。

![image](https://cask.scotch.io/2015/04/flexbox-flex-basis.jpg)

#### flex属性

后两个属性可选。该属性有两个快捷值：`auto` (`1 1 auto`) 和 none (`0 0 auto`)。

建议优先使用这个属性，而不是单独写三个分离的属性，因为浏览器会推算相关值。

#### align-self属性

![image](https://cask.scotch.io/2015/04/flexbox-align-self.jpg)