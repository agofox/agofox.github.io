---
title: 动态路由OSPF
date: 2023-07-05 09:06:31
tags: OSPF
---

- [配置OSPF](#配置ospf)
- [管理距离AD值](#管理距离ad值)
- [度量值Metric](#度量值metric)
- [OSPF报文类型](#ospf报文类型)
- [三张表](#三张表)
- [建立邻居的过程](#建立邻居的过程)

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
show ip ospf neighbor
```


### 管理距离AD值
```
CISCO: 
       STATIC   0
       OSPF     110
       静态优于OSPF

HW:    
       STATIC   60
       OSPF     10
       OSPF优于静态
```
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

### 三张表

```
邻居表          建立邻居如何
                sh ip ospf neighbor

链路状态数据库   所有有关OSPF传递过来的信息
                sh ip ospf database

ospf路由表      使能(最优的条目)的路由加入表项
                sh ip route ospf
```

### 建立邻居的过程

7种状态：
1.      down:  没有宣告 接口down
2.      init:  
            
        互发hello 
        router-id      标识设备在ospf中的名称
                       选举  1.手动     router ospf 1 
                                        router-id 2.2.2.2
                             2.逻辑接口IP地址最大的
                             3.物理接口IP地址最大的
3.      2-way:   
              选举DR： 1.DR优先级，越大越优 范围0-255 默认为1，为0时不选举
                       2.router-id 越大越优
                       3.  int f0/0   ip ospf priority 0
                     
              DR选举很慢:     10-40s
                              int f0/0
                              ip ospf network point-to-point
4.      exstart
5.      exchange
6.      loading
7.      full
                               
重置进程   clear ip ospf processy       