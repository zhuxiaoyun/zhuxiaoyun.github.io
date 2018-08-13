---
title: 多PC同步管理博客
date: 2017-06-07 17:14:53
categories:  #这里写的分类会自动汇集到 categories 页面上，分类可以多级，Hexo不支持指定多个同级分类，所以个人博客将是实用技术的二级分类
- 实用技术 # 一级分类
- 个人博客 # 二级分类 
tags:   #这里写的标签会自动汇集到 tags 页面上
- 实用 #可配置多个标签，注意格式
- 个人博客
---
链接：http://www.imooc.com/article/9707?block_id=tuijian_wz
来源：慕课网

### A电脑备份博客内容到github
1. 配置.gitignore文件。进入博客目录文件夹下，找到此文件，用sublime text 打开，在最后增加两行内容/.deploy_git和/public
2. 初始化仓库。在博客根目录下，在git bash下依次执行git init和git remote add origin <server> <server>为远程仓库地址。
3. 同步到远程仓库。gitbash下依次执行以下命令

```bash
$ git add . #添加目录下所有文件 
$ git commit -m "更新说明" #提交并添加更新说明 
$ git push -u origin master #推送更新到远程仓库
```

### B电脑拉下远程仓库文件
在B电脑上同样先安装好node、git、ssh、hexo，然后建好hexo文件夹，安装好插件，（然后选做：将备份到远程仓库的文件及文件夹删除），然后执行以下命令：

```bash
$ git init
$ git remote add origin <server>
$ git fetch --all
$ git reset --hard origin/master
```

### 发布博客后同步
在B电脑发布完博客之后，记得将博客备份同步到远程仓库
执行以下命令：

```bash
$ git add .
#可以用git master 查看更改内容
$ git commit -m "更新信息"
$ git push -u origin master #以后每次提交可以直接git push
```

### 平时同步管理
每次想写博客时，先执行：git pull进行同步更新。发布完文章后同样按照上面的 发布博客后同步 同步到远程仓库。

### 平时常用命令整理

```bash 
$ git pull #同步更新
$ hexo new post "新建文章" #简写形式 hexo n "新建文章"
$ hexo clean #清除旧的public文件夹
$ hexo generate #生成静态文件 简写形式 hexo g
$ hexo server #本地运行查看 简写形式 hexo s
$ hexo deploy #发布到github上 简写形式 hexo d
$ git add . #添加更改文件到缓存区
$ git commit -m "更新说明" #提交到本地仓库
$ git push -u origin master #推送到远程仓库进行备份
```

### 解决hexo神烦的DTraceProviderBindings MODULE_NOT_FOUND

原文：http://kikoroc.com/2016/05/04/resolve-hexo-DTraceProviderBindings-MODULE-NOT-FOUND.html

#### hexo报错
```
{ [Error: Cannot find module './build/Release/DTraceProviderBindings'] code: 'MODULE_NOT_FOUND' }
{ [Error: Cannot find module './build/default/DTraceProviderBindings'] code: 'MODULE_NOT_FOUND' }
{ [Error: Cannot find module './build/Debug/DTraceProviderBindings'] code: 'MODULE_NOT_FOUND' }
```

#### 解决办法

卸载后重装 hexo
```
$ npm uninstall hexo
$ npm install hexo --no-optional
```

还是有问题
其实hexo暂时并没有用到dtrace-prodider，仅仅是报错而已，hexo的命令还是能执行，但对于强迫症来说，简直无法忍受。

一番折腾之后，我找到了适合我的解决办法，重装hexo-cli:
```
$ npm uninstall hexo-cli -g
$ npm install hexo-cli -g
```