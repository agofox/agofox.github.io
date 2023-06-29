---
title: window环境hexo+github部署
date: 2023-06-26 01:04:03
tags: Hexo
---

- [前期环境准备：](#前期环境准备)
- [安装Hexo：](#安装Hexo)
- [在GitHub Pages上部署 Hexo](#在-github-pages-上部署-hexo)


### 前期环境准备

安装Git
安装包下载地址：https://git-scm.com/
Git配置及使用：https://agofox.github.io/2023/06/27/Git%E9%85%8D%E7%BD%AE%E5%8F%8A%E4%BD%BF%E7%94%A8/

安装node
安装包下载地址：https://nodejs.org/en
(Node.js 版本需不低於8.10，建議使用 Node.js 10.0 及以上版本)

### 安装Hexo
官方安装文档：https://hexo.io/zh-cn/docs/
```bash
$ npm install hexo-cli -g
$ hexo init blog
$ cd blog
$ npm install
$ hexo server
```
### 在 GitHub Pages 上部署 Hexo

1.  建立名为 <你的 GitHub 用户名>.github.io 的储存库，若之前已将 Hexo 上传至其他储存库，将该储存库重命名即可。

2.  编辑你的 _config.yml，将 url: 更改为 <你的 GitHub 用户名>.github.io/<repository 的名字>。
如：
-  
    ```yml
    url:  https://github.com/agofox/agofox.github.io
    ```

3.  将 Hexo 文件夹中的文件 push 到储存库的默认分支，默认分支通常名为 main，旧一点的储存库可能名为 master。

    将 main 分支 push 到 GitHub：
-   
    ```bash
    $ git push -u origin main
    ```


4.  使用 node --version 指令检查你电脑上的 Node.js 版本，并记下该版本 (例如：v18.y.z)

5.  在储存库中建立 .github/workflows/pages.yml，并填入以下内容 (将 18 替换为上个步骤中记下的版本)：
-
    ```yml
    .github/workflows/pages.yml
    name: Pages

    on:
      push:
        branches:
          - main # default branch

    jobs:
      pages:
        runs-on: ubuntu-latest
        permissions:
          contents: write
        steps:
          - uses: actions/checkout@v2
          - name: Use Node.js 18.x
            uses: actions/setup-node@v2
            with:
              node-version: "18"
          - name: Cache NPM dependencies
            uses: actions/cache@v2
            with:
              path: node_modules
              key: ${{ runner.OS }}-npm-cache
              restore-keys: |
                ${{ runner.OS }}-npm-cache
          - name: Install Dependencies
            run: npm install
          - name: Build
            run: npm run build
          - name: Deploy
            uses: peaceiris/actions-gh-pages@v3
            with:
              github_token: ${{ secrets.    GITHUB_TOKEN }}
              publish_dir: ./public
    ```          
6.  当部署作业完成后，产生的页面会放在储存库中的 gh-pages 分支。

7.  在储存库中前往 Settings > Pages > Source，并将 branch 改为 gh-pages。

8.  前往 https://<你的 GitHub 用户名>.github.io 查看网站。