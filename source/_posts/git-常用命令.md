---
title: git 常用命令
date: 2022-04-11 09:24:48
tags:
    - git
    - 工具
categories: 
	- 工具
---

1. git clone  克隆
2. git add  添加
3. git commit 提交本地
4. git push 推送远程
5. git push -u origin test 与 test 分支建立关联
6. git pull 拉取新代码
7. git pull origin test    拉取制定分支代码
8. git branch 查看本地分支
9. git branch -a 查看所有分支
10. git branch -vv 查看本地和远程分支关联情况
11. git branch --set-upstream-to=origin/test 修改远程分支关联
12. git branch -D test 删除 test 分支名
13. git stash
14. git stash list 查看历史
15. git stash show -p stash@{0}  查看 stash@{0} 的具体内容
16. git checkout 切换分支
17. git checkout -b 创建新分支
18. git checkout -b test origin test  创建新分支且关联远程分支
19. git merge  合并其他分支代码到目前分支
20. git rebase -i test 将当前分支提交合并至 test 分支
21. git rebase -i logID 合并 logID 后所有提交   参考： 1. [git rebase -i 合并多次提交](https://www.jianshu.com/p/201a56ffe9a4)   2. [聊下 git rebase -i](https://www.cnblogs.com/wangiqngpei557/p/5989292.html)
22. git rebase --onto  参考：   [Git rebase --onto 用法](https://www.jianshu.com/p/4c1ed3dbf421)
23. git cherry-pick 合并某一次提交   参考：[阮一峰的网络日志  git cherry-pick 教程](http://www.ruanyifeng.com/blog/2020/04/git-cherry-pick.html) 
24. git remote 查看远程仓库名称
25. git remote -v 查看远程仓库名称和地址
26. git remote origin set-url NEWURL  修改远程仓库地址
27. git log --name-status 每次修改的文件列表，显示状态
28. git log --name-only 每次修改的文件列表
29. git log --stat 每次修改的文件列表，及文件修改的统计
30. git reset HEAD~1



git commit 提交规范

- feat: 新功能（feature）
- fix: 修补 bug
- docs: 文档（documentation）
- style: 格式（不影响代码运行的变动）
- refactor: 重构（即不是新增功能，也不是修改 bug 的代码变动）
- chore: 构建过程或辅助工具的变动
- revert: 撤销，版本回退
- perf: 性能优化
- test：测试
- improvement: 改进
- build: 打包
- ci: 持续集成
