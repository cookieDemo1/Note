###### 1停止追踪文件

```bash
git rm --cached src/views/index.vue
```

###### 2.切换分支

```bash
git checkout dev
```

###### 3.查看本地分支

```bash
git branch
```

###### 4.查看远程分支

```bash
git branch -a
```

###### 5.创建本地分支，并推送到远程(远程自动创建分支)

```bash
# 创建本地分支
git checkout -b dev

git add .

git commit -m '首次提交dev分支'

# 推动到远程，远程自动创建dev分支
git push -u --set-upstream origin dev

```

