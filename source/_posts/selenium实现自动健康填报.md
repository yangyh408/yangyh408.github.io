---
title: selenium实现自动健康填报
date: 2020-03-18
tags: [Notes, Python]
---
{% note info %}
## selenium模拟浏览器操作
{% endnote %}

在这部分开头我先贴出了自动执行填写健康信息的脚本，对于selenium较为详细的解析和使用指南放在了这一部分的后半段

### selenium实战

为什么突然想到学习一波selenium的用法呢？
一方面是在爬虫学习中听说过有这么一款功能极强的浏览器操作模块，另一方面正赶上疫情爆发大家每天都要填报个人健康情况，对于大多数同学基本都能保持健康，每日填写信息也都大同小异，那为什么不让电脑每天自动帮我们上报呢？这样也能避免错过当天的填报时间。

<!--more-->

{% codeblock lang:python %}
from selenium import webdriver
import time
from selenium.webdriver.chrome.options import Options

chrome_options = Options()
chrome_options.add_argument("--headless")
driver = webdriver.Chrome(executable_path="/usr/local/bin/chromedriver",options=chrome_options)
url = "http://xgsys.swjtu.edu.cn/SPCP/Web/UserLogin.aspx"
driver.get(url)

value = {
    "StudentId": "这儿写学号",
    "Name": "这儿写名字",
    "StuCard": "这儿写身份证号后六位",
    "codeInput":  driver.find_element_by_id('code-box').text
}
for key in value.keys():
    element = driver.find_element_by_id(key)
    element.send_keys(value[key])
driver.find_element_by_id('Button4').click()

if len(driver.find_elements_by_class_name('layui-layer-content')) == 0:
    driver.find_element_by_id('Checkbox1').click()
    driver.find_element_by_id('Save_Btn').click()
    time.sleep(2)
driver.get_screenshot_as_file("./screenshot.png")
print(driver.find_element_by_class_name('layui-layer-padding').text)
driver.close()
{% endcodeblock %}

### 安装selenium

首先和安装其他python模块一样，使用pip来安装selenium库
{% code %}
pip3 install selenium
{% endcode %}

因为selenium要控制浏览器进行操作，我们还需要下载浏览器的驱动以解析python发送给它的指令，根据浏览器的类型及版本选择不同驱动进行下载
> 四大主流浏览器下载地址：[Chrome][1]、[Safari][2]、[Edge][3]、[Firefox][4]

对于**MacOS**和**Linux**用户 *(以chrome为例)* ：
1. 将下载好的文件放入目录`/usr/local/bin`或`/usr/bin`中
    {% code %}
    sudo cp 当前chromedriver位置 /usr/local/bin
    {% endcode %}
2. 将驱动赋予执行权限
    {% code %}
    sudo chmod +x /usr/local/bin/chromedriver
    {% endcode %}

### selenium常用函数

1. 不弹出浏览器窗口并执行后续操作
    ```python
    from selenium.webdriver.chrome.options import Options
    chrome_options = Options()
    chrome_options.add_argument("--headless")
    driver = webdriver.Chrome(options=chrome_options)
    ```
2. `webdriver.Chrome(<executable_path>?,<options>?)`激活网页驱动器，executable_path用于传入网页驱动程序位置，options用于传入后续不弹出浏览器的个性化设置
3. `<webdriver>.get(url)`通过链接访问网站
4. `<webdriver>.find_element_by_id('key')`通过id检索元素
5. `<webdriver>.find_element_by_class_name('key')`通过class检索元素，返回第一个符合元素
6. `<webdriver>.find_elements_by_class_name('key')`通过class检索元素，返回列表（其余选择元素方法中elements作用也相同）
7. `<webdriver>.find_element_by_tag_name('key')`通过tag检索元素，返回第一个符合元素
8. `<webdriver>.implicitly_wait(10)`最大等待时间
9. `<webdriver>.page_source`返回当前网页HTML内容
10. `<webdriver>.get_screenshot_as_file('file_path')`将当前网页截图并保存
11. `<driver_element>.click()`点击该选中元素
12. `<driver_element>.clear()`清除输入框已有的字符串
13. `<driver_element>.send_keys('input')`在输入框中输入数据
14. `<driver_element>.text` 返回元素文本内容

{% note info %}
## crontab定时执行脚本
{% endnote %}

### crontab常用指令

> 在编辑任务时可能需要使用Vim修改，[这里][5]有关于Vim的教程

+ 编辑当前用户crontab任务 `crontab -e`
+ 查看当前用户所有crontab 列表 `crontab -l`
+ 删除当前用户所有crontab 列表 `crontab -r`

### crontab命令格式

crontab命令由六部分构成: `M H D m d command`

{% note success %}
`M`: 分（0-59） 
`H`: 时（0-23）
`D`: 天（1-31）
`m`: 月（1-12）
`d`: 周（0-6），0为星期日
`command`: 操作指令
{% endnote %}

{% note success %}
`*`: 取值范围内的所有数字
`/`: 代表"每"，如/3表示每3个
`-`: 代表从某个数字到某个数字
`,`: 代表离散的取值(取值的列表)
{% endnote %}

下面我们举一些常见的例子：
1. `* * * * * date>>cron.log` 每分钟执行将当前计算机时间输入cron.log
2. `* */3 * * * date>>cron.log` 每三小时执行
3. `00 12 10-15 * * date>>cron.log` 每月10-15号12:00执行
4. `* * * * 6,0 date>>cron.log` 每周六日执行

### crontab常见问题

+ 解决macos下crontab不执行的问题：
    ```
    # 检查launchctl中有没有cron任务：
    sudo launchctl list | grep cron 
    # 查看启动项配置：
    locate com.vix.cron
    # 创建database（如果提示warning的话）：
    sudo launchctl load -w /System/Library/LaunchDaemons/com.apple.locate.plist
    # 查看 /etc/crontab 是否存在
    ll /etc/crontab
    # 如果没有则一定要创建 /etc/crontab
    sudo touch /etc/crontab
    ```
+ crontab中python的执行路径为`/usr/bin/python3`，可以用指令安装模块：
  ```/usr/bin/pip3 install selenium --user```
+ 使用`which python3`查看终端内使用的python路径
+ 如果在同一任务中执行多条指令，指令间可以用`;`隔开
+ 如果需要查看任务报错信息，需要在输出文件后加`>2&1`

{% note primary %}
## 参考资料
1. [莫烦PYTHON-高级爬虫](https://morvanzhou.github.io/tutorials/data-manipulation/scraping/5-01-selenium/)
2. [白月黑羽教Python-自动化和性能测试](http://www.python3.vip/doc/tutorial/selenium/01/)
3. [crontab 在mac上不执行问题研究](https://segmentfault.com/a/1190000017493725)
4. [Mac下定时执行python脚本&sh脚本](https://blog.csdn.net/py_tester/article/details/78272006)
{% endnote %}

[1]:https://sites.google.com/a/chromium.org/chromedriver/downloads
[2]:https://webkit.org/blog/6900/webdriver-support-in-safari-10/
[3]:https://developer.microsoft.com/en-us/microsoft-edge/tools/webdriver/
[4]:https://github.com/mozilla/geckodriver/releases
[5]:https://www.runoob.com/linux/linux-vim.html