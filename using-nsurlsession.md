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

## Uploading Body Content

## Handling Authentication and Custom TLS Chain Validation

## Handling iOS Background Activity



