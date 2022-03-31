---
title: 快速开始
description: '快速开始'
position: 5
category: 'FC_CookBook'
---

# 快速开始

## 部署一个Hello World 事件函数


在完成[工具安装](./install.md)以及[密钥配置](./config.md)后，我们可以尝试来部署一个简单的 Serverless 应用。


首先，我们提供一系列指令来展示如何初始化一个 Hello World 函数并进行构建以及部署操作


```shell
# 初始化，并进入到项目中
$ s init start-fc-event-python3 -d start-fc-event-python3
$ cd start-fc-event-python3

# 部署函数
$ s deploy

# 调用函数
$ s invoke -e '{}'

# 删除线上Service/Function（可选）
$ s remove service
```
## ![image.png](https://img.alicdn.com/imgextra/i1/O1CN01tckuHH1RZIN3gs46y_!!6000000002125-2-tps-2356-1476.png)
这个示例工程的目录如下， s.yaml 这个文件可以看成是一个应用的声明， 这个应用里面， 只有一个服务， 这个服务使用了 fc 组件， 比如当您执行：
```bash
s fc-deploy-test deploy
# 当然直接 s deploy 也是可以的， 原因见下文的 "如果想部署两个 service 或者两个函数 咋办？"
```

- S 工具作为启动器运行起来
- S 工具发现 fc-deploy-test 使用的是 fc 组件， 加载 fc 组件 (第一次使用有下载时间)
- S 工具调用 fc 组件的 deploy 方法， 完成 fc 函数部署

​

#### 如果想部署两个函数或者两个service咋办？
```yaml
edition: 1.0.0          #  命令行YAML规范版本，遵循语义化版本（Semantic Versioning）规范
name: fcDeployApp       #  项目名称
access: "default"  #  秘钥别名

vars:
  region: cn-hangzhou
  service:
    name: fc-deploy-service
    description: 'demo for fc-deploy component'
    internetAccess: true

services:
  fc-function1: 
    component: devsapp/fc  # 组件名称
    props: #  组件的属性值
      region: ${vars.region}
      service: ${vars.service}
      function:
        name: event-py3
        description: this is a test
        runtime: python3
        codeUri: ./code
        handler: index.handler
        memorySize: 128
        timeout: 60
  
  fc-function2:
    component: devsapp/fc  # 组件名称
    props: #  组件的属性值
      region: ${vars.region}
      service: ${vars.service}
      function:
        name: event-py3-2
        description: this is a test
        runtime: python3
        codeUri: ./code2
        handler: index.handler
        memorySize: 128
        timeout: 60
```


然后直接执行  s deploy 会依次执行 fc-function1  和 fc-function2 组件中的 deploy 完成 2 个函数的部署。因此， 在您的应用的 yaml 如果定义了多个函数， 想对具体的一个函数进行 deploy 或者调用， 直接
s fc-function1 deploy 或者 s fc-function1 invoke 即可
# 部署一个 Hello World HTTP 函数 


```bash
# 初始化，并进入到项目中
$ s init start-fc-http-python3 -d start-fc-http-python3
$ cd start-fc-http-python3

# 部署函数
$ s deploy

# 调用函数
# 这里的示例是匿名的 http trigger, 
# 可以直接使用 curl 工具或者 postman 直接对生成 url(如下图) 发起 http 请求，比如
# $ curl -v http://http-trigger-py36.fc-deploy-service.123456789.cn-hangzhou.fc.devsapp.net
# 如果是 http trigger 的 authType 是 function， 则可以使用：
# $ s invoke -e '{"body":123,"method":"GET","headers":{"key":"value"},"queries":{"key":"value"},"path":"string"}'
# event 是一个json str， 如果直接在命令行中输入 json 比较复杂的话， 可以把这个json str 保存在文件 evt.json 中， 然后
# $ s invoke -f evt.json
# 比如上面例子的 evt.json 文件可以使用 s cli fc-event http  生成 event 模板
```
![image.png](https://img.alicdn.com/imgextra/i1/O1CN01hREZ9H1jX9bKM4XI5_!!6000000004557-2-tps-1403-487.png)


本文示例使用的是 python 的函数， 如果您使用的是 java 语言， 在 s deploy 之前先执行下 s build,  详情见下一章节的 “安装第三方依赖和编译”。
# 参考

- [FC 组件 yaml 规范](https://github.com/devsapp/fc/blob/main/docs/Others/yaml.md)
  > 如果网络不流畅， 可以使用备份地址 [FC 组件 yaml 规范](https://gitee.com/devsapp/fc/blob/main/docs/zh/yaml.md)
- [https://github.com/devsapp/start-fc](https://github.com/devsapp/start-fc)  更多 fc 开始示例
- [FC 企业级经典案例](https://github.com/awesome-fc/fc-faq/blob/main/docs/FC%E7%BB%8F%E5%85%B8%E6%A1%88%E4%BE%8B.md)
  > 如果网络不流畅， 可以使用备份地址 [FC 企业级经典案例](https://gitee.com/aliyunfc/fc-faq/blob/main/docs/FC%E7%BB%8F%E5%85%B8%E6%A1%88%E4%BE%8B.md)
- [https://github.com/devsapp/Application-Awesome](https://github.com/devsapp/Application-Awesome)  更多应用级别示例
