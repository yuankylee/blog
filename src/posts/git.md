---
title: "常用 Git 命令整理"
date: "2016-03-02"
description: "常用 Git 命令整理"
tags: ["git", "note"]
---

### 编辑git命令简写 
```bash
git config --global alias.co checkout
```
上面的意思是将checkout设置为co简化命令。
<p style="font-size:14px">我自己在开发中常用的简化命令，为方面记录，下面描述用简化命令</span>

```bash
git config --global alias.rm remove
git config --global alias.co checkout
git config --global alias.ci commit 
git config --global alias.st status 
git config --global alias.br branch 
```

### 常用流程命令
<div style= "box-sizing: border-box;
    margin: 0 0 1.75em 0;
    border: #E3EDF3 1px solid;
    width: 100%;
    padding: 10px;
    font-family: Inconsolata, monospace, sans-serif;
    font-size: 0.9em;
    white-space: pre;
    overflow: auto;
    background: #F7FAFB;
    border-radius: 3px;">git init <span style="color: #999; font-size: 14px">初始化一个版本仓库</span>
git clone git@xxxx.git  <span style="color: #999; font-size: 14px">Clone远程版本库</span>
git config --global user.name "[name]" <span style="color: #999; font-size: 14px">配置用户信息</span>
git config --global user.email "[email address]" <span style="color: #999; font-size: 14px">配置用户信息  
注：global全局 local本地</span>
git config --list <span style="color: #999; font-size: 14px">查看当前配置</span>
git remote add [shortname] [url]  <span style="color: #999; font-size: 14px">添加远程版本库origin</span>
eg: git remote add origin git@xxxx.git <span style="color: #999; font-size: 14px">配置使用git仓库的人员email </span>

**提交你的修改**
git status  <span style="color: #999; font-size: 14px">查看文件状态</span>
git add .  <span style="color: #999; font-size: 14px">添加当前修改的文件到暂存区</span>
git commit –m "[message]"  <span style="color: #999; font-size: 14px">提交你的修改</span>
git push [远程名] [本地分支]:[远程分支]  <span style="color: #999; font-size: 14px">推送你的更新到远程服务器
eg: git push origin master</span>
</div>

假设你已经使用`git add .`，将修改过的文件a、b加到暂存区

现在你只想提交a文件，不想提交b文件，应该这样
`git reset HEAD b`

### git常用命令扩展

查看、切换、创建和删除分支
```bash
#切换到某个分支
git co <branch>
#创建新的分支，并且切换过去
git co -b <new_branch> 
 
#删除某个分支
git br -d <branch> 
#强制删除某个分支 (未被合并的分支被删除的时候需要强制)
git br -D <branch> 
```
分支合并和rebase
```bash
git merge <branch> //将branch分支合并到当前分支
git merge origin/master --no-ff  //不要Fast-Foward合并，这样可以生成merge提交
 
git rebase master  //将master rebase到branch
```
Git暂存管理
```bash
git stash //暂存
git stash pop //恢复暂存的内容
git stash drop //删除暂存区
```
Git远程分支管理
```bash
git pull //抓取远程仓库所有分支更新并合并到本地
git pull --no-ff //抓取远程仓库所有分支更新并合并到本地，不要快进合并
git fetch origin //抓取远程仓库更新
git merge origin/master //将远程主分支合并到本地当前分支
git co --track origin/branch //跟踪某个远程分支创建相应的本地分支
git co -b <local_branch> origin/<remote_branch> # 基于远程分支创建本地分支，功能同上
 
git push //push所有分支
git push origin master //将本地主分支推到远程主分支
git push -u origin master //将本地主分支推到远程(如无远程主分支则创建，用于初始化远程仓库)
git push origin <local_branch> //创建远程分支， origin是远程仓库名
git push origin <local_branch>:<remote_branch> //创建远程分支
git push origin :<remote_branch> #先删除本地分支(git br -d <branch>)，然后再push删除远程分支
```
查看文件diff
```bash
git diff <file> //比较当前文件和暂存区文件差异
git diff <$id1> <$id2> //比较两次提交之间的差异
git diff <branch1>..<branch2> //在两个分支之间比较
git diff --staged //比较暂存区和版本库差异
git diff --cached //比较暂存区和版本库差异
git diff --stat //仅仅比较统计信息
```
查看提交记录
```bash
git log
git log <file> //查看该文件每次提交记录
git log -p <file> //查看每次详细修改内容的diff
git log -p -2 //查看最近两次详细修改内容的diff
git log --stat //查看提交统计信息
```


基本的分支管理
```bash
git branch br01  //创建一个分支
git chekcout br01  //切换工作目录到br01
git chekcout –b br01  //将上面的命令合在一起，创建br01分支并切换到br01
git merge br01  //合并br01分支，当前工作目录为master
git branch –d br01  //合并完成后，没有出现冲突，删除br01分支
git fetch [remote-name]  //拉去远程仓库的数据
git checkout –b dev origin/develop  //建立本地的dev分支追踪远程仓库的develop分支
```
Git远程仓库管理
```bash
git remote -v //查看远程服务器地址和仓库名称
git remote show origin //查看远程服务器仓库状态
git remote add origin [git url] //添加远程仓库地址
git remote set-url origin [git url]  //设置远程仓库地址(用于修改远程仓库地址)
git remote rm <repository> //删除远程仓库
```