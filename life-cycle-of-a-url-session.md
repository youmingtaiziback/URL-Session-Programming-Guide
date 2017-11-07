# Life Cycle of a URL Session {#pageTitle}

可以通过两种方式调用NSURLSession API，系统提供代理、自定义代理。有以下情况时，需要使用自定义代理：

* 后台上传下载
* 自定义认证
* SSL证书校验
* 根据服务器返回的MIME类型决定将数据写入磁盘还是展示
* 上传流
* 限制缓存机制
* 限制HTTP转发



