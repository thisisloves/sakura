---
title: Git安装
date: 2019-6-14
tags: Git
author: 映雪
---

锺情怕到相思路。盼长堤，草尽红心。动愁吟，碧落黄泉，两处难寻。

<!--more-->

### Git 安装

- 进入网站https://git-scm.com/下载安装，然后在cmd命令行配置

```git
> git config --global user.name "FishC_Service"
> git config --global user.email "fishc_service@126.com"
#检查信息是否写入成功
git config --list 
```

### Git 工作

![截屏2022-05-11 下午5.47.32.png](/images/2022/05/11/ol7fbXry1OV4gzt.png)

- workspace：工作区
- staging area：暂存区/缓存区
- local repository：版本库或本地仓库
- remote repository：远程仓库


### Git 连接远程仓库并推送

1. 初始化本地仓库

```
git init
```

2. 添加文件到缓存区

```
git add .
```

3. 连接远程仓库

```
git remote add origin https://github.com/thisisloves/111.git
```

4.  将暂存区里的改动给提交到本地的版本库

```
git commit -m "备注信息"
```

5.  推送分支到远程分支

```
git push -u origin master 第一次加-u
```
