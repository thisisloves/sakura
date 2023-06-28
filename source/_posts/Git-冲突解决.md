---
title: Git冲突解决
date: 2019-9-8
tags: Git
author: 映雪
---

多少红颜悴，多少相思碎，唯留血染墨香哭乱冢。

<!--more-->

> 远程的代码更新,本地的代码也更新，代码与远端存在冲突。

### 冲突解决

1. 把远程仓库的 master 分支下载到本地并存为 tem 分支

```
git fetch origin master:tep
```

2. 查看 tem 分支和本地分支有何不同

```
git diff tmp
```

3. 将 tem 分支和本地的 master 分支合并

```
git merge tmp
```

4. 这时候本地和远程就没了冲突

```
git branch -d tmp 删除tmp 分支
```
