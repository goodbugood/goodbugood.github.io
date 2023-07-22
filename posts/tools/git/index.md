# Git使用命令手册大全


## 命令手册

### 分支操作

#### 查看分支

```sh
查看本地分支
git branch

查看远程分支
git branch -r

查看所有分支，包含本地和远程
git branch -a
```

#### 切换分支

```sh
git checkout branchName
```

#### 创建并切换分支

```sh
git checkout -b newBranchName
```

##### 推送本地创建的分支到远程仓库

```sh
git push origin newBranchName
```

##### 推送本地分支代码到远程分支

也就是将当前本地分支与远程仓库分支建立关联关系。

```sh
git push --set-upstream origin newBranchName
```

#### 删除本地分支

```sh
git branch -d branchName
```

#### 创建远程分支

```sh
git push origin --set-upstream newBranchName
```

#### 删除远程分支

```sh
git push origin --delete branchName
```
