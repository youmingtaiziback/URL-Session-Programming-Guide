# About the URL Loading System

URL loading system的功能包括：下载、上传、管理cookie、控制response caching、处理证书存储、认证、自定义协议扩展

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

* 对于简单请求，直接用NSURLSession的api请求，返回NSData或者磁盘文件
* 对于复杂请求，比如上传数据，为NSURLSession提供一个NSURLRequest

无论采取哪种方式，都可以通过两种方式获得返回数据

* completion block：结束后一次调用
* delegate：累加

有NSURLSession触发的下载不会被缓存，如果需要只能手动实现

#### Helper Classes

下载类通过两类辅助类提供元数据：

* NSURLRequest：使用NSMutableURLRequest下载时，系统会对request进行深拷贝
* NSURLResponse：包括
  * 元数据：MIME类型、数据长度、编码、URL
  * 数据本身

#### Redirection and Other Request Changes

#### Authentication and Credentials

#### Cache Management

#### Cookie Storage

#### Protocol Support

## How to Use This Document

* [Using NSURLSession](/using-nsurlsession.md)：概括URL Loading System
* [Life Cycle of a URL Session](/life-cycle-of-a-url-session.md)：详细介绍session如何与代理交互
* 
1

1

1

1

1

1

1

1

1

1

1

1

1

1

1

1

1

1

1

1

1

1

1

1

1

