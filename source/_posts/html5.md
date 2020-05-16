title: HTML5疑难杂症
date: 2020-05-16
categories: HTML
tags:
- HTML
- HTML5

---

HTML5的一些疑难杂症及解决方案，不断补充~

<!-- more -->

## 移动端1px像素问题
开发过程中发现，移动端写的边框1px，在有些设备中较粗，有些设备中又好像很正常，主要原因是不同易懂设备的像素密度是不同的。
window对象中有一个devicePixelRadio属性，可以反映css中的设备像素比。

### 解决方案
- 伪类+transform
即去掉原先元素的border，利用:before或after来重做border，并transform的scale缩小为原来的一半，原先的元素相对定位，新作的border绝对定位。
```
li {
    position: relative;
}
li:after {
    position: absolute;
    bottom: 0;
    left: 0;
    content: '';
    width: 100%;
    height: 1px;
    border-top: 1px solid black;
    transform: scaleY(0.5);
}
```

- viewport + rem(ios)
viewport结合rem解决像素比问题
通过设置对应viewport的rem基准值，这种方式就可以像以前一样轻松愉快的写1px了.
在devicePixelRatio = 2 时，输出viewport
```
<meta name="viewport" content="initial-scale=0.5, maximum-scale=0.5, minimum-scale=0.5, user-scalable=no">
```

在devicePixelRatio = 3 时，输出viewport：
```
<meta name="viewport" content="initial-scale=0.3333333333333333, maximum-scale=0.3333333333333333, minimum-scale=0.3333333333333333, user-scalable=no">
```

- 使用box-shadow模拟边框
```
.box-1px {
  box-shadow: inset 0px -1px 1px -1px black;
}
```

## inline-block元素中间的间距问题
display:inline-block，简单来说就是将对象呈现为inline对象，但是对象的内容作为block对象呈现，之后的内联对象会排列在同一行

比如两个input，默认中间会产生一些间距

### 解决方案
- 元素放在一行（最low最粗暴的方案） 
- 父级font-size设置为0，chrome浏览器需要设置来满足兼容性-webkit-text-size-adjust:none;

- margin负值，压缩后会出问题
- letter-sapcing （opera浏览器会有1像素间距）
```
 .box{
      letter-spacing: -5px;
}
.box input{
      letter-spacing: 0;
}
```