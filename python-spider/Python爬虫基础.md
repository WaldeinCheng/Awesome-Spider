Python爬虫基础

---

### 概念

所谓爬虫，就是模拟用户向客户端发起请求，获取相应，按照特定的规则，去获取所展示的数据。

#### 爬虫的流程

根据URL（要遵循http协议），模拟发送请求，获取响应，解析响应，然后提取url（翻页/详情）,最后保存数据。

### requests模块

用于发送请求，获取响应。

安装命令

```python
pip/pip3 install requests
```

发送get请求

```python
"""
requests的简单get请求
"""
import requests

url = "https://www.baidu.com"
# 向客户端发起请求
response = requests.get(url)
# 打印响应内容,response.text是str类型，以推测的解码方式进行解码
# print(response.text)
# response.content是bytes类型，解码方式需要指定,一般选择这种
print(response.content)
```

response响应对象的其他常用属性和方法

```python
import requests

url = "https://www.baidu.com"
# 向客户端发起请求
response = requests.get(url)
# 响应的url
print(response.url)
# 响应的状态码
print(response.status_code)
# 响应对应的请求头
print(response.request.headers)
# 响应头
print(response.headers)
# 响应对应的请求cookies
# print(response.request.)
# 响应的cookies
print(response.cookies)
# 字典将json字符串类的响应内容转换为Python对象（dict或list)
print(response.json())
```

#### 带请求头的request请求

```python
"""
带请求头的request请求
"""
import requests

url = "https://www.baidu.com"
# 定义请求头，是一个字典格式
headers = {
    'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/91.0.4472.114 Safari/537.36 Edg/91.0.864.54'
}
# 向客户端发起请求
response = requests.get(url, headers=headers)
print(response.content.decode())
```

#### 带参数的request请求

带参数的request请求是实际中最常用的，因为分析数据，一般都需要爬取大量数据，包括各种关键字，翻页等。

```python
"""
带参数的request请求：
"""

import requests
# 第一种，不常用，直接把参数放进url中，直接获取
# url = "https://www.baidu.com/s?wd=python"
# # 定义请求头，是一个字典格式
# headers = {
#     'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/91.0.4472.114 Safari/537.36 Edg/91.0.864.54'
# }
#
# # 向客户端发起请求
# response = requests.get(url, headers=headers)
# print(response.content.decode())
# 第二种方法，使用params参数，构建字典，把参数放字典里，传过去


url = "https://www.baidu.com/s?"
# 定义请求头，是一个字典格式
headers = {
    'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/91.0.4472.114 Safari/537.36 Edg/91.0.864.54'
}

# 构建字典
data = {'wd': 'python'}
# 向客户端发起请求

response = requests.get(url, headers=headers,params=data)
print(response.content.decode())
```

#### 带cookies的request请求

cookies是为了保持登录状态，很多时候爬取数据时，需要保持登录状态。

```python
"""
带cookies的request请求
"""
import requests
url = 'https://github.com/WaldeinCheng'

headers = {
    'user-agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/91.0.4472.114 Safari/537.36 Edg/91.0.864.54',
    'cookie': '_octo=GH1.1.299949888.1624360387; logged_in=no; _gh_sess=UemW6SouvrjNsRVuWMFRtw3Q%2F8DmlypuEdp%2FkaC%2BzrGv52KR4IvpERhykZRCUDZOSaZxF1T7g6aX5%2BhXudBYTcIs42iNMz8DMgWOyYxbKqkAyTLvTvhUJbW24peKNc7%2F5TLSVS7y%2F8I%2FlXMyKXq070bOvSNFZdg23NgvKby69ad6Sq4euTPbuz3buo6JmoppZ%2BhNSP9GTiWPUabWUIA2dDN05xr6Xei9eX3yBH33QPnN82V8xrXARgGCCHsd61klUguPkCsIgJkSbDB3jj5eLw%3D%3D--W26NvT%2FP8cZU823v--1aGrcMdHCg385iQiivzc4g%3D%3D; tz=Asia%2FShanghai'
}

response = requests.get(url, headers=headers)
print(response.content.decode())
```

#### 代理的使用

使用代理可以规避自己的电脑IP地址被检测到封禁的风险，一般使用正向代理比较多。

```python
import requests

url = "https://www.baidu.com"

proxies = {
    # 'http': 'http://60.166.129.218:51061',
    'https': 'https://60.166.129.218:51061'
}

# response = requests.get(url)
response = requests.get(url, proxies=proxies)
print(response.content.decode())
```

#### 使用requests发送post请求

#### 使用session进行模拟登陆

### lxml模块和xpath语法

lxml模块是用来提取html或者xml里的数据的一个模块，lxml模块可以使用xpath语法进行解析数据。主要学习的还是xpath语法的一些规则。

首先在浏览器端，按照一个叫[xpath helper]([XPath Helper - Chrome 网上应用店 (google.com)](https://chrome.google.com/webstore/detail/xpath-helper/hgimnogjllphhhkhlmebbmlgjoejdpjl))的插件，用来学习xpath语法，也是验证取的数据是否正确的工具。

#### 基础节点语法

| 表达式   | 描述                                                        |
| -------- | ----------------------------------------------------------- |
| nodename | 选中该元素                                                  |
| /        | 从跟节点选取，或者是元素之间的过渡                          |
| //       | 从匹配的当前节点开始，不用考虑它们的位置（相对路径）        |
| .        | 选取当前节点                                                |
| ..       | 选取当前节点的父节点                                        |
| @        | 选取属性，比如//title/@href:取得是title节点href标签的属性值 |
| text()   | 选取文本，一般用在最后取两个区间内的值                      |

#### 选取特定节点的语法

| 路径表达式                           | 描述                                                         |
| ------------------------------------ | ------------------------------------------------------------ |
| //title[@lang="eng"]                 | 选择lang属性值为eng的所有title元素                           |
| /bookstore/book[1]                   | 选取属于bookstore子元素的第一个book元素                      |
| /bookstore/book[last()]              | 选取属于bookstore子元素的最后一个book元素                    |
| /bookstore/book[last()-1]            | 选取属于bookstore子元素的倒数第二个book元素                  |
| /bookstore/book[position()>1]        | 选取属于bookstore子元素的book元素，从第二个开始              |
| //book/title[text()='Harry Potter']  | 选取所有book下的title元素，仅仅选择文本为Harry Potter的title元素 |
| /bookstore/book[price()>35.00]/title | 选取bookstore元素的book元素的所有title元素，其中price元素值要大于35 |

#### xpath和lxml实战

爬取百度贴吧帖子的标题和详情的url，并自动翻页。

```python
"""
练习lxml模块如何使用xpath语法:百度贴吧
"""
from lxml import etree
import requests


class BaiDuTiBa(object):
    def __init__(self, name):
        self.url = "https://tieba.baidu.com/f?kw={}".format(name)
        self.headers = {
            'ser-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/91.0.4472.114 Safari/537.36 Edg/91.0.864.54'
        }

    def get_data(self, url):
        response = requests.get(url, headers=self.headers)
        return response.content

    def parse_data(self, data):
        # 创建element对象
        html = etree.HTML(data)
        el_list = html.xpath("//div[@class='t_con cleafix']/div/div/div/a")
        # print(len(el_list))
        data_list = []
        for el in el_list:
            temp = {}
            temp['title'] = el.xpath("./text()")[0]
            temp['url'] = 'https://tieba.baidu.com/' + el.xpath("./@href")[0]
            data_list.append(temp)

        # 获取下一页url
        try:
            next_url = 'https:' + \
                html.xpath("//div[@class='thread_list_bottom clearfix']/div/a[contains(text(),'下一页')]/@href")[0]
        except Exception:
            next_url = None
        return data_list, next_url

    def save_data(self, data_list):
        for data in data_list:
            print(data)

    def run(self):
        # url
        next_url = self.url
        # headers
        while True:
            # 发送请求，获取响应
            data = self.get_data(next_url)
            # 响应中获取数据，翻页判断
            data_list, next_url = self.parse_data(data)
            self.save_data(data_list)
            # 判断nex_url是否为空
            print(next_url)
            # 判断是否终结
            if next_url == None:
                break




if __name__ == '__main__':
    tieba = BaiDuTiBa('李毅')
    tieba.run()
```

