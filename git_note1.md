
# Git是目前世界上最先进的分布式版本控制系统（没有之一）

## 基本命令
 - 创建版本库：在合适的地方使用mkdir命令创建目录
 - pwd命令显示当前目录
 - 使用git init 命令将创建的目录变成Git可以管理的仓库
 - git add <file>命令将文件添加到仓库
 - git commit -m <message>命令文件提交到仓库
 - 修改文件中的内容之后使用git status命令查看结果
 - git diff <file>可以查看具体修改的内容
 - git add、git commit重新提交


## 时光穿梭机
 > git log命令显示从最近到最远的提交日志
 > git log --pretty=oneline 显示版本号和提交日志
 > 回退到上一个版本git reset --hard HEAD^
 > git reset --hard <版本号前几位>
 > git reflog 记录每一次命令（即可以找到所有的版本号）

### 工作区是在电脑中能看到的目录，版本库是工作区中的隐藏目录 .git
 
stage 为暂存区，git add将文件添加到暂存区
创建Git版本库时，Git自动为我们创建了master分支，git commit将暂存区的所有内容提交到当前分支。

### Git跟踪并管理的是修改而并非文件。

git checkout -- <filename>
若该文件还没有被放到暂存区中，撤销修改就回到和版本库中一模一样的状态
若该文件已经添加到暂存区中，做了修改之后，撤销修改就回到添加到暂存区后的状态。

-改乱了工作区中某个文件内容时，若直接丢弃工作区中的修改，用命令git checkout -- file
-改乱了工作区某个文件内容，并且提交到了暂存区中，若想丢弃修改，第一步用命令git reset HEAD <file>/git restore <file>撤销提交到暂存区的修改，第二步git checkout -- file

## 删除文件
创建文件后git add、git commit操作，使用rm <file>删除文件
若需从版本库中删除该文件，使用git rm命令，并且git commit
若误删文件，使用git checkout -- file撤销删除，文件恢复为版本库中的内容

## Git杀手级功能之一：远程仓库
GitHub提供Git仓库托管服务
创建SSH Key：
打开 Git Bash，ssh-keygen -t rsa -C “email”
cd ~/.ssh 打开.ssh文件夹 pwd查看路径 将.ssh文件下的id_rsa.pub中的内容粘贴到Github下的SSH Key里
关联一个远程库，使用命令git remote add origin git@server-name:path/repo-name.git
关联后，git push -u origin master第一次推送master分支的所有内容，此后每次本地提交后，使用命令git push origin master推送最新修改
git clone将GitHub中的库克隆到本地

## 分支管理：
创建和合并分支
创建并切换到分支：git checkout -b <分支名> 即 git branch <fenzhi>、git checkout <fenzhi>
git branch查看当前分支
在新创建的分支上修改内容，git add、git commit -m提交到该分支，切换回master分支后，发现文件内容未修改，使用git merge <fenzhi>命令将分支合并到master分支，使用git branch -d <fenzhi>命令删除分支
创建并切换分支也可以用git switch -c <fenzhi>，切换到master分支 git switch master
查看分支：git branch
创建分支：git branch <name>
切换分支：git checkout <name>或者git switch <name>
创建+切换分支：git checkout -b <name>或者git switch -c <name>
合并某分支到当前分支：git merge <name>
删除分支：git branch -d <name>
创建并切换到新的分支，在新的分支上提交修改，切换回master分支，修改文件内容并提交，此时将新的分支合并到master分支上会产生冲突，git status可以查看冲突的文件，此时重新修改文件内容，重新提交，git log --graph可以查看分支合并图，最后删除分支。

合并分支时，Git会使用Fast forward模式，该模式下删除分支后会丢掉分支信息。强制禁用Fast forward模式，Git会在merge时生成一个新的commit，从分支历史上就可看出分支信息。git merge --no-ff -m “ ” <fenzhi>
合并分支时，加上--no-ff参数就可以用普通模式合并，合并后的历史有分支，能看出来曾经做过合并，而fast forward合并就看不出来曾经做过合并。
未被追踪的文件:指的是新建的文件或文件夹且还没加入到暂存区(新建的还没有被git add 过得) 未加入到暂存区的文件:指的是已经被追踪过，但是没有加入到暂存区(已经执行过git add/commit的但是这次修改后还没有git add) 

丢弃一个没有被合并过的分支，通过git branch -D <name>强制删除


