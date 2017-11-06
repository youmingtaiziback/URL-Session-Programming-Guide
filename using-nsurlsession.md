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

## Sample Delegate Class Interface

## Creating and Configuring a Session

## Fetching Resources Using System-Provided Delegates

## Fetching Data Using a Custom Delegate

## Downloading Files

## Uploading Body Content

## Handling Authentication and Custom TLS Chain Validation

## Handling iOS Background Activity



