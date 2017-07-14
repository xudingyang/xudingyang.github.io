---
title: sass继承
date: 2017-05-09 20:33:29
tag: [前端,sass]
---

# 1 啥是继承
父子元素嵌套是天然的继承。

但是非父子元素之间怎么继承呢？

- 选择器继承可以让选择器继承另一个选择器的所有样式，并联合(并集)声明。
- 使用选择器的继承，要使用关键词@extend，后面紧跟需要继承的选择器。
- 与混合的区别，见本文第 4 部分 

<!-- more -->

# 2 继承选择器
## 2.1 继承类选择器
**sass代码：**
```scss
.btn {
  border: 1px solid #ff0000;
  padding: 0 12px;
  color: #00b2ff;
}
.btn-first {
  background-color: #fff;
  color: blue;
  @extend .btn
}
.btn-second {
  background-color: #f0f0f0;
  content: "老子继承于.btn-first";
  @extend .btn-first
}
```
**css代码：**
```css
@charset "UTF-8";
.btn, .btn-first, .btn-second {
  border: 1px solid #ff0000;
  padding: 0 12px;
  color: #00b2ff;
}

.btn-first, .btn-second {
  background-color: #fff;
  color: blue;
}

.btn-second {
  background-color: #f0f0f0;
  content: "老子继承于.btn-first";
}
```
## 2.2 继承任意选择器
**sass代码：**
```scss
.hoverlink {
  @extend a:hover;
  @extend h1;
}
.comment a.user:hover {
  font-weight: bold;
}
h1 {
  color: white;
}
```
**css代码：**
```css
.comment a.user:hover, .comment .user.hoverlink {
  font-weight: bold;
}

h1, .hoverlink {
  color: white;
}
```
# 3 占位选择器%
如果不调用则不会有任何多余的css文件，避免了以前在一些基础的文件中预定义了很多基础的样式，然后实际应用中不管是否使用了@extend去继承相应的样式，都会解析出来所有的样式。

**sass代码：**
```scss
%mt5 {
  margin-top: 5px;
}
%pt5 {
  padding-top: 5px;
}
.box-1 {
  @extend %mt5;
  @extend %pt5;
}
.box-2 {
  @extend %mt5;
  .inner {
    @extend %pt5;
  }
}
```
**css代码：**
```css
.box-1, .box-2 {
  margin-top: 5px;
}

.box-1, .box-2 .inner {
  padding-top: 5px;
}
```

# 4 继承与混合
## 4.1 混合
> A 混到 B

B会把A的所有代码复制到B中，这样A和B中有大量重复的代码。**不会把相同的代码合并**

## 4.2 继承
> B 继承 A

B仍然会获得A的全部样式，但是他们的代码会以并集形式存在，没有重复代码。
## 4.3 到底用啥
- 如果涉及到变量，那就混合
- 如果不传参，你想咋用都行。我喜欢用继承。