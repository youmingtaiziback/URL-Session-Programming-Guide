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

**Fetching Content as Data \(In Memory\)**

* 对于简单请求，用NSURLSession的api直接请求URL，返回NSData或者磁盘文件
* 对于复杂请求，比如上传数据，为NSURLSession提供一个NSURLRequest

无论采取哪种方式，都可以通过两种方式获得返回数据

* completion block：结束后一次调用
* delegate：累加

**Downloading Content as a File**

* 对于简单请求，用NSURLSession的api直接请求URL，返回NSData或者磁盘文件
* 对于复杂请求，比如上传数据，为NSURLSession提供一个NSURLRequest

> 由NSURLSession触发的下载不会被缓存，如果需要只能手动实现

#### Helper Classes

下载类通过两类辅助类提供元数据：`NSURLRequest`、`NSURLResponse`

**URL Requests**

> 当app从NSMutableURLRequest发起一个连接或者下载时，对NSMutableURLRequest会进行深拷贝

**Response Metadata**

从服务器返回的响应可分为两部分：元数据和内容。元数据包括MIME类型、数据长度、编码、URL

> NSURLResponse对象只包含元数据，数据本身会通过block或者代理返回。NSCachedURLResponse对象封装了NSURLResponse、URL数据和app提供的其他信息

#### Redirection and Other Request Changes

有些协议，比如HTTP会让服务器告诉客户端数据已经移动到不同的URL了。app端会收到回调并且做出响应

#### Authentication and Credentials

服务器可以保护特定的数据，让客户端通过证书或者用户名密码访问

URL loading system提供了类用来持久化存储证书，可以为单个request、app运行期间、永久存储在key chain中

[NSURLCredentialStorage](https://developer.apple.com/documentation/foundation/nsurlcredentialstorage)为session管理证书存储，并提供[NSURLCredential](https://developer.apple.com/documentation/foundation/urlcredential)到[NSURLProtectionSpace](https://developer.apple.com/documentation/foundation/nsurlprotectionspace)的映射。只有当鉴权查询成功证书才会被存储

[NSURLAuthenticationChallenge](https://developer.apple.com/documentation/foundation/urlauthenticationchallenge)封装了[NSURLProtocol](https://developer.apple.com/documentation/foundation/nsurlprotocol)的实现对请求进行认证所需的信息：证书、protection space、error或者response、已经尝试过的认证次数

NSURLAuthenticationChallenge的实例会被NSURLProtocol的子类用来通知URL loading system需要进行认证，也会被传递给[NSURLSession](https://developer.apple.com/documentation/foundation/nsurlsession)的代理方法

#### Cache Management

URL loading system提供了内存、磁盘缓存，缓存是基于单个app的。session根据NSURLRequest和NSURLSessionConfiguration的缓存策略访问缓存

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



