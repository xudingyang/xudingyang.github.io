---
title: 云服务器部署hexo踩坑
date: 2017-04-27 10:45:17
tags: [hexo,踩坑]
---
### 致谢
首先，感谢这两位作者的文章：
[阿里云VPS搭建自己的的Hexo博客](https://segmentfault.com/a/1190000005723321)
[使用 Git Hook 自动部署 Hexo 到个人 VPS](http://www.swiftyper.com/2016/04/17/deploy-hexo-with-git-hook/)

### 事故记录
<!-- more -->
与上两文重复的部分就不赘述了。
我是看了第一篇文章，通过第一篇文章找到第二篇文章。

看了第一篇文章的过程中，一直未遇到障碍，直到在本地hexo deploy这个环节，服务器git空仓库已经收到了数据，但是本地一直报错：`fatal: Not a git repository: git@xxxxxx.blog.git`。

增加权限，修改路径，改变文件放置路径，啥招数都用了，就是不行。更奇葩的是，远程仓库明明接收到了git push提交的数据，结果本地非要说Not a git repository。

**后来，我把空仓库以及生产目录都放到了root用户下，这下所有疑难杂症全好了。**

我觉得既然所有目录都放在了root目录下，那么git用户就没有存在的意义了，于是把git用户干掉了。结果在本地提交的时候需要填写git用户的密码，而git用户明明被我干掉了，为什么要填git用户密码呢？也许是我用的这个服务器必须用git用户管理git服务器，也或许是它把登录git服务器与登录git用户搞混了(`因为他们的写法都是git@xxx.xxx.xxx.xxx`)。

后来，我再次创建git用户，并在git目录下增加.ssh文件夹和authorized_keys文件。一切又好了。

时间原因，具体原因以后再探究，这里暂时记下这个踩坑经历。

2018年10月30日修改：就是权限的问题。git用户对那几个目录没有权限。
