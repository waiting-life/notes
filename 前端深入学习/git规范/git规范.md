# git 规范

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


```

