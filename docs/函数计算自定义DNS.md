# 简介

假如是如下场景，您的函数 service 配置了 vpc config， 然后您在 Private Zone 添加了一个域名解析， 假设 test.abc.com 解析到该 vpc 的一个内网地址， 如果函数访问域名出现无法解析的情况，

1. 如果您的函数没有访问公网的需求， 直接将您 service 配置中的公网访问关闭即可。

2. 如果您的函数同时还有访问公网的需求， 可以通过修改 /etc/hosts 实现

- 如果是 custom-container runtime， 直接在 Dockerfile 中修改 /etc/hosts

- 如果是其他 runtime, 可以考虑使用这个方案 [如何在函数计算内部中自定义 DNS 解析](https://developer.aliyun.com/article/680746), 目前可以做到 tcp 层面上的解析， 比如 curl 是 OK 的， 但是 ping 域名此方案暂时不生效
