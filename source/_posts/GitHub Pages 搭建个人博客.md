---
title: GitHub Pages 搭建个人博客
date: 2021-01-06 16:08:37
tags: 
    - GitHub Pages
    - Hexo
    - hexo-theme-matery
    - GitHub Actions
---

## 1. GitHub 新建 xxl115.github.io 仓库

参考 [GitHub Pages](https://pages.github.com/) 初始化仓库

## 2. 使用 hexo 生成静态博客系统

对比 [hexo](https://hexo.io/)、 [Jekyll](https://jekyllrb.com/)、 [hugo](https://gohugo.io/) ，最后决定使用hexo。

### hexo 常用命令，[官方文档](https://hexo.io/zh-cn/docs/commands)

#### init

```bash
    hexo init [folder]
```

新建一个网站。如果没有设置 folder ，Hexo 默认在目前的文件夹建立网站。

#### new

```bash
    hexo new [layout] <title>
```

新建一篇文章。如果没有设置 layout 的话，默认使用 _config.yml 中的 default_layout 参数代替。如果标题包含空格的话，请使用引号括起来。

参数|描述
---|---
-p, --path  | 自定义新文章的路径
-r, --replace   | 如果存在同名文章，将其替换
-s, --slug  | 文章的 Slug，作为新文章的文件名和发布后的 URL

layout 三种默认布局：post、page 和 draft。在创建这三种不同类型的文件时，它们将会被保存到不同的路径；而您自定义的其他布局和 post 相同，都将储存到 source/_posts 文件夹。

layout  |   path
----    |   ----
post    |   source/_posts
page    |   source
draft   |   source/_drafts

#### generate

```bash
    hexo generate
```

生成静态网站。

该命令可以简写为

```bash
    hexo g
```

#### server

```bash
    hexo server
```

启动服务器。默认情况下，访问网址为： http://localhost:4000/。

#### clean

```bash
    hexo clean
```

清除缓存文件 (db.json) 和已生成的静态文件 (public)。

在某些情况（尤其是更换主题后），如果发现您对站点的更改无论如何也不生效，您可能需要运行该命令。

## 3. 选用 [hexo-theme-matery](https://github.com/blinkfox/hexo-theme-matery) 主题

通过对比和筛选，最后选择使用 hexo-theme-matery。

hexo-theme-matery 是一个采用 Material Design 和响应式设计的 Hexo 博客主题。

### 特性

- 简单漂亮，文章内容美观易读
- [Material Design](https://material.io/) 设计
- 响应式设计，博客在桌面端、平板、手机等设备上均能很好的展现
- 首页轮播文章及每天动态切换 `Banner` 图片
- 瀑布流式的博客文章列表（文章无特色图片时会有 `24` 张漂亮的图片代替）
- 时间轴式的归档页
- **词云**的标签页和**雷达图**的分类页
- 丰富的关于我页面（包括关于我、文章统计图、我的项目、我的技能、相册等）
- 可自定义的数据的友情链接页面
- 支持文章置顶和文章打赏
- 支持 `MathJax`
- `TOC` 目录
- 可设置复制文章内容时追加版权信息
- 可设置阅读文章时做密码验证
- [Gitalk](https://gitalk.github.io/)、[Gitment](https://imsun.github.io/gitment/)、[Valine](https://valine.js.org/) 和 [Disqus](https://disqus.com/) 评论模块（推荐使用 `Gitalk`）
- 集成了[不蒜子统计](http://busuanzi.ibruce.info/)、谷歌分析（`Google Analytics`）和文章字数统计等功能
- 支持在首页的音乐播放和视频播放功能
- 支持`emoji`表情，用`markdown emoji`语法书写直接生成对应的能**跳跃**的表情。
- 支持 [DaoVoice](http://www.daovoice.io/)、[Tidio](https://www.tidio.com/) 在线聊天功能。

## 4. 使用 GitHub Actions 自动部署静态网站

### 思路

- 在hexo分支，使用 hexo g 生成静态文件
- 将静态文件提交至 master 分支，通过域名就可以访问
  
### 解决办法

通过 Github Actions 中 workflow 自动化执行，在 hexo 分支根目录新建文件夹 .github\workflows ，在该文件中新建 node.js.yml 文件，每当 push 到该分支都会 CI 。

node.js.yml

```yml
name: Node.js CI

on:
  push:
    branches: 
      - hexo

jobs:
  build:
    runs-on: ubuntu-latest
    strategy:
      matrix:
        node-version: 
          - 14.x
    steps:
    - uses: actions/checkout@v2
    - name: Use Node.js ${{ matrix.node-version }}
      uses: actions/setup-node@v1
      with:
        node-version: ${{ matrix.node-version }}
    - name: Install hexo
      run: |
        npm -g install hexo
        npm audit fix
    - name: hexo generate
      run: |
        hexo clean 
        hexo generate
    - name: Commit files
      working-directory: ./public
      run: |
        git init
        git config user.name "Young Bean" 
        git config user.email "xxl115@163.com"
        git add .
        git commit -a -m "Auto update docs by Travis CI."
    - name: Push changes
      uses: ad-m/github-push-action@master
      with:
        github_token: ${{ secrets.GITHUB_TOKEN }}
        branch: master
        force: true
        directory: ./public
```

## 5. 使用自己的域名

在 hexo 分支 source 文件夹下新建 CNAME 文件，CNAME 文件内容为自己的域名地址 blog.boletime.com ，具体可参考 [配置 GitHub Pages 站点的自定义域](https://docs.github.com/cn/free-pro-team@latest/github/working-with-github-pages/configuring-a-custom-domain-for-your-github-pages-site)。
