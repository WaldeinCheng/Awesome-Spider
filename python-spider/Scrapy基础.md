Scrapy框架学习

---

- Scrapy是纯Python实现一个专门为了爬取网站数据，提取结构性数据给编程的框架。
- 所谓框架就是把一些常用的一类轮子封装好，方便我们学习和使用的一套模块。
- Scrapy使用了Twisted异步网络架构来处理网络通讯，可以加快我们的下载速度，不用自己去是心啊异步框架。

### Scrapy架构

![](\img\scrapy架构图.png)

**架构介绍：**

- ENGINE（引擎）：负责SCHEDULER、DOWNLOADER、SPIDERS、ITEM PIPELINES中的通讯，信号和数据传递等。
- SCHEDULER（调度器）：负责接受引擎发过来的Request请求，并按照一定方式进行整理排列，入队，当引擎需要时，会交还给引擎。
- DOWNLOADER（下载器）：负责下载引擎发送的所有Request请求，并将获取到的Responses交还给引擎，有引擎交给Spider处理。
- SPIDER（爬虫）：用我们编写，负责处理所有的Responses，从中分析提取数据，获取Item字段所需的数据，并将需要跟进的URL提交给引擎，再次进入调度器。
- ITEM PIPELINES（管道）：负责Spider中获取到的Item，并进行后期处理（分析，过滤，存储等）。

### 入门程序

根据视频学习，爬取的是传智播客官网的[老师信息列表](www.itcast.cn/channel/teacher.shtml)。

首先创建一个scrapy项目

```python
# 命令创建一个scrapy的项目
scrapy startproject firstSpider
```

创建后的项目目录各个文件作用如下：

- scrapy.cfg：项目的配置文件
- FirstSpider/：项目的Python模块，引用代码都在里面
- FirstSpider/items.py：项目的目标文件，用来接收所需要数据的字段
- FirstSpider/pipelines.py：项目的管道文件
- FirstSpider/settings.py：项目的设置文件
- FirstSpider/spider/：项目的主要代码实现
- FirstSpider/\__init__.py：用来统一导包

创建项目的代码文件，使用以下命令，生成文件会在FirstSpider/spider目录下：

```python
# 创建一个名为itcast的爬虫，并指定爬虫域为"www.itcast.cn"
scrapy genspider itcast "www.itcast.cn"
```

此次要爬取网页的老师的三个信息，`items.py`中代码为：

```python
import scrapy
class FirstspiderItem(scrapy.Item):
    # define the fields for your item here like:
    # 老师姓名
    name = scrapy.Field()
    # 老师职称
    title = scrapy.Field()
    # 简介
    info = scrapy.Field()
    # pass
```

`itcast.py`为主要实现代码：

```python
import scrapy
from FirstSpider.items import FirstspiderItem

class ItcastSpider(scrapy.Spider):
    # 爬虫名，启动爬虫时需要的必需参数
    name = 'itcast'
    # 爬取范围，允许爬虫在这个域名下进行爬取（可选）
    allowed_domains = ['www.itcast.cn']
    # 启动时的url列表，爬虫执行的第一批请求，将从这个列表获取
    start_urls = ['http://www.itcast.cn/channel/teacher.shtml']

    def parse(self, response):
        node_list = response.xpath("//div[@class='li_txt']")
        # items = []
        for node in node_list:
            # 创建item存储获取到的元素
            item = FirstspiderItem()
            # .extract()是xpath对象转换位unicode字符串
            name = node.xpath("./h3/text()").extract()
            title = node.xpath("./h4/text()").extract()
            info = node.xpath("./p/text()").extract()

            item['name'] = name[0]
            item['title'] = title[0]
            item['info'] = info[0]
            # items.append(item)
            # 使用管道处理
        # return items
            yield item

```

`pipelines.py`为管道文件，主要处理 `yield`返回的item数据，并返回给引擎。

```python
from itemadapter import ItemAdapter
import json

class FirstspiderPipeline:
    # 初始化，可选
    def __init__(self):
        self.f = open("itcast_pipeline.json", "wb") # 以二进制方式创建

    # 处理item，必要过程
    def process_item(self, item, spider):
        content = json.dumps(dict(item), indent=1, ensure_ascii=False) # ensure_ascii为false，说明可以输出中文
        self.f.write(content.encode("utf8")) # Python3中写入时默认为二进制写入，所以前面创建时权限也有以二进制形式创建
        return item

    # 关闭文件
    def close_spider(self, spider):
        self.f.close()
```

**注意：**在此次跟着视频实现中，发现本地导包导入不进去，网上查询发现，此类项目根目录必须设置为源路径，详情请看[Scrapy中Items导包问题]([Scrapy中的items导入问题解决！本地包导入不了怎么办？看这里！_OnTheOurWay的博客-CSDN博客_scrapy导入items模块](https://blog.csdn.net/weixin_42726887/article/details/89218939))

###  scrapy.Request的参数详解

```python
scrapy.Request(url, [callback=None, method='GET', headers=None,cookies=None, meta=None, dont_filter=False, body=None])
```

- []里的都是可选参数
- callback：表示当前的url的响应交给哪个函数去处理
- meta：实现数据在不同解析函数中的传递，一般用在拼接不同类型url里的数据
- method：指定GET还是POST请求
- headers：接收一个请求头**字典**，不包括cookie
- cookies：接收一个**字典**，专门放置cookie
- dont_filter：默认为False,会过滤请求的url地址，即请求过的url地址不会继续被请求，设置为True,表示会被重复访问，比如翻页请求。
- body：接收json字符串，为POST请求时携带数据

### scrapy的post请求-模拟登录

scrapy中的模拟登录一般有两种：

- 重写start_request()方法，直接把cookies传过去
- 使用FormRequest()发送post请求，携带cookies

```python
import scrapy


class Git1Spider(scrapy.Spider):
    name = 'git1'
    allowed_domains = ['github.com']
    start_urls = ['https://github.com/WaldeinCheng']

    # 想要携带cookies发送请求，就要重新start_request()方法
    def start_requests(self):
        url = self.start_urls[0]
        temp = '_octo=GH1.1.299949888.1624360387; _device_id=7fccc9e51acd8640c2169c3c32f32eeb; user_session=7zH_7sSL13beX0sK9HsZy6hOcANZNxusN0zclT8j4q4gyC_g; __Host-user_session_same_site=7zH_7sSL13beX0sK9HsZy6hOcANZNxusN0zclT8j4q4gyC_g; logged_in=yes; dotcom_user=WaldeinCheng; has_recent_activity=1; color_mode={"color_mode":"dark","light_theme":{"name":"light","color_mode":"light"},"dark_theme":{"name":"dark","color_mode":"dark"}}; tz=Asia/Shanghai'
        cookies = {data.split('=')[0]: data.split('=')[-1]
                   for data in temp.split(';')}
        yield scrapy.Request(url=url,
                             callback=self.parse,
                             cookies=cookies
                             )

    def parse(self, response):
        print(response.xpath('/html/head/title/text()').extract_first())
```

第二种就是构建post请求体，然后把登录后的状态构建好传过去。

```python
import scrapy


class Git2Spider(scrapy.Spider):
    name = 'git2'
    allowed_domains = ['github.com']
    start_urls = ['https://github.com/login']

    def parse(self, response):
        # 发送登录请求，获取post数据
        token = response.xpath(
            '//input[@name="authenticity_token"]/@value').extract_first()
        post_data = {
            'commit': "Sign in",
            'authenticity_token': token,
            'login': 'cfl1226@163.com',
            'password': 'waldeincheng1226',
            'webauthn-support': 'supported'
        }
        # 针对登录url发送post请求
        yield scrapy.FormRequest(
            url='https://github.com/session',
            callback=self.after_login,
            formdata=post_data
        )

    def after_login(self, response):
        yield scrapy.Request('https://github.com/WaldeinCheng', callback=self.check_login)

    def check_login(self, response):
        print(response.xpath('/html/head/title/text()').extract_first())
```

### 中间件的使用

使用中间件可以在爬虫的请求发起之前或者请求返回之后对数据进行定制化修改，从而开发出适应不同情况的爬虫。

Scrapy中有两种中间件：下载器中间件（Downloader Middleware）和爬虫中间件（Spider Middleware）。

其中下载中间件的官方定义就是：

> *下载器中间件是介于Scrapy的request/response处理的钩子框架，是用于全局修改Scrapy request和response的一个轻量、底层的系统。*

通俗定义就是：用来更换代理IP，更换Cookies，更换User-Agent，自动重试。

爬虫中间件作用于爬虫文件，处理爬虫代码中的异常和测试一些功能。
