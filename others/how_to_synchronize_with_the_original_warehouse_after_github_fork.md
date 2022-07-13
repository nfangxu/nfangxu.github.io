# Github进行fork后如何与原仓库同步

> 在 Gitbub 上, fork 别人的代码提交pr, 或者修改成自己的需要的内容是很常见的.
> 如何在 fork 别人代码之后, 重新拉取别人分支上的代码是一个问题.

## 方式 1: 暴力删除, 重新fork

> 如果你只是提交了一个小bug, 或者你提交的代码, 已经提交到了别人代码的主仓库, 你可以使用这个方法.

1. 在你fork仓库的setting的最下面, delete这个仓库
2. 进入别人的仓库, 重新fork

## 方式 2: 通过 upstream 同步

> 通过设置 upstream (中文名: 上游分支), 同步远程分支

### 设置 upstream

- 查看是否设置 upstream 分支 

```bash
git remote -v
```

如果没有设置, 你会看到以下信息

```
origin	git@github.com:nfangxu/hyperf.git (fetch)
origin	git@github.com:nfangxu/hyperf.git (push)
```

- 设置 upstream

> 一般情况下，设置好一次 upstream 后就无需重复设置。

```bash
git remote add upstream https://github.com/hyperf/hyperf.git
```

- 查看是否设置成功

```bash
git remote -v
```

如果成功设置, 你会看到以下信息

```
origin	git@github.com:nfangxu/hyperf.git (fetch)
origin	git@github.com:nfangxu/hyperf.git (push)
upstream	https://github.com/hyperf/hyperf.git (fetch)
upstream	https://github.com/hyperf/hyperf.git (push)
```

### 合并代码

- 获取原仓库的更新

```bash
git fetch upstream
```

- 切换到指定分支(一般是 `master`)

```bash
git checkout master
```

- 合并远程代码

```bash
git merge upstream/master
```

- 将代码同步到自己的仓库

```bash
git push origin master
```