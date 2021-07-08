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

#### 标签的切换

标签也就是浏览器所打开的标签，使用selenium控制浏览器标签的切换

```python
from selenium import webdriver

url = 'https://hf.58.com/'
driver = webdriver.Chrome()

driver.get(url)

temp = driver.find_element_by_xpath('/html/body/div[3]/div[1]/div[1]/div/div[1]/div[1]/span[2]/a')
temp.click()

print(driver.current_url)
print(driver.window_handles) # 列出当前所有的标签句柄

driver.switch_to.window(driver.window_handles[-1]) # 切换标签的语法

print(driver.current_url)
print(driver.window_handles)
```

#### 窗口切换

一般应用在注册登录，验证码领域，大多数的登录和验证码模块都是一个内嵌的Frame框架，需要切换到Frame里才能进行下一步操作

```python
"""
selenium框架的窗口切换
"""
from selenium import webdriver

url = 'https://qzone.qq.com/'
driver = webdriver.Chrome()
driver.get(url)

# 切换到登录的frame里,这里的参数是frame的id
# 如果没有id，可以使用xpath先定位到frame然后再切换
# temp = driver.find_element_by_xpath('//*[@id="login_frame"]')
# driver.switch_to.frame(temp)
driver.switch_to.frame('login_frame')

driver.find_element_by_id('switcher_plogin').click()
driver.find_element_by_id('u').send_keys('usercode')
driver.find_element_by_id('p').send_keys('password')
driver.find_element_by_id('login_button').click()
```

#### 获取cookie

一般用于爬虫需要登录的场景，但是登录模块很难实现，就用selenium框架先模拟登陆获取cookies

```python
"""
获取cookies
"""
from selenium import webdriver

url = 'https://www.baidu.com'
driver = webdriver.Chrome()
driver.get(url)

print(type(driver.get_cookies()))
# cookies列表转成cookies字典
cookies = {data['name']:data['value'] for data in driver.get_cookies()}
print(cookies)
```

#### 启动无界面模式

主要用于没有界面显示，服务器端

### 斗鱼主播

```python
"""
使用selenium框架爬取斗鱼主播信息
"""
from selenium import webdriver
import time


class Douyu(object):
    def __init__(self):
        self.url = 'https://www.douyu.com/directory/all'
        self.driver = webdriver.Chrome()

    def parse_data(self):
        time.sleep(5)
        room_list = self.driver.find_elements_by_xpath(
            '//*[@id="listAll"]/section[2]/div[2]/ul/li/div')
        # print(len(room_list))
        data = []
        # 遍历每一个房间
        for room in room_list:
            temp = {}
            temp['title'] = room.find_element_by_xpath(
                './a/div[2]/div[1]/h3').text
            temp['type'] = room.find_element_by_xpath(
                './a/div[2]/div[1]/span').text
            temp['owner'] = room.find_element_by_xpath(
                './a/div[2]/div[2]/h2/div').text
            temp['num'] = room.find_element_by_xpath(
                './a/div[2]/div[2]/span').text
            data.append(temp)
        return data

    def save_data(self, data):
        for d in data:
            print(d)

    def run(self):
        # url
        # driver
        # get
        self.driver.get(self.url)
        # parse
        while True:
            data_list = self.parse_data()
            # save
            self.save_data(data_list)
            # next_url
            try:
                el_next = self.driver.find_element_by_xpath('//*[@class=" dy-Pagination-next"]')
                self.driver.execute_script('scrollTo(0,10000000)')
                el_next.click()
            except:
                break


if __name__ == '__main__':
    douyu = Douyu()
    douyu.run()
```

