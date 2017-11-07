# Using NSURLSession {#pageTitle}

## Understanding URL Session Concepts

#### Types of Sessions

NSURLSession的类型由创建它的configuration对象的类型决定

* Default sessions：持久的基于磁盘的缓存，证书存放在keychain
* Ephemeral sessions：不在磁盘存储任何数据，所有数据存于RAM并和session绑定
* Background sessions：类似于Default sessions，单独进程处理数据传输。还有一些其他限制

#### Types of Tasks

NSURLSession内部支持三种task：

* Data tasks：通过NSData接收数据，返回的数据可以分批也可以通过completion handler
* Download tasks：接收文件形式的数据，app没有运行时支持后台下载
* Upload tasks：上传文件形式的数据，app没有运行时支持后台上传

#### Background Transfer Considerations

后台下载是在另外的进程执行，所以有一些限制：

* session必须提供一个代理
* 仅支持HTTP、HTTPS
* 会有转发
* 上传仅支持从文件上传（app退出时，从data或者stream的上传会失败）
* 如果后台转发是app在后台时启动的，configuration对象的discretionary属性视为YES

iOS中，如果后台传输完成或者需要证书，而此时app没有运行，系统启动app并调用UIApplicationDelegate的application:handleEventsForBackgroundURLSession:completionHandler:。该方法回传进来session的identifier，app应该用此identifier创建一个session并保存completion handler。当session结束后，会调用session的delegate的URLSessionDidFinishEventsForBackgroundURLSession:方法。在该方法中调用回调，系统就知道再次挂起app是安全的

> 多个identifier时，必须为每一个identifier创建一个session

app挂起时，如果有任务完成，代理的URLSession:downloadTask:didFinishDownloadingToURL:会被调用，task和下载的file的URL会传进来

app挂起时，如果需要证书，session会调用代理的URLSession:task:didReceiveChallenge:completionHandler: 或者URLSession:didReceiveChallenge:completionHandler:

后台的上传或者下载会在网络失败后自动重试

#### Life Cycle and Delegate Interaction

Life Cycle of a URL Session

#### NSCopying Behavior

* 对session或者task拷贝会返回同一对象
* 对configuration对象拷贝会返回新的对象

## Sample Delegate Class Interface

* NSURLSessionDelegate
  * NSURLSessionTaskDelegate
    * NSURLSessionDataDelegate
    * NSURLSessionDownloadDelegate
    * NSURLSessionStreamDelegate

## Creating and Configuring a Session

NSURLSession API提供了一系列配置选项

* 针对个别session的cache、cookie、证书、协议提供私有存储
* 针对task或者session的认证绑定
* 支持文件的上传下载，鼓励数据和元数据分离
* 限制每一个host的最大连接数
* 针对每一个资源设置timeout
* TLS最高、最低版本的支持
* 自定义代理字典
* cookie机制的控制
* HTTP管道的控制

实例化session需要：

* 一个configuration对象管理session和它内部的task的行为
* 可选的，代理对象处理接受的数据并处理session的事件

> 如果要后台传输，必须提供自定义代理

除了background configuration，其他两种configuration可以重复使用，因为session对configuration是深拷贝

## Fetching Resources Using System-Provided Delegates

```
NSURLSession *sessionWithoutADelegate = [NSURLSession sessionWithConfiguration:defaultConfiguration];
NSURL *url = [NSURL URLWithString:@"https://www.example.com/"];

[[sessionWithoutADelegate dataTaskWithURL:url completionHandler:^(NSData *data, NSURLResponse *response, NSError *error) {
NSLog(@"Got response %@ with error %@.\n", response, error);
NSLog(@"DATA:\n%@\nEND DATA\n", [[NSString alloc] initWithData:data encoding:NSUTF8StringEncoding];
}] resume];
```

## Fetching Data Using a Custom Delegate

```
NSURL *url = [NSURL URLWithString: @"https://www.example.com/"];
NSURLSessionDataTask *dataTask = [defaultSession dataTaskWithURL:url];
[dataTask resume];
```

## Downloading Files

下载文件时需要实现的代理方法

* `URLSession:downloadTask:didFinishDownloadingToURL:`提供临时文件的路径
* > 此方法结束后，临时文件会被删除
* `URLSession:downloadTask:didWriteData:totalBytesWritten:totalBytesExpectedToWrite:`下载的进度
* `URLSession:downloadTask:didResumeAtOffset:expectedTotalBytes:` 断点续传
* `URLSession:task:didCompleteWithError:` 下载失败

下载中途暂停再开始的步骤

* `cancelByProducingResumeData:`
* `downloadTaskWithResumeData:`/`downloadTaskWithResumeData:completionHandler:`

在失败的回调`URLSession:task:didCompleteWithError:` 中，对于可重新开始的任务，error对象中获取`userInfo`，它里面包含了已下载的数据，然后调用`downloadTaskWithResumeData:`/`downloadTaskWithResumeData:completionHandler:`

## Uploading Body Content

对于HTTP POST请求，可以以三种方式向body里面提供数据

* NSData：数据在内存
* 文件：文件在磁盘
* 流：数据来自网络

无论哪种方式，如果提供了自定义代理，它的`URLSession:task:didSendBodyData:totalBytesSent:totalBytesExpectedToSend:`将会被调用来通知上传进度

如果上传的是流，必须为session提供一个实现了`URLSession:task:needNewBodyStream:`的代理

#### Uploading Body Content Using an NSData Object

上传NSData需要调用`uploadTaskWithRequest:fromData:`或者`uploadTaskWithRequest:fromData:completionHandler:`

对于请求的头部信息，session对象自动计算`Content-Length`，其他的字段根据需要提供

```
NSURL *textFileURL = [NSURL fileURLWithPath:@"/path/to/file.txt"];
NSData *data = [NSData dataWithContentsOfURL:textFileURL];
 
NSURL *url = [NSURL URLWithString:@"https://www.example.com/"];
NSMutableURLRequest *mutableRequest = [NSMutableURLRequest requestWithURL:url];
mutableRequest.HTTPMethod = @"POST";
[mutableRequest setValue:[NSString stringWithFormat:@"%lld", data.length] forHTTPHeaderField:@"Content-Length"];
[mutableRequest setValue:@"text/plain" forHTTPHeaderField:@"Content-Type"];
 
NSURLSessionUploadTask *uploadTask = [defaultSession uploadTaskWithRequest:mutableRequest fromData:data];
[uploadTask resume];
```

#### Uploading Body Content Using a File

#### Uploading Body Content Using a Stream

#### Uploading a File Using a Download Task

## Handling Authentication and Custom TLS Chain Validation

## Handling iOS Background Activity



