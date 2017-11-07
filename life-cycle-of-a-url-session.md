# Life Cycle of a URL Session {#pageTitle}

可以通过两种方式调用NSURLSession API，系统提供代理、自定义代理。有以下情况时，需要使用自定义代理：

* 后台上传下载
* 自定义认证
* SSL证书校验
* 根据服务器返回的MIME类型决定将数据写入磁盘还是展示
* 上传流
* 限制缓存机制
* 限制HTTP转发

## Life Cycle of a URL Session with System-Provided Delegates

系统提供代理方式时，相关方法的调用顺序：

* 创建session configuration，对于后台任务，需要提供唯一的identifier
* 创建session，delegate为空
* 创建任务
  * 任务开始时处于挂起状态，调用resume后启动
  * > 如果创建`NSURLSession`时没有提供`delegate`，任务就必须有`completionHandler`参数，否则拿不到收据
* 断点续传
  * `cancelByProducingResumeData:`
  * `downloadTaskWithResumeData:` /`downloadTaskWithResumeData:completionHandler:`
* 下载结束后，NSURLSession调用completion handler
  * > NSURLSession的error参数中不会描述服务器端的错误，只描述客户端的错误。错误码定义在URL Loading System Error Codes

## Life Cycle of a URL Session with Custom Delegates

## 



