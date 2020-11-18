- [FTP](#ftp)
  - [1xx初步](#1xx初步)
  - [2xx完成](#2xx完成)
  - [3xx中间](#3xx中间)
  - [4xx瞬态否定](#4xx瞬态否定)
- [5xx永久性否定](#5xx永久性否定)
- [6xx受保护](#6xx受保护)
- [WebSockets状态码](#websockets状态码)

## FTP

### 1xx初步

_肯定的初步答复，这些状态代码指示一项操作已经成功开始，但客户端希望在继续操作新命令前得到另一个答复。_

- 110 重新启动标记答复。
- 120 服务已就绪，在 nnn 分钟后开始。
- 125 数据连接已打开，正在开始传输。
- 150 文件状态正常，准备打开数据连接。

### 2xx完成

_肯定的完成答复，一项操作已经成功完成。客户端可以执行新命令。_

- 200 命令确定。
- 202 未执行命令，站点上的命令过多。
- 211 系统状态，或系统帮助答复。
- 212 目录状态。
- 213 文件状态。
- 214 帮助消息。
- 215 NAME 系统类型，其中，NAME 是 Assigned Numbers 文档中所列的正式系统名称。
- 220 服务就绪，可以执行新用户的请求。
- 221 服务关闭控制连接。如果适当，请注销。
- 225 数据连接打开，没有进行中的传输。
- 226 关闭数据连接。请求的文件操作已成功（例如，传输文件或放弃文件）。
- 227 进入被动模式 (h1,h2,h3,h4,p1,p2)。
- 230 用户已登录，继续进行。
- 250 请求的文件操作正确，已完成。
- 257 已创建“PATHNAME”。

### 3xx中间

_肯定的中间答复，该命令已成功，但服务器需要更多来自客户端的信息以完成对请求的处理。_

- 331 用户名正确，需要密码。
- 332 需要登录帐户。
- 350 请求的文件操作正在等待进一步的信息。

### 4xx瞬态否定

_瞬态否定的完成答复，该命令不成功，但错误是暂时的。如果客户端重试命令，可能会执行成功。_

- 421 服务不可用，正在关闭控制连接。如果服务确定它必须关闭，将向任何命令发送这一应答。
- 425 无法打开数据连接。
- 426 Connection closed; transfer aborted.
- 450 未执行请求的文件操作。文件不可用（例如，文件繁忙）。
- 451 请求的操作异常终止：正在处理本地错误。
- 452 未执行请求的操作。系统存储空间不够。

## 5xx永久性否定

_永久性否定的完成答复，该命令不成功，错误是永久性的。如果客户端重试命令，将再次出现同样的错误。_

- 500 语法错误，命令无法识别。这可能包括诸如命令行太长之类的错误。
- 501 在参数中有语法错误。
- 502 未执行命令。
- 503 错误的命令序列。
- 504 未执行该参数的命令。
- 530 未登录。
- 532 存储文件需要帐户。
- 550 未执行请求的操作。文件不可用（例如，未找到文件，没有访问权限）。
- 551 请求的操作异常终止：未知的页面类型。
- 552 请求的文件操作异常终止：超出存储分配（对于当前目录或数据集）。
- 553 未执行请求的操作。不允许的文件名。

## 6xx受保护

- 600 Series，Replies regarding confidentiality and integrity
- 631 Integrity protected reply.
- 632 Confidentiality and integrity protected reply.
- 633 Confidentiality protected reply.

## WebSockets状态码

_WebSockets 的CloseEvent 会在连接关闭时发送给使用 WebSockets 的客户端。它在 WebSocket 对象的 onclose 事件监听器中使用。服务端发送的关闭码，以下为已分配的状态码。_

|状态码 | 名称 | 描述 |
| --- | --- | --- |
|0–999 | - |  保留段, 未使用。 |
|1000 | CLOSE_NORMAL | 正常关闭; 无论为何目的而创建, 该链接都已成功完成任务。 |
|1001 | CLOSE_GOING_AWAY | 终端离开, 可能因为服务端错误, 也可能因为浏览器正从打开连接的页面跳转离开。 |
|1002 | CLOSE_PROTOCOL_ERROR | 由于协议错误而中断连接。 |
|1003 | CLOSE_UNSUPPORTED | 由于接收到不允许的数据类型而断开连接 (如仅接收文本数据的终端接收到了二进制数据)。 |
|1004 | - | 保留。 其意义可能会在未来定义。 |
|1005 | CLOSE_NO_STATUS  | 保留。  表示没有收到预期的状态码。 |
|1006 | CLOSE_ABNORMAL | 保留。 用于期望收到状态码时连接非正常关闭 (也就是说, 没有发送关闭帧)。 |
|1007 | Unsupported Data  | 由于收到了格式不符的数据而断开连接 (如文本消息中包含了非 UTF-8 数据)。 |
|1008 | Policy Violation  | 由于收到不符合约定的数据而断开连接。 这是一个通用状态码, 用于不适合使用 1003 和 1009 状态码的场景。 |
|1009 | CLOSE_TOO_LARGE | 由于收到过大的数据帧而断开连接。 |
|1010 | Missing Extension | 客户端期望服务器商定一个或多个拓展, 但服务器没有处理, 因此客户端断开连接。 |
|1011 | Internal Error | 客户端由于遇到没有预料的情况阻止其完成请求, 因此服务端断开连接。 |
|1012 | Service Restart | 服务器由于重启而断开连接。 [Ref] |
|1013 | Try Again Later | 服务器由于临时原因断开连接, 如服务器过载因此断开一部分客户端连接。 [Ref] |
|1014 | - |由 WebSocket 标准保留以便未来使用。 |
|1015 | TLS Handshake   | 保留。 表示连接由于无法完成 TLS 握手而关闭 (例如无法验证服务器证书)。 |
|1016–1999 |  - | 由 WebSocket 标准保留以便未来使用。 |
|2000–2999 |  - | 由 WebSocket 拓展保留使用。 |
|3000–3999 |  - | 可以由库或框架使用。 不应由应用使用。 可以在 IANA 注册, 先到先得。 |
|4000–4999 |  - | 可以由应用使用。 |