---
layout: post
title:  "Python selenium —— 搞定网页单选框(radio button)、复选框(checkbox)"
date:   2016-09-08 09:50:15
categories: selenium
permalink: /archivers/radio-button-checkbox
---

网页上有时候遇到`checkbox`和`radio button`，一般情况下这两种都是`<input>`标签，我们可以通过点击或者发送空格的方式进行选中

## **1.选择**

试验网页代码checkandradio.html：

```html
<html>
<body>
Checkbox:
<input type="checkbox" value="cv1" name="c1">
<input type="checkbox" value="cv2">
<input type="checkbox" value="cv3" name="c1">
<input type="checkbox" value="cv4">
<p>
Radio:
<input type="radio" value="rv1" name="r1">
<input type="radio" value="rv2" name="r1">
</body>
</html>
```

定位：就是普通的`input`标签，按照正常的定位方式定位就可以，不再赘述。

下面我们用selenium选中其中的checkbox（1、2）和radio1->radio2，上代码：

```python
from selenium import webdriver
from selenium.webdriver.common.keys import Keys
from time import sleep

driver = webdriver.Firefox()
driver.maximize_window()
driver.get('file:///D:/checkboxandradio.html')

# checkbox
driver.find_element_by_xpath('//input[@value="cv1"]').click()  # click
driver.find_element_by_xpath('//input[@value="cv2"]').send_keys(Keys.SPACE)  # send space

# radio
driver.find_element_by_xpath('//input[@value="rv1"]').send_keys(Keys.SPACE)  # send space
sleep(1)
driver.find_element_by_xpath('//input[@value="rv2"]').click()  # click

sleep(1)
driver.quit()
```

从上例可以看出我们对这种`checkbox`和`radio button`，可以通过直接点击或者发送空格的方式达到选中或者反选的目的。

## **2.检查某个框是否被选中**

方法：

> element.is_selected()

示例代码如下：

```python
from selenium import webdriver
from selenium.webdriver.common.keys import Keys
from time import sleep

driver = webdriver.Firefox()
driver.maximize_window()
driver.get('file:///D:/checkboxandradio.html')

# checkbox
driver.find_element_by_xpath('//input[@value="cv1"]').click()  # click
driver.find_element_by_xpath('//input[@value="cv2"]').send_keys(Keys.SPACE)  # send space
if driver.find_element_by_xpath('//input[@value="cv2"]').is_selected():
    print 'selected!'
else:
    print 'not yet!'

# radio
driver.find_element_by_xpath('//input[@value="rv1"]').send_keys(Keys.SPACE)  # send space
sleep(1)
driver.find_element_by_xpath('//input[@value="rv2"]').click()  # click
if driver.find_element_by_xpath('//input[@value="rv1"]').is_selected():
    print 'selected!'
else:
    print 'not yet!'

sleep(1)
driver.quit()
```

结果：

```
selected!
not yet!
```

当然，选中和判断是否选中还有其他的方法，如**模拟鼠标点击**、**用JS点击**、**JS修改标签属性选中**；**用JS、jQuery判断是否选中**、**用标签属性判断是否选中**，不过针对大部分情况，以上方法足够用了。如果以上方法失效，可以考虑直接修改或获取标签属性，或者可能是其他因素如等待时间、页面遮挡等导致无法选中，可进行更多尝试。

****

> 更多关于python selenium的文章，请关注我的CSDN专栏：**[Python Selenium自动化测试详解](http://blog.csdn.net/column/details/12694.html)**

