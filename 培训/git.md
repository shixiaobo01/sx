```
# 设置name和email 
git config --global user.name "yourname"

git config --global user.email "youremail"

#查看git配置
git config --list

# 查看提交日志
git log
# 一行模式输出
git log --pretty=oneline

# 记录每一次命令
git reflog

# 查看工作区和版本库里面最新版本的区别
git diff HEAD -- <file>

# 把文件在工作区的修改全部撤销
(命令git checkout -- readme.txt意思就是，把readme.txt文件在工作区的修改全部撤销，这里有两种情况：

一种是readme.txt自修改后还没有被放到暂存区，现在，撤销修改就回到和版本库一模一样的状态；
一种是readme.txt已经添加到暂存区后，又作了修改，现在，撤销修改就回到添加到暂存区后的状态。
总之，就是让这个文件回到最近一次git commit或git add时的状态。）
<<<<<<< HEAD

git checkout -- <file>

#把暂存区的修改撤销掉（unstage），重新放回工作区

git reset HEAD <file>

#用HEAD表示当前版本DEAD^^ DEAD~100
git reset --hard HEAD^

#回退到指定版本

git reset --hard commitId(具体版本的id)



#确实要从版本库中删除该文件
git rm删掉，并且git commit
#删错了，把误删的文件恢复到最新版本
git checkout -- <file>
```

对接gitlab
```
#创建key
ssh-keygen -t rsa -C "youremail@example.com"

#要关联一个远程库，使用命令
git remote add origin git@server-name:path/repo-name.git

#查看远端库信息
git remote -v

#克隆一个库
git clone 
```
分支
```
#创建dev分支，然后切换到dev分支（git checkout命令加上-b参数表示创建并切换）

git checkout -b dev
#创建并切换
git branch dev
git checkout dev
#查看当前分支
git branch
git branch -a

#把dev分支的工作成果合并到master分支
git checkout master
git merge dev (合并某分支到当前分支)

git merge --no-ff -m "merge with no-ff" dev

合并分支时，加上--no-ff参数就可以用普通模式合并，合并后的历史有分支，能看出来曾经做过合并，而fast forward合并就看不出来曾经做过合并

# 根据某个分支创建新分支
git checkout -b test  master
#推送新分支到远端的某分支
git push -u  origin test



#删除dev分支
git branch -d dev
```


git 交互式学习闯关游戏
```
https://learngitbranching.js.org/

```