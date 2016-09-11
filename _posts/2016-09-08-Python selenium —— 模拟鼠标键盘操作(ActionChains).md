---
layout: post
title:  "Python selenium —— 模拟鼠标键盘操作(ActionChains)"
date:   2016-09-08 13:50:15
categories: selenium
permalink: /archivers/mouse-and-keyboard-actionchains
---


用selenium做自动化，有时候会遇到需要模拟鼠标操作才能进行的情况，比如单击、双击、点击鼠标右键、拖拽等等。而selenium给我们提供了一个类来处理这类事件 —— **`ActionChains`**

> selenium.webdriver.common.action_chains.**ActionChains**(*driver*)

这个类基本能够满足我们所有对鼠标操作的需求。

## **1.`ActionChains`基本用法**

首先需要了解`ActionChains`的执行原理，当你调用`ActionChains`的方法时，不会立即执行，而是会将所有的操作按顺序存放在一个队列里，当你调用`perform()`方法时，队列中的时间会依次执行。

这种情况下我们可以有两种调用方法：

- 链式写法

```python
menu = driver.find_element_by_css_selector(".nav")
hidden_submenu = driver.find_element_by_css_selector(".nav #submenu1")

ActionChains(driver).move_to_element(menu).click(hidden_submenu).perform()
```

- 分步写法

```python
menu = driver.find_element_by_css_selector(".nav")
hidden_submenu = driver.find_element_by_css_selector(".nav #submenu1")

actions = ActionChains(driver)
actions.move_to_element(menu)
actions.click(hidden_submenu)
actions.perform()
```

两种写法本质是一样的，`ActionChains`都会按照顺序执行所有的操作。

## **2.`ActionChains`方法列表**

> **click**(*on\_element=None*)  ——单击鼠标左键
> 
> **click\_and\_hold**(*on\_element=None*)  ——点击鼠标左键，不松开
> 
> **context\_click**(*on\_element=None*)  ——点击鼠标右键
> 
> **double\_click**(*on\_element=None*)  ——双击鼠标左键
> 
> **drag\_and\_drop**(*source, target*)  ——拖拽到某个元素然后松开
> 
> **drag\_and\_drop\_by\_offset**(*source, xoffset, yoffset*)  ——拖拽到某个坐标然后松开
> 
> **key\_down**(*value, element=None*)  ——按下某个键盘上的键
> 
> **key\_up**(*value, element=None*)  ——松开某个键
> 
> **move\_by\_offset**(*xoffset, yoffset*)  ——鼠标从当前位置移动到某个坐标
> 
> **move\_to\_element**(*to_element*)  ——鼠标移动到某个元素
> 
> **move_to_element_with_offset**(*to_element, xoffset, yoffset*)  ——移动到距某个元素（左上角坐标）多少距离的位置
> 
> **perform**()  ——执行链中的所有动作
> 
> **release**(*on_element=None*) ——在某个元素位置松开鼠标左键
> 
> **send_keys**(*\*keys_to_send*)  ——发送某个键到当前焦点的元素
> 
> **send_keys_to_element**(*element, \*keys_to_send*)  ——发送某个键到指定元素

接下来用示例来详细说明和演示每一个方法的用法：

## **3.代码示例**

**1. 点击操作**

> 示例网址[http://sahitest.com/demo/clicks.htm](http://sahitest.com/demo/clicks.htm)

代码：

```python
# -*- coding: utf-8 -*-

from selenium import webdriver
from selenium.webdriver.common.action_chains import ActionChains
from time import sleep


driver = webdriver.Firefox()
driver.implicitly_wait(10)
driver.maximize_window()
driver.get('http://sahitest.com/demo/clicks.htm')

click_btn = driver.find_element_by_xpath('//input[@value="click me"]')  # 单击按钮
doubleclick_btn = driver.find_element_by_xpath('//input[@value="dbl click me"]')  # 双击按钮
rightclick_btn = driver.find_element_by_xpath('//input[@value="right click me"]')  # 右键单击按钮


ActionChains(driver).click(click_btn).double_click(doubleclick_btn).context_click(rightclick_btn).perform()  # 链式用法

print driver.find_element_by_name('t2').get_attribute('value')

sleep(2)
driver.quit()
```

结果：

```
[CLICK][DOUBLE_CLICK][RIGHT_CLICK]
```

**2.鼠标移动**

> 示例网址[http://sahitest.com/demo/mouseover.htm](http://sahitest.com/demo/mouseover.htm)

示例代码：

```python
# -*- coding: utf-8 -*-

from selenium import webdriver
from selenium.webdriver.common.action_chains import ActionChains
from time import sleep

driver = webdriver.Firefox()
driver.implicitly_wait(10)
driver.maximize_window()
driver.get('http://sahitest.com/demo/mouseover.htm')

write = driver.find_element_by_xpath('//input[@value="Write on hover"]')  # 鼠标移动到此元素，在下面的input框中会显示“Mouse moved”
blank = driver.find_element_by_xpath('//input[@value="Blank on hover"]')  # 鼠标移动到此元素，会清空下面input框中的内容

result = driver.find_element_by_name('t1')

action = ActionChains(driver)
action.move_to_element(write).perform()  # 移动到write，显示“Mouse moved”
print result.get_attribute('value')

# action.move_to_element(blank).perform()
action.move_by_offset(10, 50).perform()  # 移动到距离当前位置(10,50)的点，与上句效果相同，移动到blank上，清空
print result.get_attribute('value')

action.move_to_element_with_offset(blank, 10, -40).perform()  # 移动到距离blank元素(10,-40)的点，可移动到write上
print result.get_attribute('value')

sleep(2)
driver.quit()
```

结果

```
Mouse moved

Mouse moved
```

一般很少用位置关系来移动鼠标，如果需要，可参考下面的链接来测量元素位置

> [http://jingyan.baidu.com/article/eb9f7b6d87e2ae869264e847.html](http://jingyan.baidu.com/article/eb9f7b6d87e2ae869264e847.html)

**3.拖拽**

> 示例网址[http://sahitest.com/demo/dragDropMooTools.htm](http://sahitest.com/demo/dragDropMooTools.htm)

代码：

```python
# -*- coding: utf-8 -*-

from selenium import webdriver
from selenium.webdriver.common.action_chains import ActionChains
from time import sleep

driver = webdriver.Firefox()
driver.implicitly_wait(10)
driver.maximize_window()
driver.get('http://sahitest.com/demo/dragDropMooTools.htm')

dragger = driver.find_element_by_id('dragger')  # 被拖拽元素
item1 = driver.find_element_by_xpath('//div[text()="Item 1"]')  # 目标元素1
item2 = driver.find_element_by_xpath('//div[text()="Item 2"]')  # 目标2
item3 = driver.find_element_by_xpath('//div[text()="Item 3"]')  # 目标3
item4 = driver.find_element_by_xpath('//div[text()="Item 4"]')  # 目标4

action = ActionChains(driver)
action.drag_and_drop(dragger, item1).perform()  # 1.移动dragger到目标1
sleep(2)
action.click_and_hold(dragger).release(item2).perform()  # 2.效果与上句相同，也能起到移动效果
sleep(2)
action.click_and_hold(dragger).move_to_element(item3).release().perform()  # 3.效果与上两句相同，也能起到移动的效果
sleep(2)
# action.drag_and_drop_by_offset(dragger, 400, 150).perform()  # 4.移动到指定坐标
action.click_and_hold(dragger).move_by_offset(400, 150).release().perform()  # 5.与上一句相同，移动到指定坐标
sleep(2)
driver.quit()
```

结果：

```
dropped dropped dropped dropped
```

一般用坐标定位很少，用上例中的方法1足够了，如果看源码，会发现方法2其实就是方法1中的`drag_and_drop()`的实现。**注意：拖拽使用时注意加等待时间，有时会因为速度太快而失败。**

**4.按键**

模拟按键有多种方法，能用`win32api`来实现，能用`SendKeys`来实现，也可以用selenium的WebElement对象的`send_keys()`方法来实现，这里`ActionChains`类也提供了几个模拟按键的方法。

> 示例网址[http://sahitest.com/demo/keypress.htm](http://sahitest.com/demo/keypress.htm)

代码1：

```python
# -*- coding: utf-8 -*-

from selenium import webdriver
from selenium.webdriver.common.action_chains import ActionChains
from time import sleep

driver = webdriver.Firefox()
driver.implicitly_wait(10)
driver.maximize_window()
driver.get('http://sahitest.com/demo/keypress.htm')

key_up_radio = driver.find_element_by_id('r1')  # 监测按键升起
key_down_radio = driver.find_element_by_id('r2')  # 监测按键按下
key_press_radio = driver.find_element_by_id('r3')  # 监测按键按下升起

enter = driver.find_elements_by_xpath('//form[@name="f1"]/input')[1]  # 输入框
result = driver.find_elements_by_xpath('//form[@name="f1"]/input')[0]  # 监测结果

# 监测key_down
key_down_radio.click()
ActionChains(driver).key_down(Keys.CONTROL, enter).key_up(Keys.CONTROL).perform()
print result.get_attribute('value')

# 监测key_up
key_up_radio.click()
enter.click()
ActionChains(driver).key_down(Keys.SHIFT).key_up(Keys.SHIFT).perform()
print result.get_attribute('value')

# 监测key_press
key_press_radio.click()
enter.click()
ActionChains(driver).send_keys('a').perform()
print result.get_attribute('value')
driver.quit()
```

结果：

```
key downed charCode=[0] keyCode=[17] CTRL
key upped charCode=[0] keyCode=[16] NONE
key pressed charCode=[97] keyCode=[0] NONE
```

示例2：

> 示例网址[http://sahitest.com/demo/label.htm](http://sahitest.com/demo/label.htm)

代码：

```python
# -*- coding: utf-8 -*-

from selenium import webdriver
from selenium.webdriver.common.action_chains import ActionChains
from selenium.webdriver.common.keys import Keys
from time import sleep

driver = webdriver.Firefox()
driver.implicitly_wait(10)
driver.maximize_window()

driver.get('http://sahitest.com/demo/label.htm')

input1 = driver.find_elements_by_tag_name('input')[3]
input2 = driver.find_elements_by_tag_name('input')[4]

action = ActionChains(driver)
input1.click()
action.send_keys('Test Keys').perform()
action.key_down(Keys.CONTROL).send_keys('a').key_up(Keys.CONTROL).perform()  # ctrl+a
action.key_down(Keys.CONTROL).send_keys('c').key_up(Keys.CONTROL).perform()  # ctrl+c

action.key_down(Keys.CONTROL, input2).send_keys('v').key_up(Keys.CONTROL).perform()  # ctrl+v

print input1.get_attribute('value')
print input2.get_attribute('value')

driver.quit()
```

结果：

```
Test Keys
Test Keys
```

复制粘贴用`WebElement< input >.send_keys()`也能实现，大家可以试一下，也可以用更底层的方法，同时也是`os`弹框的处理办法之一的`win32api`，有兴趣也可以试试`SendKeys`、[`keybd_event`](http://blog.csdn.net/yizhou2010/article/details/6178115)

****

> 更多关于python selenium的文章，请关注我的CSDN专栏：**[Python Selenium自动化测试详解](http://blog.csdn.net/column/details/12694.html)**

