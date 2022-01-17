git 规范

### 1. 全局安装commitizen & cz-conventional-changelog

```shell
npm install -g commitizen cz-conventional-changelog
echo '{ "path": "cz-conventional-changelog" }' > ~/.czrc
```

### 2. 执行git cz

```shell
git cz
```

![image-20210809202612140](/Users/wangqiaojuan/Library/Application Support/typora-user-images/image-20210809202612140.png)

**注意**：`Are there any breaking changes?`有什么有破坏性的改变吗？

### 3. push到feature_v1.0.4分支上去

```shell
git push
```

### 4. 切换到develop分支上面

```shell
git checkout develop
```

### 5. 在develop分支上合并feature_v1.0.4

```shell
git merge feature_v1.0.4
```

### 6. 然后在develop分支上push代码

```shell
git push
```

### 7. 在Jenkins上面点击构建

### 8. 切换到feature_v1.0.4分支上继续开发代码

（在测试环境测试完发给）

![image-20210809203406038](/Users/wangqiaojuan/Library/Application Support/typora-user-images/image-20210809203406038.png)



### 扩展

```shell
npm list -g --depth=0
git stash
git status

```



创建分支： git branch 分支名

```shell
git push -u origin 分支名称
```





```shell
// 提交代码
// feature
git pull
git add .
git cz
git push

git checkout develop
// develop
git pull
git merge feature
git push


// git stash可以回到修改之前
git stash
// 回到修改之后可以通过
// 1. git list查看stash对应的stash
git stash list
// 删除对应的缓存栈
git stash pop 0
// 如果写了一堆没用的代码，可以通过git stash直接回到之前，然后清空stash栈
git stash clear
```

```shell
pwd   // pwd用于显示当前目录
ls -ah	// 可以查看到默认隐藏文件
git add // 告诉git，把文件添加到仓库
git commit // 告诉git，把文件提交到仓库
git diff   // 看看具体修改了什么内容，比较文件

// 在执行第二步commit之前，运行git status看看当前仓库的状态
git status
// 提交后，再用git status查看仓库的当前状态


// 版本回退
// HEAD表示当前版本
// 上一个版本就是HEAD^
// 上上一个版本就是HEAD^^
// 往上100个版本写100个^比较容易数不过来，所以写成HEAD~100
// 回退到上一个版本
git reset --hard HEAD^
// git reset --hard commit_id

// 可以丢弃工作区的修改
git checkout -- file


```

ssh://git@172.18.9.35:10022/web/bdw-admin.git

[39.108.148.175:](http://39.108.148.175:)

```shell

```





```shell
删除分支
// 删除本地分支
// delete branch locally
git branch -d localBranchName

// 删除远程仓库分支
// delete branch remotely
git push origin --delete remoteBranchName

// 删除远程分支时如果远程分支和标签名重复删除会出错，可以通过下面方式
git push origin --delete refs/heads/release/v1.1.0_1227
```



+ 把文件往git版本库里面添加的时候，是分两步之行的
  + 第一步是用git add把文件添加进去，实际上就是把文件添加到暂存区。
  + 第二步是用git commit提交更改，实际上就是把暂存区的内容提交到当前分支。

**可以简单理解为需要提交的文件修改统统放到暂存区，然后，一次性提交暂存区的所有修改**



#### 工作区

就是你在电脑里能看到的目录

#### 暂存区

git add把文件添加进去，实际上就是把文件添加到暂存区。



```shell
# 添加一个test.ts文件
git status

On branch main
Your branch is up to date with 'origin/main'.

Untracked files: # test.ts文件还没有被添加(git add)过，所以状态是Untracked
  (use "git add <file>..." to include in what will be committed)
        src/utils/test.ts
        
nothing added to commit but untracked files present (use "git add" to track)


# 然后git add再git status
git add .
git status

On branch main
Your branch is up to date with 'origin/main'.

Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        new file:   src/utils/test.ts


# 执行git commit再git status
git commit -m 'test.ts'

On branch main
Your branch is ahead of 'origin/main' by 1 commit.
  (use "git push" to publish your local commits)

nothing to commit, working tree clean

# 执行git push再git status
git push
git status

On branch main
Your branch is up to date with 'origin/main'.

nothing to commit, working tree clean
```





```shell
# 对test.ts做修改
const p = {
    name: 'cpp',
    age: 23
}
console.log(p)  # 修改，新增一行

git add src/utils/test.ts 
git status

On branch main
Your branch is up to date with 'origin/main'.

Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        modified:   src/utils/test.ts

# 然后对test.ts再做修改
const p = {
    name: 'cpp',
    age: 23
}
console.log(p)
const lg = {
    name: 'cjz',
    age: 24
}
# 直接git commit提交
git commit -m '测试'
git status

On branch main
Your branch is ahead of 'origin/main' by 1 commit.
  (use "git push" to publish your local commits)

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   src/utils/test.ts

no changes added to commit (use "git add" and/or "git commit -a")

# git commit只负责把暂存区的修改提交了，也就是第一次的修改被提交了，第二次的修改不会被提交。因为第二次的修改没有通过git add 加入暂存区
```



#### 撤销修改

```shell
git checkout -- filename # 可以丢弃工作区的修改

git checkout -- src/utils/test.ts
# 意思就是把src/utils/test.ts文件在工作区的修改全部撤销
# 这里有两种情况，一种是src/utils/test.ts自修改后还没有被放到暂存区，现在，撤销修改就回到和版本库一模一样的状态
# 一种是src/utils/test.ts已经添加到暂存区，又做了修改，撤销修改就是回到添加到暂存区后的状态
# 总之，就是让这个文件回到最近一次git commit或git add时的状态。


# 一. 还没有添加到暂存区
# test.ts修改后未git add
const p = {
    name: 'cpp',
    age: 23
}
console.log(p)
const lg = {   # 新加，还没git add
    name: 'cjz',
    age: 24
}
git checkout -- src/utils/test.ts
# test.ts
const p = {
    name: 'cpp',
    age: 23
}
console.log(p)


# 二. 部分添加到暂存区，部分没有
# test.ts修改后git add
const p = {
    name: 'cpp',
    age: 23
}
console.log(p)
const lg = {   # 新加，已经git add
    name: 'cjz',
    age: 24
}
git add .
git status

On branch main
Your branch is ahead of 'origin/main' by 1 commit.
  (use "git push" to publish your local commits)

Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        modified:   src/utils/test.ts

# 此时再做修改，增加console.log(lg.name)
# 然后再撤销git checkout -- src/utils/test.ts
git checkout -- src/utils/test.ts
# 文件就又复原了
# test.ts
const p = {
    name: 'cpp',
    age: 23
}
console.log(p)
const lg = {   
    name: 'cjz',
    age: 24
}

# 三. 已经git add 添加到缓存去了，但是还没有git commit
# 命令git reset HEAD <file>可以把暂存区的修改撤销掉（unstage），重新放回工作区
# test.ts
const p = {
    name: 'cpp',
    age: 23
}
console.log(p)
const lg = {    # 新加，已经git add
    name: 'cjz',
    age: 24
}

git status

On branch main
Your branch is ahead of 'origin/main' by 1 commit.
  (use "git push" to publish your local commits)

Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        modified:   src/utils/test.ts
        
git reset HEAD src/utils/test.ts
git status # 回到未git add之前了

// git reset 命令主要有三个选项： --soft、--mixed 、--hard，默认参数为 --mixed。

On branch main
Your branch is ahead of 'origin/main' by 1 commit.
  (use "git push" to publish your local commits)

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   src/utils/test.ts

no changes added to commit (use "git add" and/or "git commit -a")
# 然后再git checkout -- filename
git checkout -- src/utils/test.ts

# test.ts
const p = {
    name: 'cpp',
    age: 23
}
console.log(p)

```



`git reset` 命令主要有三个选项：` --soft`、`--mixed` 、`--hard`，默认参数为` --mixed`。

+ --soft 操作是软重置，只撤销了`git commit`操作，保留了 `git add` 操作。
+ `--hard` 是具有破坏性，是很危险的操作，它很容易导致数据丢失，如果我们真的进行了该操作想要找回丢失的数据，那么此时可以使用`git reflog` 回到未来，找到丢失的commit
+ --mixed 参数会保留提交的源码改动，只是将索引信息回退到了某一个版本，如果还需要继续提交，再次执行 `git add` 和 `git commit`

#### 删除文件

在git中，删除也是一个修改操作

**注意**：

1. `git checkout`其实是用版本库里的版本替换工作区的版本，无论工作区是修改还是删除，都可以“一键还原”
2. 从来没有被添加到版本库就被删除的文件，是无法恢复的！
3. 命令`git rm`用于删除一个文件。如果一个文件已经被提交到版本库，那么你永远不用担心误删，但是要小心，你只能恢复文件到最新版本，你会丢失**最近一次提交后你修改的内容**。



```shell
rm src/utils/test.ts
# 这个时候，Git知道你删除了文件，因此，工作区和版本库就不一致了，git status命令会立刻告诉你哪些文件被删除了：

On branch main
Your branch is ahead of 'origin/main' by 1 commit.
  (use "git push" to publish your local commits)

Changes not staged for commit:
  (use "git add/rm <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        deleted:    src/utils/test.ts

no changes added to commit (use "git add" and/or "git commit -a")

git checkout -- src/utils/test.ts
# 恢复误删文件
```



#### 远程仓库

##### 删除远程仓库

+ 如果添加的时候地址写错了，或者就是想删除远程库，可以用git remote rm <name>命令。使用前，建议先用git remote -v查看远程库信息：

```shell
git remote -v # 查看远程库信息
git remote rm <name> # 删除远程库
```

此处的“删除”其实是解除了本地和远程的绑定关系，并不是物理上删除了远程库。远程库本身并没有任何改动。要真正删除远程库，需要登录到GitHub，在后台页面找到删除按钮再删除。

Git鼓励大量使用分支：

```shell
git branch # 查看分支
git branch <name> # 创建分支
git checkout <name>   git switch <name>  # 切换分支
git checkout -b <name>   git switch -c <name>  # 创建+切换分支
git merge <name> # 合并某分支到当前分支
git branch -d <name>  # 删除本地分支

```

通常，合并分支时，如果可能，Git会用`Fast forward`模式，但这种模式下，删除分支后，会丢掉分支信息。

如果要强制禁用`Fast forward`模式，Git就会在merge时生成一个新的commit，这样，从分支历史上就可以看出分支信息。

```shell
# dev分支上增加函数调用这一行

# test.js
console.log(111)
console.log(2222)
console.log(444)
console.log(555)
console.log('创建新的feature分支')
console.log('测试分支合并')
function testNoff() {
    console.log('--no-ff')
}
testNoff() # 新增


git add .
git commit -m '继续测试--no-ff'
git switch main 
git merge --no-ff -m '合并dev分支，测试--no-ff' dev 
git push
git log

```

![image-20211207100758579](/Users/wangqiaojuan/Library/Application Support/typora-user-images/image-20211207100758579.png)

#### bug分支

每个bug都可以通过一个新的临时分支来修复，修复后，合并分支，然后将临时分支删除。

+ `dev`上进行的工作还没有提交
+ Git还提供了一个`stash`功能，可以把当前工作现场“储藏”起来，等以后恢复现场后继续工作
+ 首先确定要在哪个分支上修复bug，假定需要在`master`分支上修复，就从`master`创建临时分支：





```shell
# *dev分支进行需求开发，但是任务还没完成还不能提交，使用git stash
# test.js
git stash
# 确定要在main分支修复bug，切换到main分支上，从main分支创建临时分支
git switch main
# 创建一个bug分支
git checkout -b issue-101
# 修改完bug后
$ git add .
$ git commit -m "fix bug 101" # fb8787a
# 切换到main分支，并完成合并
git switch main
git merge --no-ff -m 'merged bug fix 101' issue-101
# 切换到dev分支继续工作
git switch dev
git status # 工作区是干净的，刚才的工作现场存到哪去了
On branch dev
nothing to commit, working tree clean

git stash list  # 命令查看
# 工作现场还在，Git把stash内容存在某个地方了，但是需要恢复一下

# 方法一
git stash apply # 用git stash apply恢复，但是恢复后，stash内容并不删除，你需要用git stash drop来删除

# 方法二
git stash pop # 恢复的同时把stash内容也删了：

# 你可以多次stash，恢复的时候，先用git stash list查看，然后恢复指定的stash，用命令：
git stash apply stash@{0}

# 在master分支上修复了bug后，我们要想一想，dev分支是早期从master分支分出来的，所以，这个bug其实在当前dev分支上也存在。

# 同样的bug，要在dev上修复，我们只需要把fb8787a fix bug 101这个提交所做的修改“复制”到dev分支。意：我们只想复制fb8787a fix bug 101这个提交所做的修改，并不是把整个master分支merge过来。
# 为了方便操作，Git专门提供了一个cherry-pick命令，让我们能复制一个特定的提交到当前分支：


git stash list
git stash pop
git stash list



git cherry-pick fb8787a
git stash pop

git log --graph
git log --graph --pretty=oneline --abbrev-commit



```

+ 如果要丢弃一个没有被合并过的分支，可以通过`git branch -D <name>`强行删除。

- 建立本地分支和远程分支的关联，使用`git branch --set-upstream branch-name origin/branch-name`；

  

#### git rebase



#### 标签管理

+ 发布一个版本时，我们通常先在版本库中打一个标签（tag）

在Git中打标签非常简单，首先，切换到需要打标签的分支上：

```shell
git tag v1.0
# 可以用命令git tag查看所有标签：
git tag

# 默认标签是打在最新提交的commit上的。有时候，如果忘了打标签，比如，现在已经是周五了，但应该在周一打的标签没有打，怎么办？
# 方法是找到历史提交的commit id，然后打上就可以了：


```



```shell
git log --graph --pretty=oneline --abbrev-commit
git rebase

git tag
git tag v1.0.0
git log --pretty=oneline --abbrev-commit

# 想对d722491commit打tag
git tag v0.0.9 d722491
git tag
git log --pretty=oneline --abbrev-commit

# 还可以创建带有说明的标签，用-a指定标签名，-m指定说明文字：
git tag -a v1.0.0 -m "version 1.0.0 released" 1094adb

# 将标签推送到远程
git push origin v0.0.9
# 如果标签打错了，也可以删除：
git tag -d v1.0.0
git tag # 查看本地tag
# 如果标签已经推送到远程，要删除远程标签就麻烦一点，先从本地删除：
git tag -d v0.0.9
git tag # 查看本地tag
git push origin --tags # 可以推送全部未推送过的本地标签
# 然后，从远程删除。删除命令也是push，但是格式如下：
git push origin :refs/tags/v0.0.9


git branch -a
   dev
 * main
   remotes/origin/main
   remotes/origin/test    # 其中remote/origin/xxx是远程分支
   remotes/origin/test2
   remotes/origin/test3
```



![image-20211207112702258](/Users/wangqiaojuan/Library/Application Support/typora-user-images/image-20211207112702258.png)

+ 标签总是和某个commit挂钩。如果这个commit既出现在master分支，又出现在dev分支，那么在这两个分支上都可以看到这个标签

```shell
查看本地分支和追踪情况: git remote show origin
同步删除远端已删除分支: git remote prune origin
```



1. 旧分支：oldBranch
2. 新分支：newBranch

> 步骤：

1. 先将本地分支重命名

   ```shell
   git branch -m oldBranch newBranch
   ```

2. 删除远程分支（远端无此分支则跳过该步骤）

   ```shell
   git push --delete origin oldBranch
   ```

3. 将重命名后的分支推到远端

   ```shell
   git push origin newBranch
   ```

4. 把修改后的本地分支与远程分支关联

   ```shell
   git branch --set-upstream-to origin/newBranch
   ```



```shell
git remote set-url origin ssh://git@39.108.148.175:10022/web/admin/bdw-admin-member.git
```

