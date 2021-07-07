Selenium框架

---

### 概要

Selenium框架会大幅降低爬虫的编写难度，但是与此同时会降低爬虫的爬取速度。

### 工作原理

使用selenium框架，根据我们的Python代码，调用不同浏览器的webdriver，然后控制浏览器完成我们的需要的功能。

### 安装

```bash
pip install selenium
```

下载浏览器对应版本的webdriver：这里以google浏览器为例[ChromeDriver](https://chromedriver.chromium.org/downloads)，然后配置环境变量，也就是把`chromedriver`文件所在的路径配置到环境变量中去。

### 第一个程序

```python
"""
selenium:模拟百度搜索Python，第一个程序
"""
from selenium import webdriver
import time

# 1. 创建driver对象
# 这里如果已经配置好环境变量，可以直接调用，如果没有配置，需要使用executable_path参数指定chromedriver的路径
driver = webdriver.Chrome()

# 2. 发送请求
driver.get('https://www.baidu.com')
# 这里暂停一下：为了看看执行的过程
time.sleep(3)

# 在百度中搜索’Python‘
driver.find_element_by_id('kw').send_keys('Python')

# 点击'百度搜索'
driver.find_element_by_id('su').click()

time.sleep(6)

# 退出浏览器，释放资源
driver.quit()
```

### driver中常用属性和方法

```python
"""
driver的属性和方法
"""
from selenium import webdriver

# 创建个浏览器对象
driver = webdriver.Chrome()

# 发送一个请求
url = "http://www.baidu.com"
driver.get(url)

# 打印渲染之后的网页源代码
print(driver.page_source)
# 响应之后的当前url
print(driver.current_url)
# print(driver.back())
# print(driver.forward())
# 关闭当前标签页
print(driver.close())
# 关闭浏览器
print(driver.quit())
# 当前网页快照，常用于截图中验证码识别
print(driver.save_screenshot('baidu.png'))
```

### driver定位元素标签获取标签对象的方法

```python
"""
selenium定位元素的方法
"""

from selenium import webdriver

url = 'http://www.baidu.com'

driver = webdriver.Chrome()

driver.get(url)

# 通过id定位元素
# driver.find_element_by_id('kw').send_keys('Python')
# 通过xpath定位元素
# driver.find_element_by_xpath('//*[@id="kw"]').send_keys('Python')
# 通过标签的name属性值来定位元素
# driver.find_element_by_name('wd').send_keys('Python')
# 通过css选择器来定位元素
# driver.find_element_by_css_selector('#kw').send_keys('Python')
# 通过类名来定位元素
# driver.find_element_by_class_name('s_ipt').send_keys('Python')

# driver.find_element_by_id('su').click()

# 通过链接文本定位元素
driver.find_element_by_partial_link_text('hao').click()
```

注意：

- find_element和find_elements的区别
  - 多了个s的就返回列表，没有s就返回匹配的第一个标签对象
  - find_element匹配不到就报错，find_elements匹配不到就返回一个空列表

### selenium其他使用方法

