---
title: Linux服务器安全设置
date: 2023-05-01 01:05:31
tags: Linux
description: 简单提高服务器安全系数的方法
---
- [使用强密码](#使用强密码)
- [修改 ssh 登陆配置](#修改-ssh-登陆配置)
- [禁止系统响应任何从外部 / 内部来的 ping 请求](#禁止系统响应任何从外部--内部来的-ping-请求)
- [用户管理](#用户管理)


### 使用强密码
pwgen 命令在 linux 下生成一个随机的强壮密码

pwgen 程序生成易于人类记忆并且尽可能安全的密码。

易于人类记忆的密码永远都不会像完全随机的密码一样安全。

使用 -s 选项来生成完全随机，难于记忆的密码。由于我们记不住，这些密码应该只用于机器。

在 Debian/Ubuntu 系统中，使用 APT-GET 命令 或 APT 命令 来安装 pwgen。
```bash
$ sudo apt install pwgen
```
假设你想要生成 3 个 14 字符长的密码，方法如下：
```bash
$ pwgen -s 14 3
7YxUwDyfxGVTYD em2NT6FceXjPfT u8jlrljbrclcTi 
```
如果你真的想要生成 5 个超强随机密码，方法如下：
```bash
$ pwgen -cnys 14 3 
#-n密码中至少包含一个数字 
#-y密码中至少包含一个特殊符号 
#-c在列中打印生成的密码
l~]JD_,W%5bp.E  +i2=D3;BQ}p+$I  n.a3,.D3VQ3~&i
```
### 修改 ssh 登陆配置

打开 ssh 配置文件

```bash
vim /etc/ssh/sshd_config

#修改以下几项
Port 10000
#更改SSH端口，最好改为10000以上，别人扫描到端口的机率也会下降。防火墙要开放配置好的端口号，如果是阿里云服务器，你还需要去阿里云后台配置开发相应的端口才可以，否则登不上哦！如果你觉得麻烦，可以不用改
 
Protocol 2
#禁用版本1协议, 因为其设计缺陷, 很容易使密码被黑掉。
 
PermitRootLogin no
#尝试任何情况先都不允许 Root 登录. 生效后我们就不能直接以root的方式登录了，我们需要用一个普通的帐号来登录，然后用su来切换到root帐号，注意 su和su - 是有一点小小区别的。关键在于环境变量的不同，su -的环境变量更全面。
 
PermitEmptyPasswords no
＃禁止空密码登陆。
```

最后需要重启 sshd 服务
```bash
service sshd restart
```

### 禁止系统响应任何从外部 / 内部来的 ping 请求
```bash
echo "1"> /proc/sys/net/ipv4/icmp_echo_ignore_all
```
默认值为 0

### 用户管理


下面是基本的用户管理命令
```bash
查看用户列表：cat /etc/passwd
查看组列表：cat /etc/group
查看当前登陆用户：who
查看用户登陆历史记录：last
```

一般需要删除系统默认的不必要的用户和组，避免被别人用来爆破：
```bash
userdel sync
userdel shutdown
# 需要删除的多余用户共有：sync shutdown halt uucp operator games gopher
groupdel adm
groupdel games
# 需要删除的多余用户组共有：adm lp games dip
```

Linux 中的帐号和口令是依据 /etc/passwd 、/etc/shadow、 /etc/group 、/etc/gshadow 这四个文档的，所以需要更改其权限提高安全性：
```bash
chattr +i /etc/passwd
chattr +i /etc/shadow
chattr +i /etc/group
chattr +i /etc/gshadow
```

如果还原，把 +i 改成 -i , 再执行一下上面四条命令。

注：i 属性：不允许对这个文件进行修改，删除或重命名，设定连结也无法写入或新增数据！只有 root 才能设定这个属性。