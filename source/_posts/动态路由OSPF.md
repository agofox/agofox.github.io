---
title: 动态路由OSPF
date: 2023-07-05 09:06:31
tags: OSPF
---

### 配置OSPF

HW:
```bash
sys
ospf 1  #进程1
area 0  #区域0一般为骨干
network 172.16.12.0  0.0.0.255  #宣告网段
dis ospf peer brief #查看邻居信息
```

CISCO:
```
conf t
router ospf 1
network 172.16.12.0 0.0.0.255 area 0
show ip ospf nei 
```


### 管理距离AD值

CISCO: STATIC   0
       OSPF     110
       静态优于OSPF

HW:    STATIC   60
       OSPF     10
       OSPF优于静态

### 度量值Metric 

```
                100/带宽(m)
                S=1.544m    cost=64
                E=10m       cost=10
ospf    cost    F=100m      cost=1
                G=1000m     cost=1
                TEN G=10000m    cost=1
```

修改OSPF默认cost带宽:
```
bandwidth-reference 1000    1000/带宽(m)
```
### OSPF报文类型
```
Hello   建立和维护邻居关系
DBD     链路状态数据库描述信息
LSR     链路状态请求，向OSPF邻居请求链路状态信息
LSU     链路状态更新(包含一条或多条LSA)
LSAck   对LSU中的LSA进行确认

hello:  建立邻居    同区域同网段
        维护邻居    周期性发送  10s发送一次 40s超时

dbd:    数据库描述信息

lsr:    链路状态请求包
        dbd中对应的路由信息

lsu:    链路状态更新包
        发送路由信息    LSA-LSDB-SPF-路由表

lsack:  链路状态确认包
        确认对方发送的lsu信息
```