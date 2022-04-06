---
title: 安装第三方依赖和编译
description: '安装第三方依赖和编译'
position: 6
category: 'FC_CookBook'
---


# 安装第三方依赖和编译
## 简介


在实际场景中， 部署在 FC 的业务函数大都需要依赖第三方库就构建完成， 因此， 有第三方包依赖的函数怎么快速构建部署到函数计算平台是一个重要的事宜。阿里云函数计算（FC）组件为使用者提供了FC相关资源的构建/安装依赖的能力。可以通过`build`指令，快速进行构建/安装依赖操作。

指令详情参考 [build](https://www.serverless-devs.com/fc/command/build)


- 对于解释型语言， 如 nodejs、python、php,  build 操作完成的事情是安装第三方依赖
- 对于编译型语言， 如 java,  build 就是编译
- 对于 custom container,  build 就是从 Dockerfile 编译成 image



您可以通过`build -h`/`build --help`参数，唤起帮助信息。


# 快速使用


当我们下载好[Serverless Devs开发者工具](../../Getting-started/Install-tutorial.md), 并完成[阿里云密钥配置](../../Getting-started/Setting-up-credentials.md)之后，我们可以根据自身的需求进行资源的快速构建。


Python语言，Java语言、Nodejs语言、Php语言，以及容器镜像等都可以支持。以下例举可以识别的依赖文件名（即通过识别该文件，进行相关依赖下载或资源构建）：


- Python: requirements.txt
- Nodejs: package.json
- Php: composer.json
- Java: pom.xml
- Container: dockerfile

> ⚠️ 注意：在部分语言完成项目构建之后，部署的时候可能会出现交互式操作，提醒用户是否要将安装的依赖路径加入到环境变量中，以便线上可以正确的加载到这些依赖内容。此时可以通过交互式的方法，根据提醒输入`y`，也可以在部署时通过`-y`命令，默认进行环境变量等内容的添加。

以 Python 应用为例：在具有 `requirements.txt` 的 Python 项目下，可以通过`s build --use-docker`命令实现依赖安装：

![](https://img.alicdn.com/imgextra/i3/O1CN016yUmJP1aKU4boPjWo_!!6000000003311-2-tps-1667-978.png)

如上图所示：

1. 开发编辑源代码；

2. `s build --use-docker`之后， 自动根据 `requirements.txt` 下载对应的依赖到本地， 并且和源码一起组成交付物；

3. `s deploy` 将整个交付物 zip 打包， 创建函数， 同时设置好依赖包的环境变量， 让函数可以直接 `import` 对应的代码依赖包；

> **Node.js 项目**、**PHP 项目**与 Python 项目类似，都是在开发代码之后，可以通过`s build --use-docker`进行依赖安装，此时工具将会自动根据相关依赖文件（例如Node.js是 `package.json` ，PHP是`composer.json` ）下载对应的依赖到本地， 并且和源码一起组成交付物；接下来可以通过`s deploy`进行项目部署，此时工具会将整个交付物 ZIP 打包， 创建函数， 同时设置好依赖包的环境变量， 让函数可以直接 `require` 对应的代码依赖包

> **Java**是在开发代码之后，可以通过`s build --use-docker`进行 Java 工程的构建：
>
> ![](https://img.alicdn.com/imgextra/i4/O1CN014gwk4d1PZdOnL9gWC_!!6000000001855-2-tps-1304-622.png)
>
> 接下来可以通过`s deploy`进行项目部署，此时的交付物是 Jar 包。

> **Custom Container**，则是需要先[开通 ACR/CR 容器镜像服务](https://cr.console.aliyun.com/)，然后在`s.yaml`的`image`字段处填写好`acr`镜像地址，通过`s build --use-docker --dockerfile ./Dockerfile`进行项目构建；接下来可以通过`s deploy --push-registry acr-internet -y`将项目部署到线上，此时工具会先将构建完成的镜像推送到 ACR 服务，然后再进行函数的创建。

> 💡 在使用`build`命令时，可以通过环境变量 `FC_DOCKER_VERSION` 控制镜像的版本，例如 export FC_DOCKER_VERSION=latest（所有可用版本可查看 https://github.com/aliyun/fc-docker 或者 https://hub.docker.com/u/aliyunfc ）

> 💡 在代码包的场景中， 除了各自语言的库以外， 其实还有更加复杂的情况，例如，在函数计算的 Node.js Runtime 上部署 puppeteer 应用， puppeteer 库还需要安装底层的 so 库， 此时还需要 [apt-get.list](https://github.com/devsapp/start-puppeteer/blob/master/src/nodejs12/src/apt-get.list) 的支持,  具体如下图所示：
>
> ![](https://img.alicdn.com/imgextra/i2/O1CN01IOxwXQ1EiNBT7jFtJ_!!6000000000385-2-tps-1684-964.png)
>
> 感兴趣的可以参考 [fc-start-puppeteer](https://github.com/devsapp/start-puppeteer/tree/master/src)  中 Deploy using Nodejs 12 with NAS 章节。