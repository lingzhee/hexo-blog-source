---
title: eureka
date: 2021-04-08 16:08:00
tags: sping
---

## 单节点搭建

```yaml
server:
  port: 8761

eureka:
  instance:
    hostname: localhost
  client:
    registerWithEureka: false
    fetchRegistry: false
    serviceUrl:
      defaultZone: http://${eureka.instance.hostname}:${server.port}/eureka/
```

# eureka生产环境优化

```yaml
#自我保护
enable-self-preservation: false
#自我保护阈值
renewal-percent-threshold: 0.85
#剔除服务时间间隔
eviction-interval-timer-in-ms: 1000

#关闭从readonly读取注册表
use-read-only-response-cache: false
#readWrite 和 readOnly 同步时间间隔
response-cache-update-interval-ms: 1000



```