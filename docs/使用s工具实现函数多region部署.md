---
title: S工具多region部署
description: 'S工具多region部署'
position: 14
category: '概述'
---

# S工具多region部署

### shell 脚本

```bash
#! /bin/bash
# deploy pre
regions=("cn-hangzhou" "ap-southeast-1")
for r in ${regions[@]}
do
  export REGION=$r
  export LOG_PROJECT="fc-function-$r"
  s deploy -y --use-local
done
```

### s.yaml 示例

```yaml
edition: 1.0.0
name: compoent-test
access: 'default'

vars:
  region: ${env(REGION)}
  service: # service 配置原则上不能修改， 如果有改动，请和 fengchong/xiliu 对齐下
    name: test-service
    internetAccess: true
    logConfig:
      enableInstanceMetrics: true
      enableRequestMetrics: true
      logBeginRule: DefaultRegex
      project: ${env(LOG_PROJECT)}
      logstore: test-logstore

services:
  test:
    component: devsapp/fc  # 组件名称
    props: #  组件的属性值
      region: ${vars.region}
      service: ${vars.service}
      function:
        name: test
        handler: index.handler
        memorySize: 512
        runtime: nodejs12
        timeout: 60
        codeUri: './code'
```
