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

#### Helper Classes

#### Redirection and Other Request Changes

#### Authentication and Credentials

#### Cache Management

#### Cookie Storage

#### Protocol Support

## How to Use This Document



