---
title: 本地调试
description: '本地调试'
position: 8
category: 'FC_CookBook'
---

# 本地调试

所谓的本地调试， 即在本地启动一个 docker 容器模拟函数计算线上的执行环境， 然后 mock event 参数传入调用。
​

## 事件函数
```bash
s local invoke -h
```
![image.png](https://img.alicdn.com/imgextra/i2/O1CN01pdSFvO1W060a6gzhn_!!6000000002725-2-tps-2522-1468.png)
### 快速 mock 其他事件源 event
```bash
s cli fc-event -h
```
比如，快速 mock 一个 OSS 触发器的 event：
![image.png](https://img.alicdn.com/imgextra/i2/O1CN013g26ww1OQBe5eGeqW_!!6000000001699-2-tps-1962-1084.png)
```bash
s invoke -f event-template/oss-event.json
```
## HTTP 函数
```bash
s local start -h
```
### 没有自定义域名
![image.png](https://img.alicdn.com/imgextra/i4/O1CN01wAUrzu1vGuwvP5qbH_!!6000000006146-2-tps-2754-1050.png)

### 有自定义域名
![image.png](https://img.alicdn.com/imgextra/i4/O1CN01G0rBKd1jfOjP9VOKo_!!6000000004575-2-tps-2562-1680.png)


## 总结
本地调试虽然很方便快捷， 但是在实际场景中：

- 有不少函数是需要访问线上 vpc 的内网地址或者挂载了 NAS， 这个时候本地是没有办法 mock， 比如只有 vpc 内网地址的数据库， kafka 等
- 表格存储触发器， 这种二进制的 event 很难 mock


[本地调试详情](https://www.serverless-devs.com/fc/command/local)

因此， 端云联调作为 serverless 调试的终态方案来解决本地开发环境和线上环境联调打通。
