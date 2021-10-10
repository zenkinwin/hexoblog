---
title: git add -A 和 git add . 的区别
categories:
  - Git
tags:
  - git
abbrlink: '5130'
date: 2021-09-19 23:12:21
updated: 2021-09-19 23:12:21
---

整理几条记录关于 `git add` 相关内容，特别是对删除文件时候需要使用 `git add -A` 来提交。<!--more-->

### git add -u

提交被修改（modified）和被删除（deleted）文件，不包括新文件（new）。他仅监控已经被add的文件（即tracked file），他会将被修改的文件提交到暂存区。`git add -u` 不会提交新文件（untracked file）。（ `git add --update` 的缩写）

### git add .

提交新文件（new）和被修改（modified）文件，不包括被删除（deleted）文件。他会监控工作区的状态树，使用它会把工作时的所有变化提交到暂存区，包括文件内容修改(modified)以及新文件（new），但不包括被删除的文件。

### git add -A

提交所有变化，是上面两个功能的合集（ `git add --all` 的缩写），提交被修改（modified）和被删除（deleted）文件，包括新文件（new）。
