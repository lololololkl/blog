---
tags: [Git]
categories: 
- other
- Git
title: Git
date: 2021-09-15 20:03:02
---

# git
```git
git init
git add .
git commit -m "comment..."
git pull 
```

# 实用命令汇总
### 工作区 -> 暂存区
```shell
$ git add <file/dir>
```


### 暂存区 -> 本地仓库
```shell
$ git commit -m "some info"
```
<!-- more -->

### 本地仓库 -> 远程仓库
```shell
$ git push origin master  # 本地master分支推送到远程origin仓库
```


### 工作区 <- 暂存区
```shell
$ git checkout -- <file>  # 暂存区文件内容覆盖工作区文件内容
```


### 暂存区 <- 本地仓库
```shell
$ git reset HEAD <file>   # 本地仓库文件内容覆盖暂存区文件内容
```


### 本地仓库 <- 远程仓库
```shell
$ git clone <git_url>        # 克隆远程仓库
$ git fetch upstream master  # 拉取远程代码到本地但不应用在当前分支
$ git pull upstream master   # 拉取远程代码到本地但应用在当前分支
$ git pull --rebase upstream master  # 如果平时使用rebase合并代码则加上
```


### 工作区 <- 本地仓库
```shell
$ git reset <commit>          # 本地仓库覆盖到工作区(保存回退文件内容修改)
$ git reset --mixed <commit>  # 本地仓库覆盖到工作区(保存回退文件内容修改)
$ git reset --soft <commit>   # 本地仓库覆盖到工作区(保留修改并加到暂存区)
$ git reset --hard <commit>   # 本地仓库覆盖到工作区(不保留修改直接删除掉)
```


# 配置实用参数选项
## 全局配置
### 用户信息
```shell
$ git config --global user.name "your_name"
$ git config --global user.email "your_email"
```


### 文本编辑器
```shell
$ git config --global core.editor "nvim"
```


### 分页器
```shell
$ git config --global core.pager "more"
```


### 别名
```$ git config --global alias.gs "git status"shell

```


### 纠错
```shell
$ git config --global help.autocorrect 1
```

## 个人配置
### 不加--global参数的话，则为个人配置
```shell
$ git config --list
$ git config user.name
$ git config user.name "your_name"
```


### 如果在项目中设置，则保存在.git/config文件里面
```shell
$ cat .git/config
[user]
    name = "your_name"
......

```
