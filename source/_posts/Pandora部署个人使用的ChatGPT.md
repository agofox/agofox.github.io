---
title: 使用Pandora部署个人使用的ChatGPT
date: 2023-06-28 00:43:33
tags: ChatGPT
---

- [使用Pandora部署个人使用的ChatGPT](#使用pandora部署个人使用的chatgpt)
    - [购买VPS服务器](#购买vps服务器)
    - [部署在VPS上(Debian系统)](#部署在vps上debian系统)
      - [下载Python源码包](#下载python源码包)
      - [源码安装Python](#源码安装python)
    - [安装Pandora](#安装pandora)

#   使用Pandora部署个人使用的ChatGPT
Pandora以其部署chatgpt简单，抗干扰能力强在过去一段时间一直表现不错.
作者项目地址：https://github.com/pengzhile/pandora

### 购买VPS服务器
1. 首先你需要购买一台没被OpenAI封锁IP的VPS,使用OpenAI-Checker检测IP是否可以访问OpenAI:

```bash
bash <(curl -Ls https://cdn.jsdelivr.net/gh/missuo/OpenAI-Checker/openai.sh)
```
2.  运行后显示：Your IP supports access to OpenAI. 即表示该VPS可以使用OpenAI

作者Github项目地址：https://github.com/missuo/OpenAI-Checker

### 部署在VPS上(Debian系统)

#### 下载Python源码包
   python下载地址：https://www.python.org/downloads/source/

   ```bash
   sudo apt update 
   curl -O  https://www.python.org/ftp/python/3.11.4/Python-3.11.4.tgz 
   ```
####  源码安装Python

1.  解压并进入解压文件夹

    ```bash
    tar zxvf Python-3.11.4.tgz  
    cd Python-3.11.4 
    ```
2.  安装make及gcc编译器
    ```bash 
    sudo apt install -y make gcc  
    ```

3.  执行configure，生成Makefile文件并将软件进行安装
    ```bash
    ./configure  
    make && make install 
    ```

4.  查看python版本，检查是否成功安装
    ```bash
    python3 -V
    ```
### 安装Pandora
pip安装运行

```bash
pip install pandora-chatgpt
pandora
```
如果你想支持gpt-3.5-turbo模式：在[api]中填写OpenAI获取到的API
```bash
pip install 'pandora-chatgpt[api]'
```
程序参数
```html
可通过 pandora --help 查看。
-p 或 --proxy 指定代理，格式：protocol://user:pass@ip:port。
-t 或 --token_file 指定一个存放Access Token的文件，使用Access Token登录。
-s 或 --server 以http服务方式启动，格式：ip:port。
-a 或 --api 使用gpt-3.5-turboAPI请求，你可能需要向OpenAI支付费用。
--tokens_file 指定一个存放多Access Token的文件，内容为{"key": "token"}的形式。
--threads 指定服务启动的线程数，默认为 8，Cloud模式为 4。
--sentry 启用sentry框架来发送错误报告供作者查错，敏感信息不会被发送。
-v 或 --verbose 显示调试信息，且出错时打印异常堆栈信息，供查错使用
```

例如使用Token及IP地址或域名访问服务器部署的ChatGPT
Token获取地址：https://ai-20230626.fakeopen.com/auth

创建token文件并将获取到的token写入文件中，使用IP地址进行访问（也可将IP地址换成域名进行访问）。

使用域名访问需要先将服务器IP进行解析。
```bash
vi /var/token.txt
pandora -t /var/token.txt -s 192.168.1.1:4000
```
![图片显示需要能访问Github](https://raw.githubusercontent.com/agofox/agofox.github.io/main/img-folder/pandora/1.png)

将pandora放至后台不间断运行并将错误日志写入pandora.log

```bash 
nohup pandora -t /var/token.txt -s 192.168.1.1:4000 >pandora.log 2>&1 $
```

