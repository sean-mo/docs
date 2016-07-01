# Github 相关

### 扩展

Octotree Sourcegraph


### github Star

- http://gitconstellation.com
- https://www.gitrep.com
- https://gitter.im/home

### gitlab issue 规范

 Labels 
 
 ```
 - bug                 错误
 - confirmed         证实
 - critical             关键
 - discussion         讨论
 - documentation   文档
 - enhancement     增强
 - feature             特点
 - suggestion         建议
 - support             支持
 ```
 
 
#git 分支

- `git merge branchName`: 将branchName合并到当前分支(快速合并)
- `git merge —on-ff -m ‘commit text’ branchName`: 将branchName合并到当前分支(禁用快速合并)

- `git branch`        				查看本地分支
- `git branch -a`                               查看远程分支
- `git push origin branchName`            创建远程分支(将本地branchName推送到远程） 
- `git checkout -b branchName`
- `git branch origin :branchName`        删除远程分支branchName
- `git checkout -b branchName`           创建本地分支
- `git checkout --track origin/dev`		 切换到远程dev分支
- `git stash`                                    保存当前工作现场（即git status是干净的)
- `git stash list`                               工作现场list
- `git stash pop`                               恢复工作现场，同时从stash删除该工作现场
- `git stash apply` 				恢复现场
- `git stash drop`				删除现场

安全
- `git fetch origin remote:local`         远程下载远程分支到本地
- `git diff diff local`                       对比
- `git merge local`                          合并 

- `git diff HEAD — filename `             查看暂存区内容比较
- `git checkout — filename`		丢弃工作区某个文件的修改
- `git reset HEAD filename`                add后进入暂存区，该文件从暂存区退回工作区

只能回退本地库，远程库无效
HEAD 指当前版本 HEAD^ 上一版本 
- `git rest —hard` HEAD 回退上一版本
- `git rest —hard commitid`  回退到commitId这个版本
- `git reflog`      查看历史操作命令 方便回退到未来的版本
- `git rm`         删除

- `git tag` 打版本标签


### 更多命令

```
git branch 查看本地所有分支
git status 查看当前状态 
git commit 提交 
git branch -a 查看所有的分支
git branch -r 查看远程所有分支
git commit -am "init" 提交并且加注释 
git remote add origin git@192.168.1.119:ndshow
git push origin master 将文件给推到服务器上 
git remote show origin 显示远程库origin里的资源 
git push origin master:develop
git push origin master:hb-dev 将本地库与服务器上的库进行关联 
git checkout --track origin/dev 切换到远程dev分支
git branch -D master develop 删除本地库develop
git checkout -b dev 建立一个新的本地分支dev
git merge origin/dev 将分支dev与当前分支进行合并
git checkout dev 切换到本地dev分支
git remote show 查看远程库
git add .
git rm 文件名(包括路径) 从git中删除指定文件
git clone git://github.com/schacon/grit.git 从服务器上将代码给拉下来
git config --list 看所有用户
git ls-files 看已经被提交的
git rm [file name] 删除一个文件
git commit -a 提交当前repos的所有的改变
git add [file name] 添加一个文件到git index
git commit -v 当你用－v参数的时候可以看commit的差异
git commit -m "This is the message describing the commit" 添加commit信息
git commit -a -a是代表add，把所有的change加到git index里然后再commit
git commit -a -v 一般提交命令
git log 看你commit的日志
git diff 查看尚未暂存的更新
git rm a.a 移除文件(从暂存区和工作区中删除)
git rm --cached a.a 移除文件(只从暂存区中删除)
git commit -m "remove" 移除文件(从Git中删除)
git rm -f a.a 强行移除修改后文件(从暂存区和工作区中删除)
git diff --cached 或 $ git diff --staged 查看尚未提交的更新
git stash push 将文件给push到一个临时空间中
git stash pop 将文件从临时空间pop下来
---------------------------------------------------------
git remote add origin git@github.com:username/Hello-World.git
git push origin master 将本地项目给提交到服务器中
-----------------------------------------------------------
git pull 本地与服务器端同步
-----------------------------------------------------------------
git push (远程仓库名) (分支名) 将本地分支推送到服务器上去。
git push origin serverfix:awesomebranch
------------------------------------------------------------------
git fetch 相当于是从远程获取最新版本到本地，不会自动merge
git commit -a -m "log_message" (-a是提交所有改动，-m是加入log信息) 本地修改同步至服务器端 ：
I
```

# Git Submodule

```
// 添加子模块
git submodule add git://github.com/felixge/node-mysql.git deps/mysql
// 获取依赖模块
git submodule init
// 更新依赖模块
$ git submodule update
// 获取 status 状态
git submodule status
// recursive 选项 自动初始化并更新仓库中的每一个子模块
git clone --recursive https://github.com/chaconinc/MainProject
// remote Git 将会进入子模块然后抓取并更新
git submodule update --remote
// -merge 服务器上的这个子模块有一个改动并且它被合并了进来
git submodule update --remote --merge
// rebase 本地做了更改时上游也有一个改动，我们需要将它并入本地。
 git submodule update --remote --rebase
// 推送到主项目前检查所有子模块是否已推送
 git push --recurse-submodules=check
// 尝试 推送子模块 那个子模块因为某些原因推送失败，主项目也会推送失败
 git push --recurse-submodules=on-demand
// 在每一个子模块中运行任意命令
git submodule foreach 'git checkout -b featureA'
git submodule foreach 'git stash'
git submodule foreach 'git diff'
```


## SSHKEY生成

> 生成一个SSHKEY

```
ssh-keygen -t rsa -C "xx@ooo.com"
```
> 会提示输入生成KEY文件的位置，一般我们都是多个sshkey，所以需要输入完整的路径，同时加_gitname。如 xx公司，则id_rsa_xx

> Enter file in which to save the key (/Users/moruhang/.ssh/id_rsa)

```
/Users/yourname/.ssh/id_rsa_xx
```
* 剩下就可以一路回车，之后查看，显示密钥串

```
cat ~/.ssh/id_rsa_xx.pub
```
> 密钥串会显示类似下面的内容，请复制它们到gitlab或者github的SSHKEY里创建

```
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDmXVj4KCtMbvW17UUw+Yef2HjMoIrzpSV5RwRiEMFJtBArAzTRxY12OadNhJCKDJ4W1SKWK3Ji3pyX/Gm+MW/4flbAwmHNmilI8aSIXryy0MzHj9JKPrTJmXrdVGpncIiH8DdAaCOrZRmrBkH/54EkBcGev2xpbZJWpO23iLRJciu1UlaOESw8ertqokF5m4b+lYTQmk+pMBkNXYCa0qJxZsc7QkaPHmYsC2mAd/YitkdH43zxmtrHwzRAEieIYQsQZdpxjSWHlpykrj7dWKSHSACaHSWQrVSpjCkRBy8DJ5QAH39CUu8XVSr3O8P4+nv6oMPZJC4j3xIiZHIJMxw9 xx@ooo.com

```
> 在~/.ssh/下创建一个config， 如下

```
# gitlab
Host gitlab.oooo.org
    HostName gitlab.oooo.org
    IdentityFile ~/.ssh/id_rsa_xx

# github
Host github.com
    HostName github.com
    IdentityFile ~/.ssh/id_rsa_github

```
> 验证是否成功

```
// xx@ooo.com 为你的帐号
ssh -t xx@ooo.com 
```
### 参考文档：

git submodule: https://git-scm.com/book/zh/v2/Git-%E5%B7%A5%E5%85%B7-%E5%AD%90%E6%A8%A1%E5%9D%97
