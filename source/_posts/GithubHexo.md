---
title: Github搭建Hexo博客
date: 2017-12-30 17:33:02
tags: 技术
---

[TOC]

## 开始之前
为什么要用Github搭建Hexo的博客呢？首先，省钱！作为一个学生狗，服务器支出是一笔不少的费用，虽然我现在有一台腾讯云1元的服务器，但是这台服务器还有别的用途。用Github搭建网站完全免费。除此之外，Hexo生成的是静态网站，托管在Github上，只要不是有人黑了你的Github帐号，你的网站就很安全。于是我背叛了worldpress转到了Hexo+Github上

## 准备
- github建立Repository，名字为你的名字.github.io（如wwillv.github.io）
- 电脑上安装[Git](https://git-scm.com/)和[Node.js](https://nodejs.org/)(国内上不去可以用[Node.js中文网](nodejs.cn/download/))

## 安装
1. 在你准备建立Hexo的位置上建立一个文件夹，如blog，cd进入该目录`npm install -g hexo`安装hexo
![npm install -g hexo](https://i.loli.net/2017/12/30/5a47283abf088.png)
2. 输入`hexo init`初始化hexo
3. 输入`hexo g(或hexo generate)`生成静态页面
4. 输入`hexo s(或hexo server)`启动本地服务![hexo s](https://i.loli.net/2017/12/30/5a472a1b2b344.png)浏览器打开`localhost:4000`![localhost:4000](https://i.loli.net/2017/12/30/5a472ad46807c.png)如果你能看到这个界面说明你的hexo搭建已基本成功
5. 此时你的hexo本地配置已基本完成，可以上传github了，但为了方便我将在本地顺便将主题和插件也一并配置结束，你可以跳过这一步骤直接到上传Github去

## 配置主题
1. 主题选择，网上有很多hexo的主题，github上也有很多，我选择的是[Next](https://github.com/iissnan/hexo-theme-next)
2. 大多数的主题的页面都有详细的配置教程，以Next为例
3. cd进入themes目录，clone仓库到next文件夹`git clone https://github.com/iissnan/hexo-theme-next next`
![clone](https://i.loli.net/2017/12/30/5a4734e8cd0b0.png)
4. 在 hexo 根目录下 的配置文件_config.yml里设置主题`theme: next`
5. 根据自己需要可以继续自定义主题，next主题可以参考[这篇文章](https://www.jianshu.com/p/f054333ac9e6)
6. 运行`hexo clean`，运行`hexo g`后运行`hexo s`，浏览器打开`http://localhost:4000/`可以看到自己的新主题的样子了
![最终效果](https://i.loli.net/2017/12/30/5a475263a4e34.png)

## 上传Github
1. 回到主目录，安装hexo-deployer-git`npm install hexo-deployer-git`
2. 修改blog文件夹（博客根目录）下的`_config.yml`，最下面的`deploy:`修改为如下格式
```
deploy:
  type: git
  repo: https://github.com/WWILLV/wwillv.github.io.git（这里是你的仓库地址，后面要加.git）
  branch: master
```
3. `hexo d（或hexo deploy）`将网站上传至github
![上传Github](https://i.loli.net/2017/12/30/5a47568210341.png)
4. 网站已建立完毕，访问你的名字.github.io（如wwillv.github.io）即可查看
![](https://i.loli.net/2017/12/30/5a4757280d2c9.png)

## 网站备份
由于hexo上传到Github上是生成的网站，源文件是不会上传的，换电脑后再移动文件很麻烦，这里推荐一个自动备份网站到Github的工具[hexo-git-backup](https://github.com/coneycode/hexo-git-backup),可按照README的介绍进行安装
1. `npm install hexo-git-backup --save`安装hexo-git-backup
2. 修改`config.yml`，加入如下内容：
```
backup:
    type: git
    theme: next,landscape,xxx
    repository:
       github: git@github.com:xxx/xxx.git,branchName
```
3. `hexo b`备份文件
4. 获取备份
```
git clone git@github.com:username/username.github.io.git
git checkout branchname
```

## 绑定域名
1. 注册域名，腾讯云，阿里云（万网），GoDaddy什么的都可以，域名注册商有很多
2. 一般域名注册都提供域名的解析，解析前ping一下自己的github.io `ping wwillv.github.io`
![ping](https://i.loli.net/2017/12/30/5a47a0048e2c3.png)
如图所示我的IP是151.101.229.147
3. 已腾讯云的解析为例，添加如下2条记录，1条A记录和1条CHAME记录
![解析](https://i.loli.net/2017/12/30/5a47a0bdbe9d4.png)
4. 在Github的仓库的设置Custom domain里填入自己的域名
![github Custom domain](https://i.loli.net/2017/12/30/5a47a0bdbfe3d.png)
5. 浏览器输入自己的域名，刷新一会儿就出来了
![fin](https://i.loli.net/2017/12/30/5a47a19107fa6.png)

## 开始第一篇文章
`hexo new "title"（或hexo n "title"）`新建一篇文章，也可以在`\source\_posts`新建一个.md文件。hexo使用markdown语法，文章也是md格式。与平时的富文本的博客不同，写作需要遵循markdown的语法。删除文章直接删除对应的.md文件后`hexo g`重新生成一下即可。

常用hexo命令
> hexo new "postName" #新建文章
> hexo new page "pageName" #新建页面
> hexo generate #生成静态页面至public目录
> hexo server #开启预览访问端口（默认端口4000，'ctrl + c'关闭server）
> hexo deploy #部署到GitHub
> hexo help  # 查看帮助
> hexo version  #查看Hexo的版本

命令缩写
> hexo n == hexo new
> hexo g == hexo generate
> hexo s == hexo server
> hexo d == hexo deploy

组合命令：
> hexo s -g #生成并本地预览
> hexo d -g #生成并上传