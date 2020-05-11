## 4. 计算机网络

### 4.1 HTTP1.0 和 HTTP1.1 有什么区别。

HTTP 1.0主要有以下几点变化：
* 请求和相应可以由于多行首部字段构成
* 响应对象前面添加了一个响应状态行
* 响应对象不局限于超文本
* 服务器与客户端之间的连接在每次请求之后都会关闭
* 实现了Expires等传输内容的缓存控制
* 内容编码Accept-Encoding、字符集Accept-Charset等协商内容的支持
* 这时候开始有了请求及返回首部的概念，开始传输不限于文本（其他二进制内容）
* HTTP 1.1加入了很多重要的性能优化：持久连接、分块编码传输、字节范围请求、增强的缓存机制、传输编码及请求管道。

---

1. **HTTP 1.0 **的特点
* 请求与响应支持头域
* 响应对象以一个响应状态行开始
* 响应对象不只限于超文本
* 开始支持客户端通过POST方法向Web服务器提交数据，支持GET、HEAD、POST方法
* 支持长连接（但默认还是使用短连接），缓存机制，以及身份认证
2. **HTTP1.1**的特点
* HTTP1.1新增了五种请求方法：OPTIONS, PUT, DELETE, TRACE 和 CONNECT 方法。
* GET 请求指定的页面信息，并返回实体主体。
* HEAD 类似于get请求，只不过返回的响应中没有具体的内容，用于获取报头
* POST 向指定资源提交数据进行处理请求（例如提交表单或者上传文件）。数据被包含在请求体中。POST请求可能会导致新的资源的建立和/或已有资源的修改。
* PUT 从客户端向服务器传送的数据取代指定的文档的内容。
* DELETE 请求服务器删除指定的页面。
* CONNECT HTTP/1.1协议中预留给能够将连接改为管道方式的代理服务器。
* OPTIONS 允许客户端查看服务器的性能。 TRACE 回显服务器收到的请求，主要用于测试或诊断。

### 4.2 TCP 三次握手和四次挥手的流程，为什么断开连接要 4 次，如果握手只有两次，会出现什么。

**三次握手流程**:

* 第一次握手(SYN=1, seq=x)：
客户端发送一个 TCP 的 SYN 标志位置1的包，指明客户端打算连接的服务器的端口，以及初始序号 X,保存在包头的序列号(Sequence Number)字段里。
发送完毕后，客户端进入 <code>SYN_SEND</code状态。
* 第二次握手(SYN=1, ACK=1, seq=y, ACKnum=x+1)：
服务器发回确认包(ACK)应答。即 SYN 标志位和 ACK 标志位均为1。服务器端选择自己 ISN 序列号，放到 Seq 域里，同时将确认序号(Acknowledgement Number)设置为客户的 ISN 加1，即X+1。
发送完毕后，服务器端进入 <code>SYN_RCVD</code状态。
* 第三次握手(ACK=1，ACKnum=y+1)：
客户端再次发送确认包(ACK)，SYN 标志位为0，ACK 标志位为1，并且把服务器发来 ACK 的序号字段+1，放在确定字段中发送给对方，并且在数据段放写ISN的+1

发送完毕后，客户端进入 ESTABLISHED 状态，当服务器端接收到这个包时，也进入 ESTABLISHED 状态，TCP 握手结束。

**四次挥手流程**：

* 第一次挥手(FIN=1，seq=x)：
假设客户端想要关闭连接，客户端发送一个 FIN 标志位置为1的包，表示自己已经没有数据可以发送了，但是仍然可以接受数据。
发送完毕后，客户端进入 FIN_WAIT_1 状态。

* 第二次挥手(ACK=1，ACKnum=x+1)：
服务器端确认客户端的 FIN 包，发送一个确认包，表明自己接受到了客户端关闭连接的请求，但还没有准备好关闭连接。
发送完毕后，服务器端进入 CLOSE_WAIT 状态，客户端接收到这个确认包之后，进入 FIN_WAIT_2 状态，等待服务器端关闭连接。

* 第三次挥手(FIN=1，seq=y)：
服务器端准备好关闭连接时，向客户端发送结束连接请求，FIN 置为1。
发送完毕后，服务器端进入 LAST_ACK 状态，等待来自客户端的最后一个ACK。

* 第四次挥手(ACK=1，ACKnum=y+1)：
客户端接收到来自服务器端的关闭请求，发送一个确认包，并进入 TIME_WAIT状态，等待可能出现的要求重传的 ACK 包。
服务器端接收到这个确认包之后，关闭连接，进入 CLOSED 状态。
客户端等待了某个固定时间（两个最大段生命周期，2MSL，2 Maximum Segment Lifetime）之后，没有收到服务器端的 ACK ，认为服务器端已经正常关闭连接，于是自己也关闭连接，进入 CLOSED 状态。



**如果握手只有两次**，两次后会重传直到超时。如果多了会有大量半链接阻塞队列。

[TCP三次握手四次挥手](https://segmentfault.com/a/1190000006885287)

[TCP 协议](https://hit-alibaba.github.io/interview/basic/network/TCP.html)

### 4.3 TIME_WAIT 和 CLOSE_WAIT 的区别。

* TIME_WAIT状态就是用来重发可能丢失的ACK报文。
* TIME_WAIT 表示主动关闭，CLOSE_WAIT 表示被动关闭。

### 4.4 说说你知道的几种 HTTP 响应码，比如 200, 302, 404。

* 1xx：信息，请求收到，继续处理
* 2xx：成功，行为被成功地接受、理解和采纳
* 3xx：重定向，为了完成请求，必须进一步执行的动作
* 4xx：客户端错误，请求包含语法错误或者请求无法实现
* 5xx：服务器错误，服务器不能实现一种明显无效的请求
* 200 ok 一切正常
* 302 Moved Temporatily 文件临时移出
* 404 not found

[Http响应码及其含义--摘自apache官网](https://my.oschina.net/gavinjin/blog/42856)

### 4.5 当你用浏览器打开一个链接的时候，计算机做了哪些工作步骤。

**DNS解析** –**端口分析** –**TCP请求** –**服务器处理请求** –**服务器响应** –**浏览器解析** —**链接关闭**

### 4.6 TCP/IP 如何保证可靠性，说说 TCP 头的结构。

* 使用序号，对收到的TCP报文段进行排序以及检测重复的数据；
* 使用校验和来检测报文段的错误；
* 使用确认和计时器来检测和纠正丢包或延时。

```java
//TCP头部，总长度20字节
typedef struct _tcp_hdr
{
unsigned short src_port;    //源端口号
unsigned short dst_port;    //目的端口号
unsigned int seq_no;        //序列号
unsigned int ack_no;        //确认号
#if LITTLE_ENDIAN
unsigned char reserved_1:4; //保留6位中的4位首部长度
unsigned char thl:4;        //tcp头部长度
unsigned char flag:6;       //6位标志
unsigned char reseverd_2:2; //保留6位中的2位
#else
unsigned char thl:4;        //tcp头部长度
unsigned char reserved_1:4; //保留6位中的4位首部长度
unsigned char reseverd_2:2; //保留6位中的2位
unsigned char flag:6;       //6位标志
#endif
unsigned short wnd_size;    //16位窗口大小
unsigned short chk_sum;     //16位TCP检验和
unsigned short urgt_p;      //16为紧急指针
}
tcp_hdr;
```

[TCP/IP协议头部结构体（网摘小结）](https://www.cnblogs.com/lancidie/archive/2013/05/16/3082378.html)

### 4.7 如何避免浏览器缓存。

* 无法被浏览器缓存的请求：
HTTP信息头中包含Cache-Control:no-cache，pragma:no-cache，或Cache-Control:max-age=0等告诉浏览器不用缓存的请求。需要根据Cookie，认证信息等决定输入内容的动态请求是不能被缓存的，经过HTTPS安全加密的请求（有人也经过测试发现，ie其实在头部加入Cache-Control：max-age信息，firefox在头部加入Cache-Control:Public之后，能够对HTTPS的资源进行缓存，参考《HTTPS的七个误解》）
* POST请求无法被缓存：
HTTP响应头中不包含Last-Modified/Etag，也不包含Cache-Control/Expires的请求无法被缓存。

[【Web 缓存机制系列】2 – Web 浏览器的缓存机制](http://www.alloyteam.com/2012/03/web-cache-2-browser-cache/)

### 4.8 简述 Http 请求 get 和 post 的区别以及数据包格式。

|                  | GET                                                          | POST                                                         |
| ---------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 回退按钮/刷新    | 无害                                                         | 数据会被重新提交（浏览器应该告知用户数据会被重新提交）       |
| 书签             | 可收藏为书签                                                 | 不可收藏为书签                                               |
| 缓存             | 能被缓存                                                     | 不能缓存                                                     |
| 编码类型         | application/x-www-form-urlencoded                            | application/x-www-form-urlencoded或multipart/form-data。为二进制数据使用多重编码 |
| 历史             | 参数保留在浏览器历史中                                       | 参数不会保存在浏览器历史中                                   |
| 对数据长度的限制 | 是的。当发送数据时，GET方法向URL添加数据；URL的长度是受限制的（URL的最大长度是2048个字符） | 无限制                                                       |
| 对数据类型的限制 | 只允许ASCII字符                                              | 没有限制。也允许二进制数据                                   |
| 安全性           | 与POST相比，GET的安全性较差，因为所发送的数据是URL的一部分。<br />在发送密码或其他敏感信息时绝不要使用GET | POST比GET更安全，因为参数不会被保存在浏览器历史或web服务器日志中 |
| 可见性           | 数据在URL中所有人都是可见的                                  | 数据不再显示在URL中                                          |

[HTTP 方法：GET 对比 POST](https://www.w3school.com.cn/tags/html_ref_httpmethods.asp)

### 4.9 简述 HTTP 请求的报文格式。

[HTTP的报文格式、GET和POST格式解释](http://www.360doc.com/content/12/0612/14/8093902_217673378.shtml)

### 4.10 HTTPS 的加密方式是什么，讲讲整个加密解密流程。

加密方式是tls/ssl，底层是通过对称算法，非对称，hash算法实现

1. 客户端发起HTTPS请求
2. 服务端的配置
3. 传送证书
4. 客户端解析证书
5. 传送加密信息
6. 服务段解密信息
7. 传输加密后的信息
8. 客户端解密信息

[图解HTTPS](https://www.cnblogs.com/zhuqil/archive/2012/07/23/2604572.html)