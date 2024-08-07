---
title: "git & github"
date: 2024-08-06T20:55:26+08:00
author: ["ageha"]
tags: [ git, github, 凭据存储, credential, token ]
draft: false
weight: 3
summary: 关于 git 以及 github 的知识
---

### git 常用命令

- git add . (添加文件到暂存区
- git commit (将暂存区内容添加到仓库中

> 提示可疑仓库权时 
> git config --global --add safe.directory /path/to/dir

- git push (上传到远程仓库

### git 凭据存储（gpt 🐮）
> 避免每次都要手动输入`用户名`和`token`来提交

- 使用 git-credential-store 存储 token

``` bash
git config --global credential.helper store
```

运行以下命令将凭据保存到文件中

``` bash
git push https://github.com/username/repository.git
```

当系统提示输入用户名和密码时，使用以下格式输入：

    用户名：输入你的 GitHub 用户名
    密码：输入你的个人访问令牌（PAT）

这样，Git 会将凭据保存到本地文件中，以后你就不需要每次都输入 token 了。

> 这里发现 tab 可以给文本套个框 😄

### 提交到远程仓库

> 确保远程仓库存在且不为空

``` bash
git remote add origin https://github.com/username/repo
git branch -m main
...
git push
```


