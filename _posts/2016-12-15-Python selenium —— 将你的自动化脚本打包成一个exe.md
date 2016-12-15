---
layout: post
title:  "Python selenium —— 将你的自动化脚本打包成一个exe"
date:   2016-12-15 11:45:13
categories: selenium
permalink: /archivers/how-to-make-your-script-exe
---

> 写好了Python selenium脚本，到其他机器上运行，还得要在其他机器上也装一套Python的环境，尤其在你用了一些第三方库的时候，甚至还要顾及操作系统是32位还是64位，是不是很坑，如果能打成一个exe就好了，不论32位还是64位，只要拷过去，安装合适版本的浏览器就行了。今天博主就带你将你的py脚本打包成一个exe

### **1. 环境**

1. 首先准备下环境，一台32位虚拟机（64位的py2exe不允许将程序打包成1个exe文件），装有你脚本执行需要的Python版本以及所有的三方库（确保在这台机器上能够执行你的脚本），安装跟你Python版本对应的py2exe包。

2. 想要打包的py脚本，下面是一个简单的打开chrome并访问[灰蓝博客](http://blog.csdn.net/huilan_same)的例子 `blog.py`：

```python
# -*- coding: utf-8 -*-

from selenium import webdriver
import time

driver = webdriver.Chrome(executable_path='chromedriver.exe')
driver.get('http://blog.csdn.net/huilan_same')
time.sleep(5)
driver.quit()
```

现在我们就把这个小脚本打成一个exe

### **2. setup**

我们创建另一个打包脚本 `setup.py`

```python
# -*- coding: utf-8 -*-

from distutils.core import setup
import py2exe, sys
sys.argv.append('py2exe')

options = {"py2exe": {
    "compressed": 1,  # 压缩
    "optimize": 2,
    "bundle_files": 1,  # 所有文件打包成一个exe文件
}}

setup(
    console=[{'script': "blog.py", "icon_resources": [(1, "robot.ico")]}],
    options=options,
    zipfile=None
)
```

我给程序加了图标，图标文件robot.ico也放在同一目录下，然后我们执行这个脚本，能够看到cmd中一系列的打包操作后创建了两个文件夹build和dist，其中，真正对我们有用的只是 `dist` 中的 **`blog.exe`** 文件（与py脚本同名），还会有一个 `w9xpopen.exe` ，这个是用于win9x系统使用的，没有意义。

![文件夹目录](http://img.blog.csdn.net/20161215142258954?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvaHVpbGFuX3NhbWU=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

注：这里博主不再详细讲解py2exe的用法，关于脚本内容感兴趣可以自行百度相关知识

取出 `blog.exe` ，同目录下放 `chromedriver.exe` （注意版本），双击执行即可

![执行](http://img.blog.csdn.net/20161215143553647?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvaHVpbGFuX3NhbWU=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

这下，不用每次都搞环境了。

### **3. 执行firefox**

上面我故意选了chrome作为例子，而非firefox，是由于firefox比较特殊，像上面一样只是把

    driver = webdriver.Chrome(executable_path='chromedriver.exe')

改成

    driver = webdriver.Firefox()

的话，打包成功但执行时会报如下错误：

![执行错误](http://img.blog.csdn.net/20161215144712777?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvaHVpbGFuX3NhbWU=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

少打了 `webdriver_pref.json` 文件，曾在stackoverflow上看到过一种解决办法是修改打包参数并把resource文件都copy过来，这种打出来之后会是一个臃肿的文件夹，后来博主便不用这种办法了。

那怎么办，看报错是少了一个json文件，那么能否把这个json文件直接写进我们的脚本里呢，所以博主便去看源码，发现主要是`FirefoxProfile`类的 `__init__()`用到了，既然如此，我们便继承过来并重写了此方法即可：

![源码中的引用](http://img.blog.csdn.net/20161215153505143?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvaHVpbGFuX3NhbWU=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

同时，我们还发现另一个引用 `webdriver.xpi` ，此文件在selenium2中相当于firefox的驱动，在启动firefox时会以插件的形式导入。所以`add_extension()`方法也需要重写。

最后，代码如下：

```python
# -*- coding: utf-8 -*-

from selenium import webdriver
import time
# 下面几个包都是FirefoxProfile需要的
import copy
import json
import tempfile
import shutil
import os

# 这里将该json文件里的内容都放到了我们的脚本里，你也可以放到另一个py文件中import进来
WEBDRIVER_PREFERENCES = """
{
  "frozen": {
    "app.update.auto": false,
    "app.update.enabled": false,
    "browser.displayedE10SNotice": 4,
    "browser.download.manager.showWhenStarting": false,
    "browser.EULA.override": true,
    "browser.EULA.3.accepted": true,
    "browser.link.open_external": 2,
    "browser.link.open_newwindow": 2,
    "browser.offline": false,
    "browser.reader.detectedFirstArticle": true,
    "browser.safebrowsing.enabled": false,
    "browser.safebrowsing.malware.enabled": false,
    "browser.search.update": false,
    "browser.selfsupport.url" : "",
    "browser.sessionstore.resume_from_crash": false,
    "browser.shell.checkDefaultBrowser": false,
    "browser.tabs.warnOnClose": false,
    "browser.tabs.warnOnOpen": false,
    "datareporting.healthreport.service.enabled": false,
    "datareporting.healthreport.uploadEnabled": false,
    "datareporting.healthreport.service.firstRun": false,
    "datareporting.healthreport.logging.consoleEnabled": false,
    "datareporting.policy.dataSubmissionEnabled": false,
    "datareporting.policy.dataSubmissionPolicyAccepted": false,
    "devtools.errorconsole.enabled": true,
    "dom.disable_open_during_load": false,
    "extensions.autoDisableScopes": 10,
    "extensions.blocklist.enabled": false,
    "extensions.checkCompatibility.nightly": false,
    "extensions.logging.enabled": true,
    "extensions.update.enabled": false,
    "extensions.update.notifyUser": false,
    "javascript.enabled": true,
    "network.manage-offline-status": false,
    "network.http.phishy-userpass-length": 255,
    "offline-apps.allow_by_default": true,
    "prompts.tab_modal.enabled": false,
    "security.csp.enable": false,
    "security.fileuri.origin_policy": 3,
    "security.fileuri.strict_origin_policy": false,
    "security.warn_entering_secure": false,
    "security.warn_entering_secure.show_once": false,
    "security.warn_entering_weak": false,
    "security.warn_entering_weak.show_once": false,
    "security.warn_leaving_secure": false,
    "security.warn_leaving_secure.show_once": false,
    "security.warn_submit_insecure": false,
    "security.warn_viewing_mixed": false,
    "security.warn_viewing_mixed.show_once": false,
    "signon.rememberSignons": false,
    "toolkit.networkmanager.disable": true,
    "toolkit.telemetry.prompted": 2,
    "toolkit.telemetry.enabled": false,
    "toolkit.telemetry.rejected": true,
    "xpinstall.signatures.required": false,
    "xpinstall.whitelist.required": false
  },
  "mutable": {
    "browser.dom.window.dump.enabled": true,
    "browser.laterrun.enabled": false,
    "browser.newtab.url": "about:blank",
    "browser.newtabpage.enabled": false,
    "browser.startup.page": 0,
    "browser.startup.homepage": "about:blank",
    "browser.usedOnWindows10.introURL": "about:blank",
    "dom.max_chrome_script_run_time": 30,
    "dom.max_script_run_time": 30,
    "dom.report_all_js_exceptions": true,
    "javascript.options.showInConsole": true,
    "network.http.max-connections-per-server": 10,
    "startup.homepage_welcome_url": "about:blank",
    "startup.homepage_welcome_url.additional": "about:blank",
    "webdriver_accept_untrusted_certs": true,
    "webdriver_assume_untrusted_issuer": true
  }
}
"""


class FirefoxProfile(webdriver.FirefoxProfile):
    """ Rewrite FirefoxProfile, to avoid 'No such file ... webdriver.xpi/pref.json' exception"""
    def __init__(self, profile_directory=None):
        if not FirefoxProfile.DEFAULT_PREFERENCES:
            FirefoxProfile.DEFAULT_PREFERENCES = json.loads(WEBDRIVER_PREFERENCES)

        self.default_preferences = copy.deepcopy(
            FirefoxProfile.DEFAULT_PREFERENCES['mutable'])
        self.native_events_enabled = True
        self.profile_dir = profile_directory
        self.tempfolder = None
        if self.profile_dir is None:
            self.profile_dir = self._create_tempfolder()
        else:
            self.tempfolder = tempfile.mkdtemp()
            newprof = os.path.join(self.tempfolder, "webdriver-py-profilecopy")
            shutil.copytree(self.profile_dir, newprof,
                            ignore=shutil.ignore_patterns("parent.lock", "lock", ".parentlock"))
            self.profile_dir = newprof
            self._read_existing_userjs(os.path.join(self.profile_dir, "user.js"))
        self.extensionsDir = os.path.join(self.profile_dir, "extensions")
        self.userPrefs = os.path.join(self.profile_dir, "user.js")

    def update_preferences(self):
        for key, value in self.DEFAULT_PREFERENCES['frozen'].items():
            self.default_preferences[key] = value
        self._write_user_prefs(self.default_preferences)

    def add_extension(self, extension=os.path.abspath('webdriver.xpi')):
        self._install_extension(extension)


# driver = webdriver.Chrome(executable_path='chromedriver.exe')
profile = FirefoxProfile()  # 这里需要实例化并用该profile启动firefox
driver = webdriver.Firefox(firefox_profile=profile)
driver.get('http://blog.csdn.net/huilan_same')
time.sleep(5)
driver.quit()
```

我们再次打包，把 `webdriver.xpi`拷贝出来到程序目录下，再次双击执行`blog.exe`，bingo，可以执行了。

### **4. 扩展**

这时你的脚本就可以在不同的win环境下跑了，如果加上yaml文件作为配置文件、excel作为数据文件、并加上日志，如果有时间还可以加上gui，就可以做一个可配置的网页自动填表程序出来了，自己动手吧。

注意：

> 以上所有示例均为Python2.7 + selenium 2.53版本的，chrome只要跟chromedriver版本匹配即可；如果你用的selenium3.x，对于firefox48及以上的版本，需要用到geckdriver，具体的自己看源码做相应改动吧。

*****

> 更多关于python selenium的文章，请关注我的CSDN专栏：**[Python Selenium自动化测试详解](http://blog.csdn.net/column/details/12694.html)**


最后，博主最近新建了一个QQ群，用于Python Selenium的技术交流，专注于Python+Selenium的技术交流与分享，感兴趣的同学可以加群**（455478219）**，也可直接点击下面的图片加群：

<a target="_blank" href="http://shang.qq.com/wpa/qunwpa?idkey=90801105d449b59723f93b7c51173840de66567e43dcd78fc58f170698636538"><img border="0" src="http://img.blog.csdn.net/20161020130233897" alt="Python Selenium交流群" title="Python Selenium交流群"></a>

欢迎来跟博主讨论Python Selenium有关的问题。

