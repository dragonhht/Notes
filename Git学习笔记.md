# 1, 将文件放入Git文档库

## 1, 本地创建Git库

- 创建文件夹
- 在文件夹下执行 ==git init==
- 查看跟踪和未跟踪文件 ==git status==
- 将文件内容加入Git索引 ==git add 文件名==
- 将Git索引中的文件内容加入文档库 ==git commit -m "提交说明"==
- 创建 .gitignore 文件 ==touch .gitignore==

## 2, 删除Git索引中的特定文件

- 当文档库中还没有加入任何文件 ==git rm --cached 文件名==
- 当文档库中已有文件 ==git reset HEAD 文件名==

## 3, 查看commit节点

- 显示最新commit的详细信息 ==git show HEAD==

## 4, 从文档库中取文件

- 取出全部文件的最新版 ==git checkout .==
- 取出制定文件 ==git checkout 文件==

## 5, 清理Git文档库

> git gc 各参数使用

> - -- aggressive 默认使用较快速的方式检查文档库,并完成清理,当需要比较久的时间,偶尔使用即可

- --auto

  > git贵先判断文档库是否需要清理

- --no-prune

  > 要求git不要清理清除文档库中不会用到的数据, 只要整理他们就可以了

## 6, 建立分支,合并和解决冲突

## 1, 建立分支

- 创建分支 ==git branch 自己取的分支名称 [ commit 节点标识符或是标签 ]== 如果指定了commit节点就会从该节点长出分支,如果没有指定,就会从最新的节点长出分支
- 列出文档库中正在开的所有分支 ==git branch==
- 切换当前操作分支 ==git checkout 分支名称==
- 删除分支 ==git branch -d 分支名称==

## 2,合并分支和解决冲突

- 合并分支到主要分支 ==git merge 分支名称==
- 合并分支 A 和 B ==git checkout A== ==git merge B==

## 远程Git文档库

## 1, 创建远程Git文档库

### 1, 先创建本地文档库, 后创建远程文档库

- 设置本地与远程文档库的关系 ==git remote add "远程文档库的名称" "远程文档库的url"==
- 查看本地所有分支 ==git branch -a==
- 让Git自动对比本地和远程的文档库的分支名称,找出对应的分支,并且在本地文档库中创建追踪分支 ==git remote update==
- 将修改推送到远程文档库 ==git push origin 分支名称==
