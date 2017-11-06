# Using NSURLSession {#pageTitle}

## Understanding URL Session Concepts

#### Types of Sessions

NSURLSession的类型由创建它的configuration对象的类型决定

* Default sessions：持久的基于磁盘的缓存，证书存放在keychain
* Ephemeral sessions：不在磁盘存储任何数据，所有数据存于RAM并和session绑定
* Background sessions：类似于Default sessions，单独进程处理数据传输

#### Types of Tasks

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



