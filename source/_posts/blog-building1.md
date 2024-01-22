---
abbrlink: 个人博客搭建
categories:
- - 博客搭建
cover: https://s2.loli.net/2024/01/19/FfEG5xSNcvHqL86.png
date: '2024-01-19T13:21:38.939361+08:00'
tags:
- Github
- Github Pages
- 博客
- 零成本
- Hexo
title: 使用Github Pages + Hexo零成本搭建个人博客
updated: '2024-01-20T11:46:46.136+08:00'
---
## 准备工作

为了搭建自己的个人博客，你需要首先做好以下准备工作：

- 一个GitHub账号
- 一台安装了[Node.js](https://nodejs.org/en)(建议使用Node.js 12.0及以上版本)与[Git](https://git-scm.com/)的电脑

## 创建Github仓库

1. 访问[https://github.com/](https://github.com/)并登陆，再右上角找到 `Create New...`->`New repository`

![https://s2.loli.net/2024/01/19/OKkcwtP2eyfLYFZ.png](https://s2.loli.net/2024/01/19/OKkcwtP2eyfLYFZ.png)

2. 在 `ReRepository name *`一栏填写 `<你的 GitHub 用户名>.github.io`，然后点击 `Create repository`

![https://s2.loli.net/2024/01/19/ilakWz5ZsK16HMS.png](https://s2.loli.net/2024/01/19/ilakWz5ZsK16HMS.png)

3. 点击 `Settings`->`Pages`，在 `Build and deployment`的 `Source`一栏选择 `GitHub Actions`

![https://s2.loli.net/2024/01/19/9jxErkOLpCdgbal.png](https://s2.loli.net/2024/01/19/9jxErkOLpCdgbal.png)

4. 点击 `Code`，点击复制按钮复制你的仓库地址以备下面使用

![https://s2.loli.net/2024/01/19/F29yB6Dt37aPcCG.png](https://s2.loli.net/2024/01/19/F29yB6Dt37aPcCG.png)

## 安装Hexo

1. 在你的电脑上新建一个文件夹，这个文件夹后续将用来存放你的博客项目文件。在资源管理器打开该文件夹，按住 `Shift`并在空白处右击，选择在此处打开终端（cmd、powershell或其他shell工具均可）
2. 依次键入以下命令

```bash
npm install hexo-cli -g

hexo init

npm install

hexo server
```

3. 执行完上述命令后，打开你的浏览器访问[localhost:4000](http://localhost:4000)，如果看到以下页面，那么你的Hexo就安装成功了

![https://s2.loli.net/2024/01/19/fYETKtgGHZFbdAj.png](https://s2.loli.net/2024/01/19/fYETKtgGHZFbdAj.png)

4. 为了继续完成以下步骤，请在终端页面根据提示按 `Ctrl`+`C`来退出本地预览

![https://s2.loli.net/2024/01/19/ricJIzToW5Q8vpB.png](https://s2.loli.net/2024/01/19/ricJIzToW5Q8vpB.png)

## 部署到Github

1. 在上一步的终端界面依次键入 `node --version` 指令检查你电脑上的 Node.js 版本，并记下该版本 (例如：`v16.y.z`)
2. 在博客文件夹的 `.github`文件夹下建立 `workflows`文件夹，并在该文件夹内创建文本文件，打开该文件填入以下内容，并将其中的两个 `<替换>`替换为上个步骤中记下的版本，保存后，重命名该文件为 `pages.yml`(请注意打开显示文件扩展名，并正确地将扩展名更改为 `.yml`)

```yaml
name: Pages

on:
  push:
    branches:
      - main # default branch

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
        with:
          token: ${{ secrets.GITHUB_TOKEN }}
          # If your repository depends on submodule, please see: https://github.com/actions/checkout
          submodules: recursive
      - name: Use Node.js <替换>.x
        uses: actions/setup-node@v2
        with:
          node-version: '<替换>'
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
      - name: Upload Pages artifact
        uses: actions/upload-pages-artifact@v2
        with:
          path: ./public
  deploy:
    needs: build
    permissions:
      pages: write
      id-token: write
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    steps:
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v2
```

3. 在第一步的终端界面依次键入以下命令

```bash
git init

git add .

git commit -m "first commit"

git branch -M main

git remote add origin <上面复制的仓库地址>

git push -u origin main
```

4. 静待片刻，打开你的浏览器，访问 `<你的 GitHub 用户名>.github.io`就可以看到你的页面啦！

## 总结

这篇文章中，我们完成了以下工作：

- 创建Github仓库
- 创建了博客项目
- 部署我们的博客项目到GitHub Pages

至此，我们可以通过GitHub的二级域名来访问我们的博客了。下一篇我们来聊聊如何美化及使用我们的博客
