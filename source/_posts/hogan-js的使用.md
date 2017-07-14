---
title: hogan.js的使用
date: 2017-07-12 09:15:33
tags: [前端, js]
---
# 1 往事
我在读大学的时候，有一次需要在JSP页面里塞入二十几条以上的`<li><li/>`标签，我记得当时是用for循环搞定的，短短一个for代码块里，Java代码、html代码、JSP的`<%%>`<!-- more -->标记轮流出现，代码相当难看，要多丑有多丑。

后来，写网页的时候展示商品列表，又是十几条重复的`<li></li>`标签，在html里面总不能写for循环了吧，于是一条一条的把这些`<li></li>`标签给撸了出来，那感觉，简直了。

实在受不了这种写法之后，我开始寻找简写的方法。终于找到了“js模板引擎”这个东东。终于天晴啦！

## 2 hogan.js
js模板引擎有很多，我用过artTemplate、hogan.js，两者中我最喜欢的还是hogan.js，因为它简单的令人发指。

### 2.1 hogan.js的安装和使用
**通过npm安装hogan.js**
> npm install hogan.js

我之所以一直称hogan.js，而不是hogan，是因为在npm中还有一个叫hogan的组件，这是热心网友给hogan.js进行的包装，可是hogan这玩意有时候不能正常用，很多人通过`npm install hogan`命令安装后各种花式报错。解决办法就是`npm install hogan.js`，还是原装的好。
### 2.2 使用
所谓模板引擎，你给它数据，它返回给你想要的东西。我们这里就是，我们准备好“html模子”、“要渲染的数据”，把这些交给引擎，然后引擎把数据填进模板，返回给我们需要的html。
#### 2.2.1 先看一个小demo:
```javascript
var hogan = require('hogan.js');
// 准备要被渲染模板
var template = '<div>姓名:{{name}}</div>';
// 准备所需的数据
var data = {
    name : '张三'
};
// 编译
var compiledTemplate = hogan.compile(template);
// 渲染
var result = compiledTemplate.render(data);

// result结果为 <div>姓名:张三</div>
```
#### 2.2.2 语法详解
##### (1) {% raw %}{{name}}{% endraw %}读取变量
在上边的demo种，把data赋给模板后，hogan.js通过{% raw %}{{name}}{% endraw %}拿到data对象里的name。
##### (2) {% raw %}{{{name}}}{% endraw %}原样读取变量
也是读取变量，只是如果花括号里面是html片段的话，不对这段html编码，直接原样输出
##### (3) {% raw %}{{#list}}{{/list}}{% endraw %}循环、布尔值为真
```javascript
var data = {
    isActive: 'true',
    navList: [
	    {name: 'user-center', desc: '个人中心'},
	    {name: 'order-list', desc: '我的订单'},
	    {name: 'user-pass-update', desc: '修改密码'},
	    {name: 'about', desc: '关于DYMall'}
	]
}
// 使用循环
{{#navList}}
<li class="nav-item">
    <span>{{desc}}</span>
</li>
{{/navList}}
// 使用布尔判断
{{#isActive}}
    <span>isActive为true我就显示<span>
{{/isActive}}
```
##### (4) {% raw %}{{^list}}{{/list}}{% endraw %}循环为空、布尔值为假
```javascript
var data = {
    isActive: 'false',
    navList: [
        // 没错，我就是空数组
	]
}
// 使用循环
{{^navList}}
    <span>我靠，这竟然是个空数组</span>
{{/navList}}
// 使用布尔判断
{{^isActive}}
    <span>isActive为false我才敢出来<span>
{{/isActive}}
```

##### (5) {% raw %}{{.}}{% endraw %}当前被遍历的变量
上边数组中存放的是键值对，可以通过{% raw %}{{key}}{% endraw %}来取值。如果数组中存放的是单值呢，这不能通过{% raw %}{{key}}{% endraw %}取值了，总得想个办法解决吧。
解决方式就是用{% raw %}{{.}}{% endraw %}

```javascript
var data = {
    list: ['张三', '李四', '刘能']
}
{{#list}}
<span>{{.}}-</span>
{{/list}}
// 结果： 张三-李四-刘能-
```
##### (6) {% raw %}{{!}}{% endraw %}注释
这就是注释

#### 2.2.3 总结
不知不觉，hogan.js的语法就写完了，实在是简单的令人发指，但是已经足够使用。因为模板引擎所需要做的无非就是：变量替换、循环。如此，hogan.js是再简单不过了。

#### 2.2.4 例子
用hogan.js实现下图:
![](/img/hoganjs_demo1.png)

```javascript
var data = { 
    navList: [
      {name: 'user-center', desc: '个人中心', href: '#', isActive: false},
      {name: 'order-list', desc: '我的订单', href: '#', isActive: false},
      {name: 'user-pass-update', desc: '修改密码', href: '#', isActive: false},
      {name: 'about', desc: '关于DYMall', href: '#', isActive: true},
    ]
}
```
```html
{{#navList}}
{{#isActive}}
<li class="nav-item active">
  {{/isActive}}
  {{^isActive}}
<li class="nav-item">
  {{/isActive}}
  <a class="link" href="{{href}}">{{desc}}</a>
</li>
{{/navList}}
```
