---
title: sass编译
date: 2017-06-04 13:51:08
tags: [前端]
---

#  编译
## 1.1 编译命令
- 单文件转换命令
```
sass style.scss style.css
```
- 单文件监听命令
```
sass --watch style.scss:style.css
```

<!-- more -->

- 文件夹监听命令
```
sass --watch sassFileDirectory:cssFileDirectory
```
- css文件转成sass/scss文件（一般不会用到）
```
sass-convert style.css style.sass   
sass-convert style.css style.scss
```
## 1.2 编译命令配置
```
sass --watch style.scss:style.css --style compact
sass --watch style.scss:style.css --sourcemap
sass --watch style.scss:style.css --style expanded --sourcemap
sass --watch style.scss:style.css --debug-info
```
- --style表示解析后的css是什么格式，有四种取值分别为：nested，expanded，compact，compressed。
- --sourcemap表示开启sourcemap调试。开启sourcemap调试后，会生成一个后缀名为.css.map文件。
- --debug-info表示开启debug信息，升级到3.3.0之后因为sourcemap更高级，这个debug-info就不太用了。

**sass代码：**
### sass里有中文的时候，css里会自动加上@charset "UTF-8"
```sass
/* 有中文注释，css里会自动加上utf-8 */
body {
	width: 100px;
	.a {
		color: red;
	}
}
.top {
	background-color: #f0f;
}
```
**生成的四种css格式：**
#### 1. nested 嵌套输出方式
**.top之前的空行是编译后空出来的，就算sass里没有，css里也会有**
```css
@charset "UTF-8";
/* 有中文注释，css里会自动加上utf-8 */
body {
  width: 100px; }
  body .a {
    color: red; }

.top {
  background-color: #f0f; }

/*# sourceMappingURL=hello.css.map */
```
#### 2. expanded 展开输出方式
**可以看出，大选择器body里的子选择器间并没有空行**
```css
@charset "UTF-8";
/* 有中文注释，css里会自动加上utf-8 */
body {
  width: 100px;
}
body .a {
  color: red;
}

.top {
  background-color: #f0f;
}

/*# sourceMappingURL=hello.css.map */
```
#### 3. compact 紧凑输出方式
```css
@charset "UTF-8";
/* 有中文注释，css里会自动加上utf-8 */
body { width: 100px; }
body .a { color: red; }

.top { background-color: #f0f; }

/*# sourceMappingURL=hello.css.map */
```
#### 4. compressed 压缩输出方式
```css
body{width:100px}body .a{color:red}.top{background-color:#f0f}
/*# sourceMappingURL=hello.css.map */
```

