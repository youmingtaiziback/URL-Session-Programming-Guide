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

#### Life Cycle and Delegate Interaction

#### NSCopying Behavior

## Sample Delegate Class Interface

## Creating and Configuring a Session

## Fetching Resources Using System-Provided Delegates

## Fetching Data Using a Custom Delegate

## Downloading Files

## Uploading Body Content

## Handling Authentication and Custom TLS Chain Validation

## Handling iOS Background Activity



