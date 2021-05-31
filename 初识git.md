# 一.Git相关知识

## 1.Git的结构

+ 工作区：写代码

+ 暂存区：进行临时存储

+ 本地库：保存历史版本

+ 工作区 (git add)-> 暂存区 (git commit)-> 本地库

## 2.Git和代码托管中心

+ 局域网：GitLab服务器

+ 外网环境下：GitHub和码云

+ 代码托管中心的任务：维护远程库

## 3.本地库和远程库

+ 团队内协作：本地库可推送(push)到远程库，也可以对远程库进行拉取(pull),其他团队成员可以将远程库克隆到本地库(clone),修改之后，在加入团队的前提下可以推送(push)到远程库

+ 跨团队协作：将A团队的远程库进行克隆(fork)到一个属于B团队的远程库,B团队进行clone到本地库修改后push到B团队的远程库，对A团队发起拉取请求(pull request),A团队审核通过后，在线进行两个远程库的合并(merge),完成跨团队的协作

## 4.Git的命令行操作

### 1.本地库初始化

+ 命令：git add
+ 效果：

  ```
  $ ll .git/
  total 7
  -rw-r--r-- 1 Caesar 197609 130  5月 31 17:21 config
  -rw-r--r-- 1 Caesar 197609  73  5月 31 17:21 description
  -rw-r--r-- 1 Caesar 197609  23  5月 31 17:21 HEAD
  drwxr-xr-x 1 Caesar 197609   0  5月 31 17:21 hooks/
  drwxr-xr-x 1 Caesar 197609   0  5月 31 17:21 info/
  drwxr-xr-x 1 Caesar 197609   0  5月 31 17:21 objects/
  drwxr-xr-x 1 Caesar 197609   0  5月 31 17:21 refs/

  ```

+ 注意：.git目录中存放的是本地库相关的子目录和文件，不要删除，也不要胡乱修改

### 2.设置签名

+ 形式：用户名和Email地址

+ 作用：区分不同开发人员的身份

+ 命令

    - 项目级别/仓库级别：仅在当前本地库范围内有效

        * git config user.name Caesar

        * git config user.email 2462252238@qq.com

        * 信息保存位置：.git/config文件

        ```
        $ cat .git/config
        [core]
                repositoryformatversion = 0
                filemode = false
                bare = false
                logallrefupdates = true
                symlinks = false
                ignorecase = true
        [user]
                name = Caesar
                email = 2462252238@qq.com
        ```

    - 系统用户级别：登录当前操作系统的用户范围

        * git config --global user.name Caesar

        * git config --global user.email 2462252238@qq.com

        * 信息保存位置：~/.gitconfig文件

        ```
        $ cat .gitconfig
        [user]
                name = Caesar
                email = 2462252238@qq.com
        ```

    - 级别优先级

        * 就近原则：项目级别优先于系统用户级别，二者都有时采用项目级别的签名

        * 如果只有系统用户级别的签名，则以系统用户级别的签名为准

        * 二者都没有不允许

### 3.基本操作

+ 添加操作：git add [file name] (将文件的“新建/修改”添加到暂存区)

+ 提交操作：git commit(输入后进入vim进行该次提交的备注信息输入，若加上-m "备注信息"可以不进入vim界面直接进行提交)(将暂存区的内容提交到本地库)

+ 状态查看操作：git status(查看工作区，暂存区状态 )

+ 历史版本信息查看操作：git log(显示历史版本信息)
    多屏显示控制方式：空格向下翻页，b 向上翻页，q 退出
    简洁命令：git log --pretty=oneline/--oneline(历史版本信息以一行的形式显示)
    显示HEAD指针的命令，并且以一行显示历史版本：git reflog(HEAD@{移动到当前版本需要多少步})

+ 版本的前进和后退

    - 基于索引值操作：git reset --hard [局部索引值]

    - 使用^符号：git reset --hard HEAD^ (只能后退)(^的个数为版本后退的版本数)

    - 使用~符号：git reset --hard HEAD~n (n：后退的步数) (只能后退)

    - reset命令的三个参数对比
    
        * --soft参数：仅仅在本地库移动HEAD指针

        * --mixed参数：在本地库移动HEAD指针，重置暂存区

        * --hard参数：在本地库移动HEAD指针，重置暂存区和工作区

+ 删除文件并找回

    - 前提：删除前，文件存在时的状态提交到了本地库

    - 操作：git reset --hard [指针位置]

        * 删除操作已经提交到本地库：指针位置指向历史记录

        * 删除操作尚未提交到本地库：指针位置使用HEAD

+ 比较文件差异

    - git diff [文件名] (将工作区的文件与暂存区进行比较)

    - git diff [本地库中的历史版本] [文件名] (将工作区中的文件与本地库中的历史记录进行比较)

    - 不带文件名时，会比较多个文件

## 5.分支管理

+ 分支的概念：在版本控制的过程中，使用多条线同时推进任务

+ 分支的好处

    - 同时并行推进多个功能的开发，提高开发效率

    - 各个分支在开发过程中，如果某一个分支开发失败，不会对其他分支有任何影响，失败的分支删除即可

+ 分支操作

    - 创建分支：git branch [分支名]

    - 查看分支：git branch -v

    - 切换分支：git checkout [分支名]

    - 合并分支：

        * 第一步：切换到接受修改的分支(被合并，增加新内容)上：git checkout [被合并分支名]

        * 第二步：执行merge命令：git merge [有新内容分支名]

    - 解决分支合并时的冲突问题

        * 第一步：编辑文件，删除特殊符号

        * 第二步：把文件修改到满意的程度

        * 第三步：git add [文件名]

        * 第四步：git commit -m "日志信息"
        注意：此时commit一定不能带文件名


# 二.总结

+ 学习收获：了解git并且学习了一些相关操作

+ 下一阶段学习目标：

    - 进一步学习git，创建远程库，GitHub相关操作等

    - 学习C语言共用体以及后侧知识，开始学习C++等
