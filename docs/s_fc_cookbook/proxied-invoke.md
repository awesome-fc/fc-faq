## 前言&背景

在 Serverless/FaaS 领域中， 调试成为了一个最突出的痛点， 虽然云厂商也提供了一些开发工具， 但都是集中在本地模拟调试+mock 参数阶段， 会存在如下痛点问题：

1. 开发的函数需要访问只有 vpc 内网地址的数据库或者 kafka 等
2. 函数计算挂载NAS 功能， 目录只能 mock
3. 开发的函数需要从 oss 下载上传， 线上函数可以直接使用 internal endpoint,   本地调试只能临时改成 public endpoint,  比如大点音视频， 除了函数执行时间长， 还有OSS公网流量费用
4. 比如触发函数的 event 是二进制 cbor 格式的json,  基本很难 mock
5. ...

简单总结下来就是：执行环境出的流量不能触达指定 vpc 或者内网 endpoint 服务， 入的流量（函数的入参 context 和 event） 不够真实。

## 端云联调

[端云联调文档](https://help.aliyun.com/document_detail/195642.html)