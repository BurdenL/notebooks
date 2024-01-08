### 从命令行创建一个新的仓库

```cmd
touch README.md
git init
git add README.md
git commit -m "first commit"
git remote add origin http://192.168.2.233:3000/liukaien/lautest.git
git push -u origin master
```

### 从命令行推送已经创建的仓库

```cmd
git remote add origin http://192.168.2.233:3000/liukaien/lautest.git
git push -u origin master
```





### 在 Git 中，你可以使用以下命令来创建一个新的分支：

```
bash
git branch <branch_name>
```

这会在当前所在的分支上创建一个新的分支，但并不会切换到新的分支。如果你想要在创建分支的同时切换到这个新分支上，可以使用以下命令：

```
bash
git checkout -b <branch_name>
```

或者在 Git 2.23 版本及更高版本中，可以使用 `git switch` 命令：

```
bash
git switch -c <branch_name>
```

这两个命令都会创建一个新分支并立即切换到这个新分支上。

如果你使用的是 Git 2.28 版本及更高版本，你还可以使用 `git restore` 命令：

```
bash
git restore --source=<source_branch> --worktree --switch <branch_name>
```

其中，`<source_branch>` 是新分支的源分支，`<branch_name>` 是新分支的名称。此命令会创建一个新的分支，并将工作目录切换到新分支上。



### 在 Git 中，当你在本地新建了一个分支后，如果想要将这个分支推送到远程服务器，可以使用以下命令：

```
bash
git push -u origin <branch_name>
```

这条命令的含义是将本地的 `<branch_name>` 分支推送到远程仓库，并在远程仓库上创建同名的分支。`-u` 选项会将本地的 `<branch_name>` 分支设置为远程仓库的默认上游分支，这样后续的 `git pull` 和 `git push` 命令就可以省略远程分支名。

如果你使用的是 Git 2.28 版本及更高版本，你还可以使用 `git branch` 命令的 `-vv` 选项来设置默认上游分支：

```
bash
git branch -u origin/<branch_name> <branch_name>
```

上述命令的作用同样是将本地 `<branch_name>` 分支推送到远程仓库，并设置为默认上游分支。

请确保在推送之前，你已经添加了远程仓库。如果没有添加远程仓库，可以使用以下命令添加：

```
bash
git remote add origin <remote_repository_url>
```

其中 `<remote_repository_url>` 是你远程仓库的 URL 地址。完成这一步后，再执行推送命令。