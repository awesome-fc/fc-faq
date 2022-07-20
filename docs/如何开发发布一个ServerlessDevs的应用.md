---
title: 如何开发发布一个ServerlessDevs的应用
description: '如何开发发布一个ServerlessDevs的应用'
position: 12
category: '概述'
---

## 前言

如果不想看文档， 建议直接看视频（大约15分钟）： 

- [https://example-static.oss-cn-beijing.aliyuncs.com/video/202203231823.mp4](https://example-static.oss-cn-beijing.aliyuncs.com/video/202203231823.mp4)  

> readme 中有图片的话， 建议使用这个图床： [https://tps.alibaba-inc.com/](https://tps.alibaba-inc.com/), 如果有权限的话

## 1. 初始化一个 Serverless Devs 的应用到本地
参考：

-  [应用开发说明](https://docs.serverless-devs.com/serverless-devs/package_dev#%E5%BA%94%E7%94%A8%E5%BC%80%E5%8F%91%E8%AF%B4%E6%98%8E)
- [应用模型规范](https://github.com/Serverless-Devs/Serverless-Devs/blob/publish_docs/spec/zh/0.0.2/serverless_package_model/package_model.md#%E5%BA%94%E7%94%A8%E6%A8%A1%E5%9E%8B%E8%A7%84%E8%8C%83)

## 2. 在本地这个初始化的应用完成具体的开发

### 1. 主要修改 publish.yaml， 可以参考下 [https://github.com/devsapp/start-word2pdf/blob/main/publish.yaml](https://github.com/devsapp/start-word2pdf/blob/main/publish.yaml) ， 修改成自己需要的

### 2. 控制台应用中心部署您应用需要的权限如下图定义：
![image.png](https://intranetproxy.alipay.com/skylark/lark/0/2022/png/79424/1657699070036-0ea9e836-d734-49fa-b9ee-df57511d76a5.png#clientId=ue04276ad-83fd-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=168&id=u2deaba5f&margin=%5Bobject%20Object%5D&name=image.png&originHeight=168&originWidth=771&originalType=binary&ratio=1&rotation=0&showTitle=false&size=25943&status=done&style=none&taskId=u4762ba38-24bd-4779-9668-274ca746f1f&title=&width=771)
![image.png](https://intranetproxy.alipay.com/skylark/lark/0/2022/png/79424/1657699141873-a46307e2-5666-4ec2-bf33-3f54bf9ffc73.png#clientId=ue04276ad-83fd-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=674&id=u550c99d4&margin=%5Bobject%20Object%5D&name=image.png&originHeight=674&originWidth=1344&originalType=binary&ratio=1&rotation=0&showTitle=false&size=129154&status=done&style=none&taskId=uf949bcc9-1242-4400-ae6e-f99d5469ca5&title=&width=1344)
比如这个 word2pdf 只是涉及到函数的创建，所以 publish.yaml  Service 要求的权限是 AliyunFCFullAccess,  那个应用中心会引导用户完成 AliyunFCServerlessDevsRole 的授权， 从而应用中心能使用这个 role 完成对您应用的 s deploy

### 3. publish.yaml 参数的定义和 s.yaml 是对应的， 这两个是一起配合的， 让用户填写， 最后生成具体的 s.yaml
![image.png](https://intranetproxy.alipay.com/skylark/lark/0/2022/png/79424/1657699320058-ca1b74bd-8012-41b8-9642-061f8bf55742.png#clientId=ue04276ad-83fd-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=893&id=udccc410f&margin=%5Bobject%20Object%5D&name=image.png&originHeight=893&originWidth=1362&originalType=binary&ratio=1&rotation=0&showTitle=false&size=164217&status=done&style=none&taskId=u28919dbb-eed0-492f-997f-8bd456c6475&title=&width=1362)
![image.png](https://intranetproxy.alipay.com/skylark/lark/0/2022/png/79424/1657699350679-441e8a00-20e2-4684-953e-9dfa4eaf41a0.png#clientId=ue04276ad-83fd-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=647&id=uc4edae79&margin=%5Bobject%20Object%5D&name=image.png&originHeight=647&originWidth=1004&originalType=binary&ratio=1&rotation=0&showTitle=false&size=170406&status=done&style=none&taskId=u84a1beff-722c-4075-9ee1-2b0944d2324&title=&width=1004)

比如你应用发布以后， 使用 s init 你的应用， 就会出现如下图交互式填写参数（控制台应用中心同理）：
![image.png](https://intranetproxy.alipay.com/skylark/lark/0/2022/png/79424/1657699463027-8e2d7242-9c5a-4044-aab1-6088a6891993.png#clientId=ue04276ad-83fd-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=514&id=u491e133d&margin=%5Bobject%20Object%5D&name=image.png&originHeight=514&originWidth=861&originalType=binary&ratio=1&rotation=0&showTitle=false&size=64624&status=done&style=none&taskId=u5a5b6506-9a80-4944-b459-e0fbff7f22c&title=&width=861)

### 4.  输入 s cli registry login,  完成个人 github 账户授权登录

### 5. 输入 s cli registry publish， 完成应用的发布

### 6. 测试应用
如果处于开发期间， 可以将将 publish.yaml 中的 version 改成 dev,   dev 版本是可以重复发布的， 发布成功后， 可以使用 cli 模式测试：  s init xxx@dev,  其中  xxx 是你应用的名字

等您测试 OK 后， 将 version 改成正式的 release 版本， 发布， 然后再测试下 cli 模式和应用中心部署模式

-  CLI 模式：s init 的模式， s init xxx， xxx 是你应用的名字
- 控制台应用中心： [https://fcnext.console.aliyun.com/applications/create?template=](https://fcnext.console.aliyun.com/applications/create?template=video-transcode)xxx,  xxx 是你应用的名字（最好先清空 AliyunFCServerlessDevsRole 的 policy， 测试您在publish.yaml 中定义的 Service/Auth 的权限是正确的， 能保证应用中心使用这个 role 完成这个应用的 s deploy）

部署成功后， 再看看具体效果是不是符合您的预期

- 如果模版详情在函数计算控制台应用中心显示异常， [http://www.devsapp.cn/details.html?name=alireadme](http://www.devsapp.cn/details.html?name=alireadme) 请使用这个 alireadme 来编写校验， 保证在控制台显示符合要求

- 如果应用有问题，fix 后，  直接修改 publish.yaml 中的 version 升一下， 重新 publish 即可，  s init 和 控制台应用中心默认会使用最大最新的版本

### 7.  上线到控制台应用中心的 Tab
填写这个表单就好： [https://survey.aliyun.com/apps/zhiliao/YdBrsSK0f](https://survey.aliyun.com/apps/zhiliao/YdBrsSK0f)

## 参考

- [https://github.com/Serverless-Devs/Serverless-Devs/discussions/439](https://github.com/Serverless-Devs/Serverless-Devs/discussions/439)  有视频教程

- [http://www.devsapp.cn/faq.html](http://www.devsapp.cn/faq.html)
