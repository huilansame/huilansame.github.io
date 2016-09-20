---
layout: post
title:  "Python selenium —— 动态id、class怎么定位"
date:   2016-09-20 12:45:13
categories: selenium
permalink: /archivers/how-locate-dynamic-id
---

今天再给大家分享一个定位不到元素的原因——动态id。

没有打开新页面、没有alert、没有frame、加了等待时间，但是还是定位不到元素？很有可能是你要定位的元素的属性是动态的，即每次打开页面，这个元素的id或者class等元素属性是动态生成的。代码执行时，元素的属性已经与之前不同，用之前的属性值去定位自然是定位不到的，会抛出NoSuchElementException。

那么，怎么判断元素属性是否是动态？很简单，一般看到元素属性里有拼接一串数字的，就很有可能是动态的。想要分辨，刷新一下浏览器再看该元素，属性值中的数字串改变了，即是动态属性了。

如下：

```html
<div id="btn-attention_2030295">...</div>
```

怎么定位这类型的元素呢？

## **1. 根据其他属性定位**

如果有其他固定属性，最先考虑的当然是根据元素的其他属性来定位，定位方式那么多，何必在这一棵树上吊死。。

## **2. 根据相对关系定位**

根据其附近的父节点、子节点、兄弟节点定位，关于这方面，博主之前的一篇文章可作为参考：[Python selenium —— 父子、兄弟、相邻节点定位方式详解](https://huilansame.github.io/huilansame.github.io/archivers/father-brother-locate)

## **3. 根据DOM顺序index定位**

这个很简单，找到该元素在主文档或某级父节点中的index，然后根据index可轻松定位，不过这种方式可能不够稳定，如果可以，还是用其他的方法定位更加合适。

## **4. 根据部分元素属性定位**

xpath中提供了三个非常好的方法来为我们定位部分属性值：

```python
driver.find_element_by_xpath("//div[contains(@id, 'btn-attention')]")
driver.find_element_by_xpath("//div[starts-with(@id, 'btn-attention')]")
driver.find_element_by_xpath("//div[ends-with(@id, 'btn-attention')]")  # 这个需要结尾是‘btn-attention’
```

`contains(a, b)` 如果a中含有字符串b，则返回true，否则返回false

`starts-with(a, b)` 如果a是以字符串b开头，返回true，否则返回false

`ends-with(a, b)` 如果a是以字符串b结尾，返回true，否则返回false

这里要多嘴一句，各种浏览器对xpath的支持情况不一样，像IE就差点，所以有时候会出现xpath在一个浏览器能定位到但在另一个浏览器定位不到的问题，不要惊讶。。

附上一个此类型问题：

[Xpath “ends-with” does not work](http://stackoverflow.com/questions/22436789/xpath-ends-with-does-not-work)


****

> 更多关于python selenium的文章，请关注我的CSDN专栏：**[Python Selenium自动化测试详解](http://blog.csdn.net/column/details/12694.html)**
