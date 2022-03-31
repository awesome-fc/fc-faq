---
title: service_role简介
description: 'No permission to assume the role'
position: 8
category: '概述'
---


## 简介

比如用户执行部署函数的时候，如 `s deploy` 出现了如下错误：

```bash
Message:  POST /services failed with 403. requestid: 133be885-1682-4590-9fcf-6dbb3111cb1d, message: No permission to assume the role 'acs:ram::123456789:role/xx-test' for you
```

原因是您配置的 service role 的授信体不是 FC 这个服务， 比如 `acs:ram::123456789:role/xx-test`

```json
{
  "Statement": [
    {
      "Action": "sts:AssumeRole",
      "Effect": "Allow",
      "Principal": {
        "RAM": ["acs:ram::987654321:root"]
      }
    }
  ],
  "Version": "1"
}
```

授信的是 987654321 这个云账号， 真正需要授信的是 fc

```json
{
  "Statement": [
    {
      "Action": "sts:AssumeRole",
      "Effect": "Allow",
      "Principal": {
        "Service": ["fc.aliyuncs.com"]
      }
    }
  ],
  "Version": "1"
}
```

![](https://img.alicdn.com/imgextra/i3/O1CN01iUbhgU1tSFUTYMYNe_!!6000000005900-2-tps-2470-802.png)
