---
layout: post
title:  "Python selenium —— 父子、兄弟、相邻节点定位方式详解"
date:   2016-09-14 17:00:13
categories: selenium
permalink: /archivers/father-brother-locate
---

今天跟大家分享下selenium中根据父子、兄弟、相邻节点定位的方法，很多人在实际应用中会遇到想定位的节点无法直接定位，需要通过附近节点来相对定位的问题，但从父节点定位子节点容易，从子节点定位父节点、定位一个节点的哥哥节点就一筹莫展了，别急，且看博主一步步讲解。

## **1. 由父节点定位子节点**

最简单的肯定就是由父节点定位子节点了，我们有很多方法可以定位，下面上个例子：

对以下代码：

```html
<html>
<body>
<div id="A">
    <!--父节点定位子节点-->
    <div id="B">
        <div>parent to child</div>
    </div>
</div>
</body>
</html>
```

想要根据 B节点 定位无id的子节点，代码示例如下：

```python
# -*- coding: utf-8 -*-
from selenium import webdriver

driver = webdriver.Firefox()
driver.get('D:\\py\\AutoTestFramework\\src\\others\\test.html')

# 1.串联寻找
print driver.find_element_by_id('B').find_element_by_tag_name('div').text

# 2.xpath父子关系寻找
print driver.find_element_by_xpath("//div[@id='B']/div").text

# 3.css selector父子关系寻找
print driver.find_element_by_css_selector('div#B>div').text

# 4.xpath轴 child
print driver.find_element_by_xpath("//div[@id='B']/child::div").text

driver.quit()
```

结果：

```
parent to child
parent to child
parent to child
parent to child
```

通过我们熟悉的方法都能很随意的寻找到，就不再多言。需要多说一句的是第4种方法 `xpath轴 child`，这个是xpath默认的轴，可以忽略不写，其实质是跟方法2一样的。

## **2. 由子节点定位父节点**

由子节点想要定位到父节点就有点难度了，对以下代码：

```html
<html>
<body>
<div id="A">
    <!--子节点定位父节点-->
    <div>
        <div>child to parent
            <div>
                <div id="C"></div>
            </div>
        </div>
    </div>
</div>
</body>
</html>
```

我们想要由 C节点 定位其两层父节点的div，示例代码如下：

```python
# -*- coding: utf-8 -*-
from selenium import webdriver

driver = webdriver.Firefox()
driver.get('D:\\py\\AutoTestFramework\\src\\others\\test.html')

# 1.xpath: `.`代表当前节点; '..'代表父节点
print driver.find_element_by_xpath("//div[@id='C']/../..").text

# 2.xpath轴 parent
print driver.find_element_by_xpath("//div[@id='C']/parent::*/parent::div").text

driver.quit()
```

结果：

```
child to parent
child to parent
```

