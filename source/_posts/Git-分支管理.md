---
title: Git分支管理
date: 2019-9-5
tags: Git
author: 映雪
---

明月楼高休独倚，酒入愁肠，化作相思泪。

<!--more-->

### Git 分支

- 几乎每一种版本控制系统都以某种形式支持分支。使用分支意味着你可以从开发主线上分离开来，然后在不影响主线的同时继续工作。

### Git 分支管理

1. 创建分支

```
git branch dev
```

2. 查看分支

```
git branch
```

3. 切换分支

```
git checkout dev
```

4. 合并分支

- 将 dev 分支合并到 master 分支,在 dev 分支修改完成后并 commit 到缓存区，然后切换到 master 分支

```
git merge dev
```

5. 删除分支

- 如果 commit 提交了，但是还没有合并，这时删除分支的话代码会报错

```
git branch -d dev
```

- 强行删除分支

```
git branch -D dev
```

### Git 分支种类

1. master：git 默认主分支。
2. stable：稳定分支，替代 master，主要用来版本发布。
3. develop：日常开发分支，该分支正常保存了开发的最新代码。
4. feature：具体的功能开发分支，只与 develop 分支交互。
5. release：release 分支可以认为是 stable 分支的未测试版。比如说某一期的功能全部开发完成，那么就将 develop 分支合并到 release 分支，测试没有问题并且到了发布日期就合并到 stable 分支，进行发布。
6. bugfix：线上 bug 修复分支。

![截屏2022-05-12 下午4.20.12.png](/images/2022/05/12/Bhmz4NvIwAKDHRs.png)

### Git 分支提交规范

1. 在单人开发的情况下，master、develop分支需要上传到云仓库，feature分支只在本地保存即可。
2. Master分支用来部署生产环境，develop分支用来部署预发布环境。当生产环境Master分支上严禁提交代码，只支持代码合并。
3. 当生产环境发生紧急bug时，需要通过bugfix分支进行bug修复。
4. 当预发布环境产生bug时，代表当前开发的功能版本存在缺陷。 bug修复在原feature分支上修复即可。Bug修复后将代码依次合并到develop分支上。
5. develop分支允许小规模代码提交，例如配置文件修改，参数类型修改。如有代码逻辑修改需要创建新分支。
6. feature分支至少要多保存一个版本。例如：当前feature分支在开发1.2功能需求，既当前feature分支名称为feature-1.2，那么git仓库中feature分支至少要留存feature-1.1版本的分支。
