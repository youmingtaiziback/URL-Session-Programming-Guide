# About the URL Loading System

URL loading system的功能包括：下载、上传、管理cookie存储、控制response caching、处理证书存储、认证、自定义协议扩展

URL loading system支持的协议：

* File Transfer Protocol \(ftp://\)
* Hypertext Transfer Protocol \(http://\)
* Hypertext Transfer Protocol with encryption \(https://\)
* Local file URLs \(file:///\)
* Data URLs \(data://\)

> Apple平台上网络安全的特性叫App Transport Security \(ATS\)，默认开启

## At a Glance

URL loading system的辅助类可以分为五种：协议支持、认证和证书、cookie存储、配置管理、缓存管理

![](/assets/import.png)

#### URL Loading

Fetching Content as Data \(In Memory\)

* 对于简单请求，用NSURLSession的api直接请求URL，返回NSData或者磁盘文件
* 对于复杂请求，比如上传数据，为NSURLSession提供一个NSURLRequest

无论采取哪种方式，都可以通过两种方式获得返回数据

* completion block：结束后一次调用
* delegate：累加

> 由NSURLSession触发的下载不会被缓存，如果需要只能手动实现

#### Helper Classes

下载类通过两类辅助类提供元数据：

* NSURLRequest：使用NSMutableURLRequest下载时，系统会对request进行深拷贝
* NSURLResponse：包括
  * 元数据：MIME类型、数据长度、编码、URL
  * 数据本身

#### Redirection and Other Request Changes

#### Authentication and Credentials

#### Cache Management

URL loading system提供了内存、磁盘缓存，缓存时基于单个app的。session根据NSURLRequest和NSURLSessionConfiguration的缓存策略访问缓存

NSURLCache提供配置缓存大小、缓存在磁盘上的位置的接口。也提供了管理缓存response的接口

目前只有http和https支持缓存

URLSession:dataTask:willCacheResponse:completionHandler:决定是否缓存response、是否只在内存做缓存

#### Cookie Storage

由于HTTP协议的无状态特性，URL loading system提供了创建cookie、管理cookie、通过request发送cookie、通过response接收cookie的接口。iOS的cookie存储是基于单个app的

#### Protocol Support

URL loading system默认支持http、https、file、ftp、data等协议，也支持应用级别的自定义协议。还可以给request和response添加协议相关的属性

## How to Use This Document

* [Using NSURLSession](/using-nsurlsession.md)：概括URL Loading System
* [Life Cycle of a URL Session](/life-cycle-of-a-url-session.md)：详细介绍session如何与代理交互
* [Encoding URL Data](/encoding-and-decoding-url-data.md)：如果把任意字符串转化为URL可安全使用的
* [Handling Redirects and Other Request Changes](/handling-redirects-and-other-request-changes.md)：响应URL request改变的方法
* [Authentication Challenges and TLS Chain Validation](/authentication-challenges-and-tls-chain-validation.md)：与服务器认证的过程
* [Understanding Cache Access](/understanding-cache-access.md)：在请求过程中如何使用cache
* [Cookies and Custom Protocols](/cookies-and-custom-protocols.md)：cookie存储和应用级自定义协议的支持



