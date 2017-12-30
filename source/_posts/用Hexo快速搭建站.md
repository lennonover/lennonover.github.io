---
title: 用Hexo快速搭建站
date: 2016-07-08 22:57:14
tags:
  - Hexo
---
## 准备
首先电脑里需要安装以下两个软件
- **git**
- **Node.js**

## Git绑定Github账号
- 通过ssh key与github网站帐户绑定
使用命令：

```plain
ssh-keygen -C '你的GitHub账号' -t rsa
```

- 会在用户目录 ~/.ssh/下建立相应的密钥文件然后打开这个文件复制密钥

- 打开GithHub设置 增加密钥

- 通过以下命令测试

```plain
ssh -v 你的GitHub账号
```


## 搭建本地Hexo博客

- 安装Hexo

```plain
npm install -g hexo
```

- 创建hexo文件夹
随便一个文件夹下（如E:\hexo），在E:\hexo内点击鼠标右键，选择Git bash，执行以下指令

```plain
hexo init
npm install
npm install hexo-deployer-git --save（这步很重要）
```

- 生成和查看本地博客
执行以下命令，然后到浏览器输入localhost:4000查看本地博客。

```plain
hexo generate
hexo server
```

## 部署到GitHub

- 编辑_config.yml

``` python
deploy:
  type: git
  repository: https://github.com/你的用户名/你的用户名.github.io.git
  branch: master
```

- 执行下列指令即可完成部署

```plain
hexo generate
hexo deploy
```

## 复制主题
看的一些大神酷炫的主题，是不是很想要（有能力自己写也可以）

- 复制主题
例如复制next主题

```plain
git clone https://github.com/iissnan/hexo-theme-next themes/next
```

- 启用主题
修改Hexo目录下的config.yml配置文件中的theme属性

```plain
theme: next
```

- 更新主题

```plain
cd themes/next
git pull
```

- 主题其他修改配置可以去看作者文档

- 增加一些Hexo常用命令

``` python
hexo new "文件名" #创建新文章，名称可自己定义
hexo n == hexo new #创建新文章，默认名称为 my new post
hexo g == hexo generate #生成
hexo s == hexo server #启动本地服务，进行文章预览调试
hexo d == hexo deploy #部署到GitHub
hexo d -g == hexo generate hexo deploy
hexo new page "about"
```




