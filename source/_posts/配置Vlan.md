---
title: 配置Vlan
date: 2023-06-28 16:03:37
type: Vlan
description: 配置Vlan
---

### 端口模式


ACCESS配置VLAN:
隔离广播域
入方向  打VLAN tag
出方向  去tag

TRUNK:
入方向:保留VLAN tag
出方向:保留VLAN tag

### ACCESS

```
vlan 10     #创建vlan
switchport mode access      #配置接口类型
switchport access vlan 10   #配置接口vlan
```

TRUNK

```
switchport trunk encapsulation dot1q    #封装为dot1q
switchport mode trunk   #配置接口类型
switchport trunk allowed vlan all   #允许所有vlan通过
```
