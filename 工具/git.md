### git本地库

##### 1.本地库初始化

```bash
# 初始化项目
git init

# 设置签名,和git,gitee的账户没有任何关系(可省略)
# --global系统级别，不加则是项目级别
git config --global user.name "huangcaiping"
git config --global user.email "2227874784@qq.com"

```

##### 2.创建仓库并push

```bash
git init
git add .
git commit -m "first commit"
git remote add origin https://gitee.com/nicesdfs/LearnGit.git
git push -u origin master
```

##### 3.已有仓库并推送

```bash
git init
git commit -m "first commit"
git remote add origin https://gitee.com/nicesdfs/LearnGit.git
git push -u origin master
```

##### 4.添加提交以及查看状态操作

```bash
# 1.查看状态
git status

# 2.追踪状态,添加到暂存区
git add .

# 3.不追踪状态，从暂存区删除，即撤销上一步操作
git rm --cached

# 4.提交，从暂存区提交到本地库
git commit -m 'firs commit"

# 5.已经add的文件修改后，可以直接commit
git commit -m 'second commit'
```

##### 5.版本前进和后退

```bash
# 1.查看版本历史记录
git log

# 2.一行只显示一条提交记录
git log --oneline

# 3.查看记录的时候，显示指针
git reflog

# 4.版本前进后退，先使用reflog查看状态
git reflog
git reset --hard 6af1c3a
```

##### 6.查看文件差异

```bash
# 将本地文件和暂存区进行比较
git diff a.txt

# 将暂存区和本地库进行比较
git diff HEAD a.txt
```

##### 7.分支管理

- 分支开发彼此独立，不影响master分支
- 分支完成之后可以和master分支合并

```bash
# 查看所有的分支
git branch -v 

# 创建分支
git branch hot_fix

# 分支切换，对a.txt文件进行修改
git checkout hot_fix

# 必须在接受修改的分支上进行合并，即master分支
git checkout master
# 使用merge和hot_fix分支进行合并
git merge hot_fix
```

##### 8.解决合并分支后产生的冲突

- 当两个合并的分支，都修改了同一个文件的同一个内容，就会产生冲突

```bash
# 修改同一个文件a.txt的同一行，在hot_fix进行合并
git merge master

# 修改文件之后git add a.txt
git add a.txt

# git commit进行完成合并,commit的时候不能带文件名
git commit -m "nice"
```



### git远程库

##### 1.推送

```bash
# 1.git remote 关联远程库
# add添加，origin是为后面一长串地址起的别名
git remote add origin https://gitee.com/nicesdfs/LearnGit.git

# 2.查看remote
git remote -v

# 2.git push推送，origin为要推送仓库的别名,master为要推送的分支(本地分支)
git push origin master
```

##### 2.下载远程库的代码

```bash
# git clone
git clone https://gitee.com/nicesdfs/LearnGit.git
```

##### 3.pull操作的分解 fetch  merge

```bash
# 1.fetch之后文件并没有改变
git fetch origin master

# 切换到fetch的远程库，看看修改了什么内容
git checkout origin/master
cat a.txt

# 2.切换回本地库master,和远程库的master合并
git checkout master
git merge origin/master
```

##### 4.pull操作( pull = fetch + merge )

```bash
# fetch + merge,数据比较简单使用pull
git pull origin master
```

##### 5.协同开发解决冲突

- 如果不是基于远程库的最新版所做的修改，不能推送，必须先拉取，进入冲突状态，则按照分支冲突解决修改即可，修改完成之后重新推送。

```bash
# 当两个人修改同一个地方的代码，push的时候，只有前面那个人可以push上去，后面那个人就需要先拉取下来，然后修改内容，再push

# 后面那个人推送不上去的操作
# 1.先pull，拉取下来之后是merger状态
git pull origin master

# 2.修改冲突的文件,并commit
vim a.txt
git add .
git commit -m 'jiejuechongtu'

# 3.推送到远程
git push origin master
```





