---
layout: post
title:  "Python selenium —— XPath and CSS cheat sheet"
date:   2016-10-13 15:45:13
categories: selenium
permalink: /archivers/xpath-css-cheat-sheet
---

## XPath、CSS定位速查表

![xpath css cheat sheet](http://img.blog.csdn.net/20161013145602983)

HTML版如下：


|描述 |Xpath  | CSS Path|
| -- | -- | -- |
|直接子元素 |   //div/a| div > a|
|子元素或后代元素 |  //div//a  |  div a|
|以id定位 | //div[@id=’idValue’]//a |div#idValue a|
|以class定位 |  //div[@class=’classValue’]//a |  div.classValue a|
|同级弟弟元素  | //ul/li[@class=’first’]/following-sibling::li |   ul>li.first + li|
|属性 |  //form/input[@name=’username’] | form input[name=’username’]|
|多个属性 | //input[@name=’continue’ and @type=‘button’]  | input[name=’continue’][type=’button’]|
|第4个子元素 |  //ul[@id=’list’]/li[4] | ul#list li:nth-child(4)|
|第1个子元素 |//ul[@id=’list’]/li[1] | ul#list li:first-child|
|最后1个子元素 | //ul[@id=’list’]/li[last()] |ul#list li:last-child|
|属性包含某字段 |  //div[contains(@title,’Title’)]| div[title*=”Title”]|
|属性以某字段开头 |  //input[starts-with(@name,’user’)] | input[name^=”user”]|
|属性以某字段结尾 |//input[ends-with(@name,’name’)]|    input[name$=”name”]|
|text中包含某字段 | //div[contains(text(), 'text')] | 无法定位 |
|元素有某属性 |  //div[@title]|   div[title]|
|父节点 | //div/.. | 无法定位|
|同级哥哥节点 | //li/preceding-sibling::div[1] | 无法定位 |


更多关于父子、兄弟节点的定位，可见博主之前的博客 **[Python selenium —— 父子、兄弟、相邻节点定位方式详解](https://huilansame.github.io/huilansame.github.io/archivers/father-brother-locate)**

如果它对你有帮助，或者你有什么好的建议，请告诉我。

pdf版本可在此下载 **[xpath css cheat sheet by 灰蓝](http://download.csdn.net/detail/huilan_same/9652810)**

*****

> 更多关于python selenium的文章，请关注我的CSDN专栏：**[Python Selenium自动化测试详解](http://blog.csdn.net/column/details/12694.html)**
