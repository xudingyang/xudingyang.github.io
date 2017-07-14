---
title: sass混合
date: 2017-05-08 14:33:29
tag: [前端,sass]
---
# 1 混合（混合宏）
用@mixin声明混合，可传参，多个参数以逗号分开，可给参数设置默认值。

声明的@mixin通过@include来调用。

# 2 不带参混合

<!-- more -->

**sass代码：**
```scss
@mixin center-block {
  margin-left:auto;
  margin-right:auto;
}
.demo{
  @include center-block;
}
```
**编译后的css：**
```css
.demo {
  margin-left: auto;
  margin-right: auto;
}
```
# 3 带参混合
## 3.1 一个参数
**sass代码：**
```scss
@mixin opacity($opacity:50) {
  opacity: $opacity / 100;
  filter: alpha(opacity=$opacity);
}
.opacity{
  @include opacity; //参数使用默认值
}
.opacity-80{
  @include opacity(80); //传递参数
}
```
**编译得到的css:**
```css
.opacity {
  opacity: 0.5;
  filter: alpha(opacity=50);
}

.opacity-80 {
  opacity: 0.8;
  filter: alpha(opacity=80);
}
```
## 3.2 多个参数
- 调用时可直接传入值，如@include传入参数的个数小于@mixin定义参数的个数，则按照顺序表示，后面不足的使用默认值，如不足的没有默认值则报错。

- 除此之外还可以选择性的传入参数，使用参数名与值同时传入。
```scss
@mixin horizontal-line($border:1px dashed #ccc, $padding:10px){
    border-bottom:$border;
    padding-top:$padding;
    padding-bottom:$padding;  
}
.imgtext-h li{
    @include horizontal-line(1px solid #ccc);
}
.imgtext-h--product li{
    @include horizontal-line($padding:15px);
}
```
## 3.3 多组参数
> 如果一个参数可以有多组值，如box-shadow、transition等，那么参数则需要在变量后加三个点表示，如$variables...

**sass代码：**
```scss
@mixin box-shadow($shadow...) {
  @if length($shadow) >= 1 {
    -webkit-box-shadow:$shadow;
    box-shadow:$shadow;
  } @else {
    $shadow: 0 0 10px rgba(111,123,123,.1);
    -webkit-box-shadow: $shadow;
    box-shadow: $shadow;
  }
}
.box{
  border:1px solid #ccc;
  @include box-shadow(0 2px 2px rgba(0,0,0,.3),0 3px 3px rgba(0,0,0,.3),0 4px 4px rgba(0,0,0,.3));
}
```
**css代码：**
```css
.box {
  border: 1px solid #ccc;
  -webkit-box-shadow: 0 2px 2px rgba(0, 0, 0, 0.3), 0 3px 3px rgba(0, 0, 0, 0.3), 0 4px 4px rgba(0, 0, 0, 0.3);
  box-shadow: 0 2px 2px rgba(0, 0, 0, 0.3), 0 3px 3px rgba(0, 0, 0, 0.3), 0 4px 4px rgba(0, 0, 0, 0.3);
}
```

# 4 @content
