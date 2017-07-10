# 1, 正则表达式
## 1, 常见的正则表达式符号和特殊字符
| 符号 | 描述 |	 示例	|
|--------|--------|--------|
|   literal     |    匹配文本字符串的字面值literal    |	foo		|
|	re1&#124;re2		|	匹配正则表达式re1或re2			|	foo&#124;bar	|
|	.		|	匹配任意字符(除了\n之外) 	|	b.b	|
|	^	|	匹配字符串其实部分	|	^Dear	|
|	$	|	匹配字符串终止部分	|	/bin/*sh$	|
|	*	|	匹配0次或者多次前面出现的正则表达式	|	[A-Za-z0-9]*	|
|	+	|	匹配1次或者多次前面出现的正则表达式	|	[a-z]+\.com	|
|	?	|	匹配0次或者1次前面出现的正则表达式	|	goo?	|
|	{N}	|	匹配N次前面出现的正则表达式	|	[0-9]{3}	|
|	{M,N}	|	匹配M~N次前面出现的正则表达式	|	[0-9],[A-Za-z]	|
|	[...]	|	匹配来自字符集的任意单一字符	|	[aeiou]	|
|	[..x-y..]	|	匹配x~y范围中的任意单一字符	|	[0-9],[A-Za-z]	|
|	[^...]	|	不匹配此字符集中出现的任意一个字符, 包括某一范围内的字符(如果在此字符集中出现)	|	[^aeiou],[^A-Za-z0-9]	|
|	(*&#124;+&#124;?&#124;{})?	|	用于匹配上面频繁出现/重复出现符号的非贪婪版本(*, +, ?, {}) 	|	.*?[a-z]	|
|	(...)	|	匹配封闭的正则表达式,然后另存为子组	|	([0-9]{3})?.f(00&#124;u)bar	|
|	\d	|	匹配任何十进制数字,与[0-9]一致(\D与\d相反, 不匹配任何分数型的数字)	|	data\d+.txt	|
|	\w	|	匹配任何字母数字字符, 与[A-Za-z0-9_]相同 (\W相反)	|	[A-Za-z_]\w+	|
|	\s	|	匹配任何空格字符, 与[\n\t\r\v\f]相同(\S与之相反)	|	of\sthe	|
|	\b	|	匹配单词边界 (\B与之相反)	|	\bThe\b	|
|	\N	|	匹配以保存的子组 (参见 (...))	|	price:\16	|
|	\c	|	逐字匹配任何特殊字符c (即, 仅按照字面意义匹配, 不匹配特殊含义)	|	\\.,\\\,\\*	|
|	\A(\Z)	|	匹配字符串的启始 (结束) (另见 ^和$)	|	\ADear	|


# 2, 网络编程
## 1, 套接字
### 1, 地址家族(address family)
-	基于文件
	-	AF_LOCAL 或 AF_UNIX
-	基于网络
	-	AF_INET 或 AF_INET6

### 2, 	面向连接和无连接的套接字
-	面向连接的套接字
	-	SOCK_STREAM

-	无连接的套接字
	-	SOCK_DGRAM

## 2, socket模块
-	一般语法
>	from socket import *  
>	socket(socket_family, socket_type, protocol=0)  
>	protocol通常省略, 默认为0

-	套接字对象内置方法

| 方法 | 描述 |
|--------|--------|
| 服务器套接字	|	|
|    s.bind()    |  将地址(主机名, 端口号对)绑定到套接字上      |
|	s.listen()	|	设置并启动TCP监听器	|
|	s.accept()	|	被动接受TCP客户端连接, 一直等待直到连接到达(阻塞)	|
|	客户端套接字方法	|	|
|	s.connect()	|	主动发起TCP服务器连接	|
|	s.connect_ex()	|	connect()的扩展版本, 此时会以错误码的形式返回问题, 而不是抛出异常	|
|	普通套接字方法	|	|
|	s.recv()	|	接受TCP消息	|
|	s.recv_into()	|	接受TCP消息到指定的缓冲区	|
|	s.send()	|	发送TCP消息	|
|	s.sendall()	|	完整的发送TCP消息	|
|	s.recvfrom()	|	接受UDP消息	|
|	s.recvfrom_into()	|	接受UDP消息到指定的缓冲区	|
|	s.sendto()	|	发送UDP消息	|
|	s.getpeername()	|	接受到套接字(TCP)的远程地址	|
|	s.getsockname()	|	当前套接字的地址	|
|	s.getsockopt()	|	返回给定套接字选项的值	|
|	s.setsockopt()	| 	设置给定套接字选项的值	|
|	s.shutdown()	|	关闭连接	|
|	s.close()	|	关闭套接字	|
|	s.detach()	|	在未关闭文件描述符的情况下关闭套接字, 返回文件描述符	|
|	s.ioctl()	|	控制套接字的模式(仅支持Windows)	|
|	面向阻塞的套接字方法	|	|
|	s.setblocking()	|	设置套接字的阻塞或非阻塞模式	|
|	s.settimeout()	|	设置阻塞套接字操作的超时时间	|
|	s.gettimeout()	|	获取套接字操作的超时时间	|
|	面向文件的套接字方法	|	|
|	s.fileno()	|	套接字的文件描述符	|
|	s.makefile()	|	创建与套接字关联的文件对象	|
|	数据属性	|	|
|	s.family	|	套接字家族	|
|	s.type	|	套接字类型	|
|	s.proto	|	套接字协议	|

## 3, 实现连接
### 1, TCP连接
-	服务器

```
# coding: utf-8
from socket import *
from time import ctime

HOST = ''
PORT = 21567
BUFSIZ = 1024
ADDR = (HOST, PORT)

# 创建服务器套接字
s = socket(AF_INET, SOCK_STREAM)
# 套接字与地址绑定
s.bind(ADDR)
# 监听连接
s.listen(5)
# 服务器无限循环
while True:
    print '等待连接...'
    # 接受客户端连接
    tcpCliSock, addr = s.accept()
    print '... 连接: ', addr

    # 通信循环
    while True:
        # 接受消息
        data = tcpCliSock.recv(BUFSIZ)
        if not data:
            break
        # 发送消息
        tcpCliSock.send('[%s] %s' % (ctime(), data))

    # 关闭客户端套接字
    tcpCliSock.close()

```

-	客户端

```
# coding: utf-8
from socket import *

HOST = 'localhost'
PORT = 21567
BUFSIZ = 1024
ADDR = (HOST, PORT)

# 创建客户端套接字
s = socket(AF_INET, SOCK_STREAM)
# 尝试连接服务器
s.connect(ADDR)

# 通信循环
while True:
    # 输入信息
    data = raw_input('> ')
    if not data:
        break
    # 发送信息
    s.send(data)
    # 接受信息
    data = s.recv(BUFSIZ)
    if not data:
        break
    print data

# 关闭客户端套接字
s.close()

```

### 2, UDP连接

-	服务器

```
# coding: utf-8
from socket import *

HOST = ''
PORT = 21567
BUFSIZ = 1024
ADDR = (HOST, PORT)

# 创建服务器套接字
s = socket(AF_INET, SOCK_DGRAM)
# 绑定服务器套接字
s.bind(ADDR)

# 服务器无限循环
while True:
    print '等待...'
    # 接受信息
    data, addr = s.recvfrom(BUFSIZ)
    # 发送信息
    s.sendto('UDP连接: ' , addr)
    print 'received:', addr

```

-	客户端

```
# coding: utf-8
from socket import *

HOST = 'localhost'
PORT = 21567
BUFSIZ = 1024
ADDR = (HOST, PORT)

# 创建客户端套接字
s = socket(AF_INET, SOCK_DGRAM)

# 通信循环
while True:
    data = raw_input('> ')
    if not data:
        break
    # 发送信息
    s.sendto(data, ADDR)
    # 接受信息
    data, ADDR = s.recvfrom(BUFSIZ)
    if not data:
        break
    print data

# 关闭客户端套接字
s.close()

```