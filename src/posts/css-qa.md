---
title: "CSS实际运用常见问题及解决方法"
date: "2016-03-01"
description: "CSS实际运用常见问题及解决方法"
tags: ["css"]
---

在前端开发中，有很多小问题需要规避，这里整理一些多年开发中常见的小问题，不定期更新，也欢迎大家讨论添加更多小方法方便大家开发！

<span style="color: #004986">这里很多需要重视的是代码规范，有时间会详细写一篇讲讲代码规范。</span>

**1 margin-top 和 margin-bottom 边距互吞现象**

如果上下两个div，上面的设置`margin-bottom：100px`，下面的设置`margin-top:200px`,会导致两个div的间距还是200px，间距有100px被相互吞食掉了。

<span style="color:#009944">解决办法</span>：只使用margin-bottom或者margin-top来设置两个div的间距，避免不必要的兼容问题。

