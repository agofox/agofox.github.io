---
title: ubuntu安装网易云音乐
date: 2023-08-23 18:21:39
tags: ubuntu
---

### 网易云音乐ubuntu安装包

官方已经把 linux 版本的下载地址删除了。估计是用的人少吧。
安装包地址：https://d1.music.126.net/dmusic/netease-cloud-music_1.2.1_amd64_ubuntu_20190428.deb



### ubuntu22.04以上安装网易云音乐报错：

22版本ubuntu升级了很多库文件，导致netease的库无法使用。netease的库中，使用的是老版本的libselinux，老版本的libselinx需要老版本的libgio，但是，22版本的libgio是新的，所以，需要重新复制新版本的libselinx等库到netease的库中。

命令行运行命令：
```
ubuntu@ubuntu-Desktop:~$ netease-cloud-music 
/opt/netease/netease-cloud-music/netease-cloud-music: /opt/netease/netease-cloud-music/libs/libselinux.so.1: no version information available 
```


报错内容如下：


/opt/netease/netease-cloud-music/netease-cloud-music: /opt/netease/netease-cloud-music/libs/libselinux.so.1: no version information available (required by /lib/x86_64-linux-gnu/libgio-2.0.so.0) /opt/netease/netease-cloud-music/netease-cloud-music: symbol lookup error: /lib/x86_64-linux-gnu/libgio-2.0.so.0: undefined symbol: g_module_open_full
这里可以看到，netease库文件目录为opt/netease/netease-cloud-music/netease-cloud-music/libs，库中libselinx版本明显与系统的/lib/x86_64-linux-gnu/中的ibgio-2.0.so.0对不上，所以，将环境中的libselinx库复制到netease库中。 

执行命令：

```
sudo cp /lib/x86_64-linux-gnu/libselinux.so.1 /opt/netease/netease-cloud-music/libs/
```

同理，继续往下排查，后续的排查过程与上述一致，故省略了。

执行以下命令可以解决该问题：

```
sudo cp /lib/x86_64-linux-gnu/libslang.so.2 /opt/netease/netease-cloud-music/libs/
sudo cp /lib/x86_64-linux-gnu/libgmodule-2.0.so.0 /opt/netease/netease-cloud-music/libs/
sudo cp /lib/x86_64-linux-gnu/libpango-1.0.so.0 /opt/netease/netease-cloud-music/libs/
sudo cp /lib/x86_64-linux-gnu/libfribidi.so.0 /opt/netease/netease-cloud-music/libs/
sudo apt-get install  libcanberra-gtk-module
```
   


