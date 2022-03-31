---
title: 使用s工具进行克隆函数
description: '使用s工具进行克隆函数'
position: 13
category: '概述'
---


# 使用s工具进行克隆函数

### 在s工具中有一个s sync命令，我们可以通过这个命令实现快速克隆函数。[查看详情](https://help.aliyun.com/document_detail/295906.html)，也可以通过执行`s cli fc sync -h`查看帮助信息
```
Sync

  Synchronize online resources to offline resources 

Usage

  s sync <options>  
                    

Options

  -f, --force boolean      Mandatory overwrite code file                                                 
  --function-name string   Specify the alicloud fc function name.                                        
  --region string          Specify the region of alicloud.                                               
  --service-name string    Specify the alicloud fc service name.                                         
  --target-dir string      Specify storage directory(default: current dir)                               
  --trigger-name string    Specify the alicloud fc trigger name, you can set names by using multiple     
                           trigger-name option, eg: --trigger-name triggerA --trigger-name triggerB.     
  --type string            Operation type, code/config/all(default: all)                                 

Global Options

  -a, --access string   Specify key alias         
  --debug string        Output debug informations 
  -h, --help string     Help for command.         

Examples with Yaml

  $ s sync                                                        
  $ s <ProjectName> sync                                          
  $ s sync --region cn-hangzhou --service-name myService          
  $ s exec -- sync  --region cn-hangzhou --service-name myService 

Examples with CLI

  $ s cli fc sync --region cn-shanghai --service-name myService --type config 
```

## 示例

- 我们先部署一个名为sync-test的函数
![](https://img.alicdn.com/imgextra/i2/O1CN019szFCJ1UZF9UUdMdX_!!6000000002531-2-tps-1346-668.png)
- 执行s sync命令进行克隆函数
```
s cli fc sync --region cn-hangzhou --service-name sync-test --function-name sync-test -a default
```
![](https://img.alicdn.com/imgextra/i3/O1CN01nA4mwK1pfIJiLd0cJ_!!6000000005387-2-tps-2782-406.png)

- 更改functionName为sync-clone,并执行s deploy重新部署函数
![](https://img.alicdn.com/imgextra/i1/O1CN01nWRO871p3jqAZf7cq_!!6000000005305-2-tps-1392-1072.png)
![](https://img.alicdn.com/imgextra/i2/O1CN01UMSMG11GEimn1WMLj_!!6000000000591-2-tps-1388-734.png)

至此，整个克隆示例完成