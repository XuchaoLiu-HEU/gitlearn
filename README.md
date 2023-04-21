# 1.基本流程

```shell
git init
git add filename
git commit -m "xxx"
git status
git remote add origin git@server-name:path/repo-name.git
git push -u origin master
git push origin master
```

```shell
1.查看分支
	git branch
2. 创建+切换分支
	git branch name
	git switch name
	或者 git switch -c name
3. 合并某分支到当前分支
	git merge name
4. 删除分支
	git branch -d name
```



# 2.基本命令

```shell
1. 初始化
	git init   // 将文件夹初始化为git可以管理的仓库

2. 添加文件到Git仓库
	git add 文件名		// 可反复多次使用 git add把文件添加进去，实际上就是把文件修改添加到暂存区
	git commit -m "xxx" // git commit提交更改，实际上就是把暂存区的所有内容提交到当前分支
	
3. 要随时掌握工作区的状态，使用git status命令。
	git status

4. 如果git status告诉你有文件被修改过，用git diff可以查看修改内容。
	git diff 文件名

5. 版本回退
	// HEAD指向的版本就是当前版本,上一个版本就是`HEAD^`，上上一个版本就是`HEAD^^`
	git reset --hard commit_id  
    
    git log  // 可以查看提交历史，以便确定要回退到哪个版本
    git log --pretty=oneline  // 减少参数显示
    // git log --graph --pretty=oneline --abbrev-commit
    git reflog  // 要重返未来，用git reflog查看命令历史，以便确定要回到未来的哪个版本
```



>·  git add -A  提交所有变化
>
>·  git add -u  提交被修改(modified)和被删除(deleted)文件，不包括新文件(new)
>
>·  git add .  提交新文件(new)和被修改(modified)文件，不包括被删除(deleted)文件



# 3.工作区和版本库

> 版本库里主要有 暂存区（stage）、分支（master)、

<img src="https://www.liaoxuefeng.com/files/attachments/919020037470528/0" alt="git-repo" style="zoom: 150%;" />

> 把文件往Git版本库里添加的时候，是分两步执行的：
>
> * 第一步是用`git add`把文件添加进去，实际上就是把文件修改添加到暂存区；
>
> * 第二步是用`git commit`提交更改，实际上就是把暂存区的所有内容提交到当前分支。
>
> 因为我们创建Git版本库时，Git自动为我们创建了唯一一个`master`分支，所以，现在，`git commit`就是往`master`分支上提交更改。
>
> 你可以简单理解为，需要提交的文件修改通通放到暂存区，然后，一次性提交暂存区的所有修改。

# 4.Git管理修改

> 第一次修改 -> `git add` -> 第二次修改 -> `git add` -> `git commit`

> Git管理的是修改，当你用`git add`命令后，在工作区的第一次修改被放入暂存区，准备提交，但是，在工作区的第二次修改并没有放入暂存区，所以，`git commit`只负责把暂存区的修改提交了，也就是第一次的修改被提交了，第二次的修改不会被提交。

# 5.撤销修改

```shell
git checkout -- 文件名
git reset HEAD 文件名
```



* 场景1：当你改乱了工作区某个文件的内容，想直接丢弃工作区的修改时，用命令`git checkout -- file`。

* 场景2：当你不但改乱了工作区某个文件的内容，还添加到了暂存区时，想丢弃修改，分两步，第一步用命令`git reset HEAD <file>`，就回到了场景1，第二步按场景1操作。

* 场景3：已经提交了不合适的修改到版本库时，想要撤销本次提交，参考版本回退，不过前提是没有推送到远程库。

# 6.删除文件

在Git中，删除也是一个修改操作

一般情况下，你通常直接在文件管理器中把没用的文件删了，或者用`rm`命令删了：

```shell
rm 文件名
git add 文件名
那么需要 git rm 文件名

```

另一种情况是删错了，因为版本库里还有呢，所以可以很轻松地把误删的文件恢复到最新版本：

```shell
git checkout -- 文件名
```

# 7.关联远程库

```shell
1. 要关联一个远程库，使用命令
	git remote add origin git@server-name:path/repo-name.git
2. 关联一个远程库时必须给远程库指定一个名字，origin是默认习惯命名；
3. 关联后，使用命令 git push -u origin master 第一次推送master分支的所有内容；
	git push -u origin master
此后，每次本地提交后，只要有必要，就可以使用命令git push origin master推送最新修改；
git push origin master
```

# 8.分支管理

```shell
1.查看分支：git branch

2.创建分支：git branch <name>

3.切换分支：git checkout <name>或者git switch <name>

4.创建+切换分支：git checkout -b <name>或者git switch -c <name>

5.合并某分支到当前分支：git merge <name>
	git merge --no-ff -m "merge with no-ff" dev
	合并分支时，加上--no-ff参数就可以用普通模式合并，合并后的历史有分支，能看出来曾经做过合并，而fast 	   forward合并就看不出来曾经做过合并。

6. git log --graph命令可以看到分支合并图

7.删除分支：git branch -d <name>

```

 **bug分支**

> 修复bug时，我们会通过创建新的bug分支进行修复，然后合并，最后删除；
>
> 当手头工作没有完成时，先把工作现场`git stash`一下，然后去修复bug，修复后，再`git stash pop`，回到工作现场；
>
> 在master分支上修复的bug，想要合并到当前dev分支，可以用`git cherry-pick <commit>`命令，把bug提交的修改“复制”到当前分支，避免重复劳动。

# 9.多人协作

>1.  首先，可以试图用`git push origin <branch-name>`推送自己的修改；
>2.  如果推送失败，则因为远程分支比你的本地更新，需要先用`git pull`试图合并；
>3.  如果合并有冲突，则解决冲突，并在本地提交；
>4.  没有冲突或者解决掉冲突后，再用`git push origin <branch-name>`推送就能成功！
>
>如果`git pull`提示`no tracking information`，则说明本地分支和远程分支的链接关系没有创建，用命令`git branch --set-upstream-to <branch-name> origin/<branch-name>`。

- 查看远程库信息，使用`git remote -v`；
- 本地新建的分支如果不推送到远程，对其他人就是不可见的；
- 从本地推送分支，使用`git push origin branch-name`，如果推送失败，先用`git pull`抓取远程的新提交；
- 在本地创建和远程分支对应的分支，使用`git checkout -b branch-name origin/branch-name`，本地和远程分支的名称最好一致；
- 建立本地分支和远程分支的关联，使用`git branch --set-upstream branch-name origin/branch-name`；
- 从远程抓取分支，使用`git pull`，如果有冲突，要先处理冲突。

# 10.标签管理

- 命令`git tag <tagname>`用于新建一个标签，默认为`HEAD`，也可以指定一个commit id；
- 命令`git tag -a <tagname> -m "blablabla..."`可以指定标签信息；
- 命令`git tag`可以查看所有标签。

----------------------

- 命令`git push origin <tagname>`可以推送一个本地标签；
- 命令`git push origin --tags`可以推送全部未推送过的本地标签；
- 命令`git tag -d <tagname>`可以删除一个本地标签；
- 命令`git push origin :refs/tags/<tagname>`可以删除一个远程标签。

# 11. vscode中使用git
> coming soon....
> dev branch