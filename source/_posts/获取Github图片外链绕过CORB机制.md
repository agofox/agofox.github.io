---
title: 获取Github图片外链绕过CORB机制
date: 2023-06-27 18:31:59
tags: Github
---

- [Cross-Origin Read Blocking (CORB)](#cross-origin-read-blocking-corb)
- [GitHub解决跨源响应的方法](#github解决跨源响应的方法)
- [图片外链地址规则:](#图片外链地址规则)

### Cross-Origin Read Blocking (CORB)
是一种安全机制，用于阻止跨源请求中潜在恶意的数据泄露。当你尝试读取一个跨源的响应，CORB 阻止了该操作。

### GitHub解决跨源响应的方法
GitHub提供了一种解决跨源响应的CORB问题的方法。你可以通过使用GitHub的原始文件URL来绕过CORB机制，以下是具体步骤：

1.  打开你想要读取的文件的GitHub页面。
2.  在页面上找到并点击文件的"Raw"按钮，它通常位于文件上方或右上角。
3.  这将打开原始文件的URL，通常以https://raw.githubusercontent.com/开头。复制该URL。
4. 在你的代码或应用程序中，使用这个原始文件URL作为跨源请求的目标URL。确保使用这个URL进行请求，而不是使用GitHub页面上的URL。

 通过使用原始文件URL，你绕过了CORB机制，因为GitHub在提供原始文件时会设置适当的CORS头部，允许跨源访问。

请注意，使用GitHub的原始文件URL解决CORB问题仅适用于GitHub上的公共存储库文件。对于私有存储库或其他非GitHub资源，你可能需要考虑其他解决方案，如CORS设置或代理服务器。

### 图片外链地址规则:
1.  正常情况Github图片URL：
  https://github.com/agofox/agofox.github.io/blob/main/img-folder/git/1.jpg
2.  原始文件URL:
  https://raw.githubusercontent.com/agofox/agofox.github.io/main/img-folder/git/1.jpg

  规则：
  ①把“1”的“github.com”替换为“raw.githubusercontent.com”；②再把“1”的“/blob”去掉.