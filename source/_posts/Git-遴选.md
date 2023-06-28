---
title: Git遴选
date: 2019-9-9
tags: Git
author: 映雪
---

心静闲看物亦静，芭蕉过雨绿生凉。

<!--more-->

> 拉取单个提交

### 遴选

- 当执行 git merge 或者 git rebase 时，一个分支的所有提交都会被合并。cherry-pick 命令允许你选择单个提交进行整合。

### 基本用法

- 指定的提交（commit）应用于其他分支。

```
git cherry-pick <commitHash>
```

### 遴选单个提交

1. 切换test分支

```
 git checkout test

```

2. 遴选对应commit 

```
git cherry-pick <commitHash>
```

### 遴选多个提交

```
git cherry-pick <HashA> <HashB>
```

- 注意：上面的命令可以转移从 A 到 B 的所有提交。它们必须按照正确的顺序放置：提交 A 必须早于提交 B，否则命令将失败，但不会报错。