以后日常同步：

bash

运行

```
git add .
git commit -m "更新：xxx"
git push
```



# 我给你一个【万能同步命令】

以后你**只需要复制这 3 行**，就能一键同步：

bash

运行

```
cd ~/文档/Ubuntu\ Obsidian
git add .
git commit -m "更新笔记"
git push
```


# 详细版本
### 1. 看这次更新**到底改了什么文件**

```
git show
```
### 2. 想看**任意一次历史的具体修改内容**

先用这个看简洁历史：

```
git log --oneline
```

## ✔ 你现在最常用的就 3 个命令


1. **看历史

```
git log --oneline
```


1. **看这次改了啥
```
git show
```


1. **看任意历史版本**

```
git show 提交号
```

## 1. 你现在遇到的报错：文件锁 index.lock

这是因为你刚才用了 **&** 符号，让 Git 同时运行两个命令，导致冲突了。

## 解决方法（只需要 1 条命令）

bash

运行

```
rm -f .git/index.lock
```

运行完就恢复正常了！