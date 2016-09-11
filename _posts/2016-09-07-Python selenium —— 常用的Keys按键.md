---
layout: post
title:  "Python selenium —— 常用的Keys按键"
date:   2016-09-07 09:20:15
categories: selenium
permalink: /archivers/useful-keys
---


我们有时会需要使用发送键盘按键的方法来进行一些操作。一些可见的正常的文本可以直接`send_keys()`发送，但是有一些功能键就需要其他的方法。

selenium就为我们提供了一个`Keys`类，其中提供了很多常用的不可见的特殊按键。

摘取常用的如下：

> **BACKSPACE**（或者**BACK_SPACE**） ——退格、删除键
> 
> **TAB** ——有时可用来切换input框的焦点
> 
> **ENTER** ——回车键，有时可用来代替点击提交按钮
> 
> **SHIFT**（或**LEFT_SHIFT**） ——和其他按键同时发送，可发送大写字母或特殊符号
> 
> **CONTROL**（或**LEFT_CONTROL**） ——和其他按键同时发送可实现一些功能如‘CONTROL+A’、‘CONTROL+C’、‘CONTROL+X’、‘CONTROL+V’等等
> 
> **ALT**（或**LEFT_ALT**） ——和其他键一起使用
> 
> **SPACE** ——输入空格，或选中checkbox、radio框
> 
> **PAGE_UP/PAGE_DOWN** ——通过按键可上下翻页
> 
> F1~F12 ——功能键

基本上我们会用到的不多，常用的大概就是`CONTROL`、`SPACE`、`ENTER`。

- `CONTROL`可以和其他键合用，实现复制、剪切等功能，见：[Python selenium —— 模拟鼠标键盘操作(ActionChains)](https://huilansame.github.io/huilansame.github.io/archivers/mouse-and-keyboard-actionchains)

- `SPACE`可用来选中check box或者radio button，见[Python selenium —— 搞定网页单选框(radio button)、复选框(checkbox)](https://huilansame.github.io/huilansame.github.io/archivers/radio-button-checkbox)

- `ENTER`键可用来提交form表单。

其他的，用到时看文档速查即可。


****

> 更多关于python selenium的文章，请关注我的CSDN专栏：**[Python Selenium自动化测试详解](http://blog.csdn.net/column/details/12694.html)**