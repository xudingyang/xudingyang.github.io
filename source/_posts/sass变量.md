---
title: sass变量
date: 2017-05-04 21:23:09
tag: [前端,sass]
---

# 变量。$符号开头
## 1 普通变量、默认变量
```scss
$width: 12px;
$height: 36px !default;
p {
  width: $width;
  height: $height;
}
```

<!-- more -->

## 2 特殊变量
**sass代码**
```scss
$borderDirection:       top !default;
$baseFontSize:          12px !default;
$baseLineHeight:        1.5 !default;

//应用于class和属性
.border-#{$borderDirection}{
  border-#{$borderDirection}:1px solid #ccc;
}
//应用于复杂的属性值
body{
  font:#{$baseFontSize}/#{$baseLineHeight};
}
```
**生成css**
```css
p { width: 12px; height: 36px; }

.border-top { border-top: 1px solid #ccc; }

body { font: 12px/1.5; }

/*# sourceMappingURL=c_style.css.map */
```

## 3 多值变量
> 多值变量分为list类型和map类型

### 3.1 list类型
list数据可通过空格，逗号或小括号分隔多个值，可用nth($var,$index)取值。

关于list数据操作还有很多其他函数如length($list)，join($list1,$list2,[$separator])，append($list,$value,[$separator])等。
变现形式：
```scss
//一维数据
$px: 5px 10px 20px 30px;
//二维数据，相当于js中的二维数组
$px: 5px 10px, 20px 30px;
$px: (5px 10px) (20px 30px);
```

**运用数组为超链接加样式:**
```scss
$linkColor: #08c #333 !default;
a{
  color:nth($linkColor,1);
  &:hover{
    color:nth($linkColor,2);
  }
}
```
**生成css**
```css
a { color: #08c; }
a:hover { color: #333; }
```
### 3.2 map类型
map数据以key和value成对出现，其中value又可以是list。

格式为：$map: (key1: value1, key2: value2, key3: value3);。

可通过map-get($map,$key)取值。关于map数据还有很多其他函数如map-merge($map1,$map2)，map-keys($map)，map-values($map)等

**使用：**
```scss
$headings: (h1: 2em, h2: 1.5em, h3: 1.2em);
// each是sass里的函数， key,value in $headings
@each $header, $size in $headings {
  #{$header} {
    font-size: $size;
  }
}
```
**编译后的css:**
```css
h1 { font-size: 2em; }

h2 { font-size: 1.5em; }

h3 { font-size: 1.2em; }
```

## 4 局部变量与全局变量
> 上古时代，sass没有作用域的概念，在选择器中声明的变量会覆盖外面全局声明的变量，也就是修改了外边变量的值，也就是它们俩是一个变量。

> v3.4以后增加了全局变量和局部变量的区别。

**我用的代码都是v3.4版本之后的代码**
### 4.1 默认情况
**sass代码：**
```scss 
$outer-width: 200px; // 全局变量
.a {
  $outer-width: 100px;   // 局部变量
  width: $outer-width;   // 局部变量起效
}
.b {
  width: $outer-width;   // 全局变量起效
}
```
**编译成css后：**
```css
.a { width: 100px; }

.b { width: 200px; }
```
## 4.2 牛逼的 !global
!global会强制把属性变为全局变量，管它在哪个作用域
**sass代码：**
```scss
$outer-width: 200px;  // 我被剥夺全局变量的资格了
.a {
  $outer-width: 100px !global;  // 此时，老子才是全局变量
  width: $outer-width;
}
.b {
  width: $outer-width;
}
```
**编译后的css:**
```css
.a { width: 100px; }

.b { width: 100px; }
```
### 4.2.1 遵循代码顺序
!global不破坏代码顺序，即在!global之前对全部变量的引用，仍然是引用的旧值，!global还没有起作用。
```scss
$outer-width: 200px;
.b {
  width: $outer-width;
}
.a {
  $outer-width: 100px !global;
  width: $outer-width;
}
```
```css
.b { width: 200px; }

.a { width: 100px; }
```

# 5 规范
胡乱声明变量的就是傻逼，又不是程序设计语言，搞那么多变量干什么。

**什么时候可以(是可以不是必须)创建变量呢：**
1. 该值至少出现了两次
2. 该值至少会被改变一次
3. 该值所有的变现都与变量有关(非巧合)