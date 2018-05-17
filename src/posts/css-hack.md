---
title: "浏览器兼容性小结"
date: "2016-03-01"
description: "浏览器兼容性小结"
tags: ["css", "guide"]
---

参考网上常见兼容性问题，解决方法总结，欢迎大家一起讨论最优解决方案。

### 一、CSS HACK的方法

首先需要知道的是：
```
所有浏览器 通用 height: 100px;
IE6 专用 _height: 100px;
IE7 专用 *+height: 100px;
IE6、IE7 共用 *height: 100px;
IE7、FF 共用 height: 100px !important;
```
例如：
```css
#example { height:100px; } /* FF */
* html #example { height:200px; } /* IE6 */
*+html #example { height:300px; } /* IE7 */
```

举几个例子：

**1、IE6 – IE7+FF**
```css
#example {
height:100px; /* FF+IE7 */
_height:200px; /* IE6 */
}
其实这个用上面说的第一种方法也可以
#example {
height:100px !important; /* FF+IE7 */
height:200px; /* IE6 */
}
```
**2、IE6+IE7 – FF**
```css
#example {
height:100px; /* FF */
*height:200px; /* IE6+IE7 */
}
```
**3、IE6+FF – IE7**
```css
#example {
height:100px; /* IE6+FF */
*+height:200px; /* IE7 */
}
```
**4、IE6 IE7 FF 各不相同**
```css
#example {
height:100px; /* FF */
_height:200px; /* IE6 */
*+height:300px; /* IE7 */
}
```
或：
```css
#example {
height:100px; /* FF */
*height:300px; /* IE7 */
_height:200px; /* IE6 */
}
```
需要注意的是，**代码的顺序一定不能颠倒了**，否则就无效了。因为浏览器在解析css的时候，如果重名的话，后写的优先极高于前面的，会用后面的覆盖前面的，就象给变量赋值一个道理，所以我们把通用的样式放前面，越专用的越放后面。


解释一下4的代码：

读代码的时候，第一行`height:100px`; 大家都通用，IE6 IE7 FF 都显示100px
到了第二行`*height:300px`; FF不认识这个属性，IE6 IE7都认，所以FF还显示100px，而IE6 IE7把第一行得到的height属性给覆盖了，都显示300px
到了第三行`_height:200px`;只有IE6认识，所以IE6就又覆盖了在第二行得到的height,最终显示200px
这样，三个浏览器都有自己的height属性了，各自愉快的玩耍.


### 二、使用IE专用的条件注释
```html
<!–其他浏览器 –>
<link rel=”stylesheet” type=”text/css” href=”css.css” />

<!–[if IE 7]>
<!– 适合于IE7 –>
<link rel=”stylesheet” type=”text/css” href=”ie7.css” />
<![endif]–>

<!–[if lte IE 6]>
<!– 适合于IE6及以下 –>
<link rel=”stylesheet” type=”text/css” href=”ie.css” />
<![endif]–>
```
对应不同的浏览器使用不同的css，方便操作浏览器HACK，易于管理，推荐使用。

### IE的if条件Hack
```html
1. <!–[if !IE]><!–> 除IE外都可识别 <!–<![endif]–>
2. <!–[if IE]> 所有的IE可识别 <![endif]–>
3. <!–[if IE 5.0]> 只有IE5.0可以识别 <![endif]–>
4. <!–[if IE 5]> 仅IE5.0与IE5.5可以识别 <![endif]–>
5. <!–[if gt IE 5.0]> IE5.0以及IE5.0以上版本都可以识别 <![endif]–>
6. <!–[if IE 6]> 仅IE6可识别 <![endif]–>
7. <!–[if lt IE 6]> IE6以及IE6以下版本可识别 <![endif]–>
8. <!–[if gte IE 6]> IE6以及IE6以上版本可识别 <![endif]–>
9. <!–[if IE 7]> 仅IE7可识别 <![endif]–>
10. <!–[if lt IE 7]> IE7以及IE7以下版本可识别 <![endif]–>
11. <!–[if gte IE 7]> IE7以及IE7以上版本可识别 <![endif]–>
```

### 浏览器兼容原因归纳

*不定期更新，欢迎大家一起讨论*

1.文字本身的大小在各个浏览器解析不一致。同样是font-size:14px的宋体文字，在不同浏览器下占的空间是不一样的，ie下实际占高16px，下留白3px，FF下实际占高17px，上留白1px，下留白3px，Opera下就更不一样了。

<span style="color:#009944">解决方案</span>：给文字设定 line-height 。确保所有文字都有默认的 line-height 值。这点很重要，在高度上我们不能容忍1px 的差异。

2.FF下容器高度限定，即容器定义了height之后，容器边框的外形就确定了，不会被内容撑大，而IE下是会被内容撑大，高度限定失效。所以不要轻易给容器定义height。

3.还讨论内容撑破容器问题，横向上的。如果float 容器未定义宽度，FF下内容会尽可能撑开容器宽度，ie下则会优先考虑内容折行。故，内容可能撑破的浮动容器需要定义width。

**浏览器对容器的边界解释是大不相同的，容器内容的影响结果各不相同。**


### 常见兼容问题解决方案

*不定期更新，欢迎大家一起讨论*

**1.FF下div设置`padding `后会导致 width 和 height 增加(div的实际宽度=div宽+padding), 但IE不会.**

<span style="color:#009944">解决办法</span>：给div设定IE、FF两个宽度，在IE的宽度前加上IE特有标记” * “号。

**2.页面居中问题.**

`body {text-align: center;}` 在IE下足够了，但FF下失效。

<span style="color:#009944">解决办法</span>：加上`margin-left: auto; margin-rignt: auto; `

**3.有的时候在IE6上看见一些奇怪的间隙，可我们高度明明设好了呀。**

<span style="color:#009944">解决办法</span>：试试在有空隙的DIV上加上`font-size:0px;`

**4.关于手形光标. `cursor: hand`只适用于 IE.**

<span style="color:#009944">解决办法</span>：在任何时候都使用`cursor: pointer`

**5.浮动IE6产生的双倍距离**
```css
#box {
  float:left;
  width:100px;
  margin:0 0 0 100px;
}
```
这种情况之下IE6会产生200px的距离

<span style="color:#009944">解决办法</span>：加上display:inline，使浮动忽略

这里细说一下block,inline两个元素,Block元素的特点是:总是在新行上开始,高度,宽度,行高,边距都可以控制(块元素);Inline元素的特点是:和其他元素在同一行上,…不可控制(内嵌元素);
```css
#box { 
    display:block; //可以为内嵌元素模拟为块元素
    display:inline; //实现同一行排列的的效果
}
```

**6.页面的最小宽度**

min-width是个非常方便的CSS命令，它可以指定元素最小也不能小于某个宽度，这样就能保证排版一直正确。但IE不认得min-这个定义，但实际 上它把正常的width和height当作有min的情况来使。这样问题就大了，如果只用宽度和高度，正常的浏览器里 这两个值就不会变，如果只用min-width和min-height的话，IE下面根本等于没有设置宽度和高度。比如要设置背景图片，这个宽度是比较重 要的。

<span style="color:#009944">解决办法</span>：为了让这一命令在IE上也能用，可以把一个<div> 放到 <body> 标签下，然后为div指定一个类：
然后CSS这样设计：
```css
#container{
  min-width: 600px;
  width:e-xpression(document.body.clientWidth < 600? “600px”: “auto” );
}
```
第一个min-width是正常的；但第2行的width使用了Javascript，这只有IE才认得，这也会让你的HTML文档不太正规。它实际上通过Javascript的判断来实现最小宽度。

**7.ul和form标签的padding与margin**

ul标签在FF中默认是有padding值的,而在IE中只有margin默认有值。form标签在IE中,将会自动margin一些边距,而在FF中margin则是0；

<span style="color:#009944">解决办法</span>：css中首先都使用这样的样式
```css
ul, form {
  margin:0;
  padding:0;
}
```
初始化,有了统一的标准，所有的网页效果也就能统一化了.

**8.DIV浮动IE文本产生3象素的bug**

左边对象浮动，右边采用外补丁的左边距来定位，右边对象内的文本会离左边有3px的间距.
```css
#box{
  float:left;
  width:800px;
}
#left{
  float:left;
  width:50%;
}
#right{
  width:50%;
  float:left;//这句是关键
}
```
```html
<!--HTML代码-->
<div id=box>
  <div id=left></div>
  <div id=right></div>
</div>
```
针对上面这段代码，个人理解：

* 第一、只要right定义了width属性，在FF下绝对就会两行显示
* 第二、两个width都定义为百分比的话，就算都为100%在IE下也会一行显示。
最简单的办法就是在right中加上`float:left`就解决了！

**9.截字省略号**
```css
.demo { 
  width: 100px;   /*定义宽度*/
  text-overflow: ellipsis;  /*把100像素后面的改成点...*/
  overflow: hidden;   /*超出隐藏*/
  white-space: nowrap; /*空格不换行*/ 
  display: block;
}
/*强制不换行*/ div{ white-space:nowrap; } 
/*自动换行*/ div{ word-wrap: break-word; word-break: normal;} 
/*强制英文单词断行*/ div{ word-break:break-all; } 
```
这个是在越出长度后会自行的截掉多出部分的文字，并以省略号结尾。但Firefox并不支持...，会直接隐藏超出宽度的字段。

**10.margin加倍的问题**
  
设置为float的div在ie下设置的margin会加倍。这是一个ie6都存在的bug。

<span style="color:#009944">解决办法</span>: 在这个div里面加上`display:inline`;    

这里细说一下block与inline两个元素：

<p style="color:#009944">block元素的特点是,总是在新行上开始,高度,宽度,行高,边距都可以控制(块元素);</p>

<p style="color:#009944">Inline元素的特点是,和其他元素在同一行上,不可控制(内嵌元素);</p>

```css
#box{
  display:block; /*可以为内嵌元素模拟为块元素*/
  display:inline; /*实现同一行排列的效果*/ 
}
```

**11.img下的留白：**
```html
<div style="background-color: red">
  <img src=”” />
</div>
```
发现图片底部不是紧贴着容器底部的，是img后面的空白字符造成.

<span style="color:#009944">解决办法</span>：给img设定 `display:block`。
