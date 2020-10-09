常见HTTP状态码汇总
---
- [HTTP](#http)
  - [1xx消息](#1xx消息)
  - [2xx成功](#2xx成功)
  - [3xx重定向](#3xx重定向)
  - [4xx客户端错误](#4xx客户端错误)
  - [5xx服务器错误](#5xx服务器错误)
- [FTP](#ftp)
  - [1xx初步](#1xx初步)
  - [2xx完成](#2xx完成)
  - [3xx中间](#3xx中间)
  - [4xx瞬态否定](#4xx瞬态否定)
  - [5xx永久性否定](#5xx永久性否定)
  - [6xx受保护](#6xx受保护)


## HTTP
超文本传输协议（HyperText Transfer Protocol），是因特网上最为广泛的一种网络传输协议，所有的[WWW](https://baike.baidu.com/item/www/109924?fr=aladdin)文件都必须遵循这个标准。
它定义了客户端可能会发给服务器什么样的消息以及可能会得到什么样的响应，基于[TCP/IP](https://baike.baidu.com/item/TCP%2FIP%E5%8D%8F%E8%AE%AE/212915?fromtitle=tcp%2Fip&fromid=214077&fr=aladdin)通讯协议来传递数据（HTML 文件, 图片文件, 查询结果等）。
**一般在开发过程中，主要是网页查看服务器端给出的响应，来判定自己的程序可能会是什么问题，以下是对常见的状态码（响应码）作出一个汇总。**

### 1xx消息

