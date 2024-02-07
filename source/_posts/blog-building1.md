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
updated: '2024-01-23T10:34:34.910+08:00'
---
## 前言

闲来无事用Github Pages + Hexo捣鼓了一个个人博客，分享一下搭建过程。

### GitHub Pages 是什么？

[GitHub Pages](https://docs.github.com/en/pages/getting-started-with-github-pages/about-github-pages) 是由 GitHub 官方提供的免费的静态站点托管服务，让我们可以在 GitHub 仓库里托管和发布自己的静态网站页面。

### Hexo 是什么？

[Hexo](https://hexo.io/zh-cn/) 是一个快速、简洁且高效的静态博客框架，它基于 Node.js 运行，可以将我们撰写的 Markdown 文档解析渲染成静态的 HTML 网页。

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
      - uses: actions/checkout@v4
        with:
          token: ${{ secrets.GITHUB_TOKEN }}
          # If your repository depends on submodule, please see: https://github.com/actions/checkout
          submodules: recursive
      - name: Use Node.js <替换>.x
        uses: actions/setup-node@v4
        with:
          node-version: '<替换>'
      - name: Cache NPM dependencies
        uses: actions/cache@v4
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
        uses: actions/upload-pages-artifact@v3
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
        uses: actions/deploy-pages@v4
```

3. 在第一步的终端界面依次键入以下命令：

```bash
git init

git add .

git commit -m "first commit"

git branch -M main

git remote add origin <上面复制的仓库地址>

git push -u origin main
```

4. 静待片刻，打开你的浏览器，访问 `<你的 GitHub 用户名>.github.io`就可以看到你的页面啦！

## 开始使用

使用[VS Code](https://code.visualstudio.com/)(你也可以使用其他你习惯使用的编辑器)打开博客项目文件夹，我们可以看到以下目录结构：

![https://s2.loli.net/2024/01/22/j1kwmnV6AHRIP5r.png](https://s2.loli.net/2024/01/22/j1kwmnV6AHRIP5r.png)

```plaintext
.
│--.github
│  `-- workflows
|-- _config.landscape.yml   # Hexo默认主题的配置文件
|-- _config.yml             # Hexo网站的配置信息
|-- db.json
|-- package-lock.json
|-- package.json
|-- scaffolds               # 模板文件夹
|   |-- draft.md
|   |-- page.md
|   `-- post.md
|-- source                  # 用来存放用户资源
|   `-- _posts              # 用来存放已发布的博客
|       `-- hello-world.md  # Hexo初始化时生成的第一篇示例博客
|-- themes                  # 主题文件夹
`-- yarn.lock
```

对于博客的任意更改（修改配置、更换主题、新增及编辑文章等），将更改推送到Github，静待片刻，即可在你的博客页面看到变更。在博客项目文件夹打开终端，使用以下命令推送更改到Github：

```bash
git add .

git commit -m "变更内容描述"

git push -u origin main
```

### 网站设置

编辑`_config.yml`文件以修改博客网站的标题、描述、作者名、网站语言等，详见[Hexo官方配置文档](https://hexo.io/zh-cn/docs/configuration)。

{% note info %}

修改yml文件请注意不要改变每行前面的缩进，仅在冒号后修改内容，且在冒号与内容之间添加一个空格

{% endnote %}

### 发布文章

{% tabs 创建文章文件 %}

<!-- tab 使用hexo命令创建文章 -->

在VS Code中使用快捷键`Ctrl`+`` ` ``（数字键1左边的键）打开终端窗口，键入命令：

```bash
hexo new "<文件名>"
```

这时你会发现`source\_posts`文件夹下出现了一个名为`<文件名>.md`的文件，打开这个文件就可以开始撰写你的文章了。

![https://s2.loli.net/2024/01/22/E6wrj2YlstdeNgD.png](https://s2.loli.net/2024/01/22/E6wrj2YlstdeNgD.png)

<!-- endtab -->

<!-- tab 手动创建文章 -->

在`source\_posts`文件夹下直接新建文件`<文件名>.md`，手动在文件开头加入如下格式Front-matter内容，然后就可以开始撰写你的文章了。

```markdown
---
title: 文章标题
date: 2024/1/22 13:00:00
categories:
- <文章分类>
tags:
- <标签1>
- <标签2>
---
```

<!-- endtab -->

{% endtabs %}

在上面创建的文件中，你会注意到文件前有以`---`分割的区域，这是用来指定文章属性的，如标题、创建日期、更新日期、标签、分类等，详见[Hexo](https://hexo.io/zh-cn/docs/front-matter)。

在该区域之后，就是撰写博客正文的地方。Hexo使用[Markdown语法](https://markdown.com.cn/basic-syntax/)来撰写文章。

### 更换主题

Hexo[主题商店](https://hexo.io/themes/)中提供了400+种博客主题供你选择，选择一个你喜欢的主题，参考主题说明文档以使用主题。

本站点使用的主题为[hexo-theme-redefine](https://github.com/EvanNotFound/hexo-theme-redefine)，如需安装此主题，参考[主题文档](https://redefine-docs.ohevan.com/getting-started)。

{% folding blue::进阶： 使用Qexo可视化后台管理你的博客 %}

[Qexo | 一个美观、强大的在线 静态博客 管理器 (oplog.cn)](https://www.oplog.cn/qexo/)

部署完成后，你可以通过浏览器可视化管理你的个人博客：

![image](https://pic.hipyt.cn/pic/2023/08/21/19f6917e23b08.png)

{% endfolding %}

## 总结

这篇文章中，我们完成了以下工作：

- 创建Github仓库
- 创建了博客项目
- 部署我们的博客项目到GitHub Pages

至此，我们可以通过GitHub的子域名来访问我们的博客了。但是由于GitHub服务器在国外，外加某些原因，我们在访问github.io时感到会有些缓慢，有时甚至会偶尔无法访问。那么有没有解决办法呢？当然有，下一篇文章我们来聊聊如何加速访问我们的博客。
