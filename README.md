# 简介

- [FC 常见问题索引](https://help.aliyun.com/document_detail/56102.html)

- S 工具
  - [S 工具十分钟 CookBook](./docs/s_fc_cookbook/readme.md)
  - [使用s工具安装字体.md](./docs/使用s工具安装字体.md)
  - [使用s工具进行克隆函数](./docs/使用s工具进行克隆函数.md)
  - [使用s工具实现函数多region部署](./docs/使用s工具实现函数多region部署.md)

- 权限
  - [函数计算子账号权限设置](./docs/子账号权限.md)
  - [No permission to assume the role](./docs/service_role.md)

- 常用示例
  - [FC 经典案例](./docs/FC经典案例.md)
  - [函数计算入门示例](./docs/函数计算入门示例.md)
  - [浏览器 js 如何调用函数](./docs/fc-js.md)

- 其他
  - [CustomRuntime安装其它语言版本解释器](./docs/CustomRuntime安装其它语言版本解释器.md)
  - [函数计算自定义 DNS](./docs/函数计算自定义DNS.md)
  - [PHP 运行时自定义扩展使用及下载](./docs/php运行时自定义扩展使用及下载.md)
  - [PHP Runtime 出错信息有 FastCGI](./docs/php-fastcgi.md)
  - [Custom/PHP Runtime安装自定义扩展](./docs/CustomRuntime、phpRuntime安装自定义扩展.md)
  - [大代码包部署的实践案例](./docs/大代码包部署的实践案例.md)
  - [如何开发发布一个ServerlessDevs的应用](./docs/如何开发发布一个ServerlessDevs的应用.md)
  - [使用FC修改NAS文件权限](./docs/使用FC修改NAS文件权限.md)

# 其他问题小结

**1. 函数计算的公网IP是什么怎么查看，需要在数据库或第三方服务设置函数计算的白名单**

函数计算的IP是动态且不可枚举的，可以参考文档配置[固定公网IP地址](https://help.aliyun.com/document_detail/410740.html)配合专有网络VPC的公网+NAT网关来完成。

**2. 使用频率比较低的函数调用时间为什么调用会稍长/为什么调用函数都需要重新启动服务**

函数计算默认是按量模式，是通过请求自动触发实例的创建，当首次发起调用时需要等待实例冷启动，在调用增加时创建实例，在请求减少后销毁实例（一般3-5分钟）。


**3. 怎样保留实例一直存活不被销毁希望消除冷启动延时的影响**

[预留模式](https://help.aliyun.com/document_detail/185038.html)是将函数实例的分配和释放交由用户管理，当配置预留函数实例后，预留的函数实例将会常驻，直到用户主动将其释放。

**4. 程序中的时间有时差/日志服务中记录的时间与程序中获取的不一致**

函数计算默认是UTC时间，可以配置环境变量进行修改。

- 可以配置环境变量进行时区修改：TZ = Asia/Shanghai  如何配置环境变量

- 需要注意的是，在nodejs runtime环境中，console.log(date) 会转成utc 时间，可以直接先转成 string ， 然后在console.log 
 var date = new Date();
 console.info(date.toTimeString());

**5. 函数执行超时 Function time out after**

如果函数调用出现偶现的超时， 可以先让用户尝试如下操作：
-  将函数的 timeout 调整大些
-  检查函数逻辑，增加日志， 看看是不是调用其他接口返回超时， 从而导致整个函数时间变长导致超时
-  有特殊的逻辑分支， 进入特别耗时的分支， 比如 cpu 密集型

**6. 客户端断开连接 Error:Invocation canceled by client**

函数计算是服务端，客户端主动取消了，需要将客户端超时设置的大一些，比如浏览器一般默认调用超时在1-2分钟

**7. 执行异常退出 Process exited unexpectedly before completing request  错误码499**

1. 函数本身逻辑有问题， 导致执行环境退出，需要用户增加日志调试下代码，多现于下游数据库问题
   
2. 如果是custom runtime 出现这种情况， 最有可能是因为实现的 custom runtime 的 http server 没有文档中的第3个条件： connection 最好设置为 keep alive,请求超时时间至少设置在 15 分钟以上

**8. 触发器不能正常触发函数调用**

可以按照如下顺序来检查：

- 是否满足触发规则， 比如：

> 1. 对于[oss 触发器](https://help.aliyun.com/document_detail/62922.html)
>    - OSS 和 函数必须在同一个 region
>    - 是不是上传了指定后缀的文件到指定的前缀目录， 后缀是不是不对，比如前缀目录配置的是a ，可以直往a目录上传文件，这种是可以。或者直接就是在根目录下上传a.zip，这种也是可以的。如果在/b目录下上传a.zip，这种前缀就变成了/b/a.zip ，这种就是不可以的。
>    - 控制台上传的是 PostObject 事件
>    - SDK 直接使用 PutObject  接口是 PutObject 事件
>    - OSS browser 工具默认使用的是分片上传， 上传完毕的事件是 CompleteMultipartUpload
> 2. 对于表格存储触发器， 是不是没有打开表的 stream

- 触发器的 role
  - 被删除了， 或者 role 的权限不足了，具体的[触发器invocation role权限](https://docs.serverless-devs.com/fc/yaml/triggers)
  - role 是自己创建的， 比如 oss触发器， 授信的服务不是 oss,  最好使用标准的 AliyunOSSEventNotificationRole
 
- 可以告知对应的  uid， bucket ， 触发规则， 触发的服务/函数， 事件源相应的同学需要一起排查看看
