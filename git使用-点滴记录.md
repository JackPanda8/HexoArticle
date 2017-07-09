---
title: git使用--点滴记录
date: 2017-02-13 12:52:20
tags: Git
---

## Git多人协作一般流程

>1. 首先，可以试图用`git push origin branch-name`推送自己的修改；
>2. 如果推送失败，则因为远程分支比你的本地更新，需要先用`git pull`试图合并；
>3. 如果合并有冲突，则解决冲突，并在本地提交；
>4. 没有冲突或者解决掉冲突后，再用`git push origin branch-name`推送就能成功！
>
>如果`git pull`提示“no tracking information”，则说明本地分支和远程分支的链接关系没有创建，用命令`git branch --set-upstream branch-name origin/branch-name`。
>
>[Git教程—廖雪峰](http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/0013760174128707b935b0be6fc4fc6ace66c4f15618f8d000)

<!-- more -->

## Git版本回退

1. git log 可以查看所有的commit历史，git reflog可以查看命令历史，两者搭配使用可以得到你想要回退到的commit之后的SHA1值（类似于该次commit的ID）。

2. git reset —hard commit_SHA1，该命令表示回退到ID为commit_SHA1的那次commit之后的状态。

   > **--soft**	Does not touch the index file or the working tree at all (but resets the head to <commit>, just like all modes do). This leaves all your changed files "Changes to be committed", as git status would put it.
   >
   > **--mixed**	Resets the index but not the working tree (i.e., the changed files are preserved but not marked for commit) and reports what has not been updated. This is the default action.If -N is specified, removed paths are marked as intent-to-add (see git-add[1]).
   >
   > **--hard**	Resets the index and working tree. Any changes to tracked files in the working tree since <commit> are discarded.
   >
   > **--merge**	Resets the index and updates the files in the working tree that are different between <commit> and HEAD, but keeps those which are different between the index and working tree (i.e. which have changes which have not been added). If a file that is different between <commit> and the index has unstaged changes, reset is aborted.In other words, --merge does something like a git read-tree -u -m <commit>, but carries forward unmerged index entries.
   >
   > **--keep**	Resets index entries and updates files in the working tree that are different between <commit> and HEAD. If a file that is different between <commit> and HEAD has local changes, reset is aborted.

如果只是想撤回某次commit，考虑使用 git revert。