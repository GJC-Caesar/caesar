# 一.Git相关知识

## 1.Git的文件管理机制

Git把数据看作是小型文件系统的一组快照。每次提交更新时，Git都会对当前的全部文件制作一个快照并保存这个快照的索引。为了高效，如果文件没有修改，Git不再重新存储该文件，而是只保留一个链接指向之前的文件。所以Git的工作方式可以称为快照流。

## 2.Git远程库

+ 创建远程库：在GitHub中注册账号并且创建一个远程库，复制该远程库地址并且于本地git库中进行远程库地址别名的命名，便于上传与推送等操作

+ 在本地使用git push [重命名] master指令将本地的git库推送到远程库

+ 克隆：git clone [远程地址]
    效果：完整地把远程库下载到本地
    创建origin远程地址别名
    初始化本地库

+ 拉取：pull = fetch + merge
    git fetch [远程库地址别名] [远程分支名]
    git merge [远程库地址别名/远程分支名]
    git pull [远程库地址别名] [远程分支名]

+ 解决冲突
    - 要点：如果不是基于GitHub远程库最新版所做的修改，不能推送，必须先拉取
            拉取下来后如果进入冲突状态，则按照“分支冲突解决”操作解决即可


