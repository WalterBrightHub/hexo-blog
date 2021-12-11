---
title: 小程序适配iPad等宽屏设备的方案
date: 2020-05-19 01:02:04
tags:
 - 小程序
---

在小程序中可以使用rpx单位作为尺寸单位，但在iPad上的体验并不好。

<!--more-->
rpx单位的优点：根据750px设计稿构建代码，手机端有一致的体验。对于小屏设备（如iPhone 5或iPad的小分屏），不需要再写代码来适配。
rpx单位的缺点：当设备宽度过宽时，内容被同比放大，产生不良的体验。

## 解决方案

当设备宽度<=480px时，使用rpx单位。
当设备宽度>=480px时，使用px单位,数值从480px下的rpx数值换算得出。

## 示例代码

这里以scss为例

```scss
/* 480/750=0.64*/
@function toPadPx($rpx){
  @return $rpx*0.64px;
}

@mixin pad-devices {
  @media (min-width:480px) {
      @content;
  }
}

.title {
  font-size: 32rpx;

  @include pad-devices {
    font-size: toPadPx(32);
  }
}

```

编译后得到：

```css
.title {
  font-size: 32rpx;
}
@media (min-width: 480px) {
  .title {
    font-size: 20.48px;
  }
}
```
