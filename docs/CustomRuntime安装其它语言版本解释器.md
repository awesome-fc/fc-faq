---
title: CustomRuntime安装其它语言版本解释器
description: 'CustomRuntime安装其它语言版本解释器'
position: 4
category: '概述'
---
# CustomRuntime安装其它语言版本解释器


云厂商原生的运行环境一般更新迭代都比较慢，当程序运行需要的版本不符合需求时，那么就需要用户自定义运行环境了。对于函数计算来说，有两种方式：
​


1. Custom Container：系统完全有由用户自己控制，代码包限制比较小；但是冷启动会比较严重。
1. Custom：和 Custom Container 相比，冷启动有非常大的优化，和其他的原生的运行环境基本一致，自身也[内置](https://help.aliyun.com/document_detail/132044.html#title-wi6-beu-a0k)了一些执行环境；安装执行环境相对复杂，对 window 用户有点不太友好，代码包本身就小还会占用一部分的代码空间。（本文主要讲述 Custom）

​

## Custom 环境安装


## 原理说明


原理很简单，就是将第三方解释器存在一个位置，然后再启动前修改环境变量的寻址路径，从而达到自定义环境的需求。所以解释器也并不局限于存在代码目录中，也可以存在 nas 中，从而为代码包减负，如果后面 custom 支持了层，也可以存放在层中。


### Node 示例


1. 在 Node 官网下载Linux的安装包（ [node-v16.13.0-linux-x64.tar.xz下载链接](https://nodejs.org/dist/v16.13.0/node-v16.13.0-linux-x64.tar.xz) ）
1. 解压文件，将 bin/node 文件拷贝到代码目录的 bin 目录下

![image.png](https://img.alicdn.com/imgextra/i2/O1CN014wDV2H1JKclHn1koH_!!6000000001010-2-tps-1410-526.png)

3. 代码目录

- 3.1 创建 bootstrap 文件
```javascript
#!/bin/bash

./bin/node index.js
```

- 3.2 创建 index.js 文件
```javascript
const http = require("http");

const { execSync } = require('child_process');

const requestListener = function (req, res) {
  res.writeHead(200);
  res.end(`${process.env.PATH} \n ${__dirname} \n ${execSync('node -v')}`);
};

const server = http.createServer(requestListener);
server.listen(9000);
```

4. 目录结构

![image.png](https://img.alicdn.com/imgextra/i2/O1CN01WKbeTK1P7EsX0fdsJ_!!6000000001793-2-tps-2142-1536.png)

5. 修改 bootstrap 和 node 的可执行权限
```javascript
chmod 755 ./bootstrap
chmod 755 ./bin/node
```

6. 压缩并到上传函数计算一个 event 函数
6. 调用函数，最后会输出执行的 PATH 环境变量、当前目录和 node 的版本

![image.png](https://img.alicdn.com/imgextra/i2/O1CN01QIjHa01kPCLBZscDl_!!6000000004675-2-tps-2442-312.png)

8. 修改函数的环境变量，新增一个 PATH 规则是：${当前目录}/bin:/${PATH 环境变量} (值 `/code/bin:/usr/local/bin/apache-maven/bin:/usr/local/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/local/ruby/bin`)。这时再调试就会发现 执行结果 node 版本为 v16.13.0

![image.png](https://img.alicdn.com/imgextra/i3/O1CN0189MOZF1LQhF9ZNlSN_!!6000000001294-2-tps-2726-312.png)


### 其他

如果使用 s 工具， 可以参考： https://github.com/devsapp/start-fc/tree/master/custom-function/nodejs12/fc-custom-nodejs12-event/src



