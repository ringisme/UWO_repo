# Git基础

> 这节主要讲如何用本地化的 Git 系统的。



## 01. 本地建仓

先从 shell 里面导航到想要监控的文件夹路径下：

```
pwd (查看当前路径)
cd /A/B (转入到 A/B/ 下)
cd .. (返回到上一级菜单)

ls (查看当前路径下文件)
ls -ah (查看当前路径下所有文件,包含隐藏)
```

好了之后, 将此目录建仓:

```bash
$ git init
Initialized empty Git repository in /A/B/.git/
```

是否成功就看此目录下有没有一个`.git`的隐藏文件就好了.



## 02. 送货上架

在本地仓库里面添加文件的话, 就像是往地板上扔东西, Git 并不会记录地板上东西的变动.

先要 Git 对这个新建的文件追踪, 需要告诉 Git, 把东西放到货架上:

```bash
$ git add 新文件.txt

# 也可以一次 add 多个文件
$ git add 新文件1.txt 新文件2.txt
```

 成功的话会没有任何反馈.

每次添加新文件之后, 最好在 commit 一下, 告诉 Git 这次都干了啥:

```bash
$ git commit -m "我这次加了个新文件进去"
成功的话会显示类似于以下语句:
[master (root-commit) cb926e7] 我这次加了个新文件进去
 1 file changed, 2 insertions(+) #表示一个文件变动, 两行内容插入.
 create mode 100644 新文件.txt
```

当然也可以 add 多个文件后, 一起 commit

送上去之后, 这个文件的变动就会被 Git 监控了.



## 03. 货品查看

把货物送上架之后, 可以没事过来看看, 哪些货物发生变化了:

```bash
$ git status

On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   00_Git简介.md # 这个文件被修改(modified)过了, 但还没有提交

no changes added to commit (use "git add" and/or "git commit -a")
```

然后看看这些货物都发生什么变化了:

```bash
$ git diff
```

看完之后把货品放回去登记一下

```bash
$ git add 00_Git简介.md
# 可见 add 这个语句有"挂号"的作用
```

然后封存:

```bash
$ git commit -m "做了一些微笑的工作"

[master 19f6643] 做了一些微小的工作
 1 file changed, 57 insertions(+)
```

最后看下地板都是收拾干净没有:

```bash
$ git status
On branch master
nothing to commit, working tree clean #真干净啊!
```

- 任何时候用`git status`看看仓库有无变动
- 用`git diff`查看变动内容



## 04. 货品重装

之前每次倒腾完货物, 都会在小本本上记一笔, 那么怎么查看记录的这些内容呢:

```bash
$ git log

# 也可以用精简模式查看:
$ git log --pretty=oneline

19f6643da18e1d11dcc65ec5cbc6489605115169 (HEAD -> master) 做了一些微小的工作
# 上行中的" HEAD" 表示当前版本.
aeb4689802e549e3af7f34d1a796ff9c531624d6 添加了第一个简介文件上架
```

显示的顺序是从最近到最远. 

前面那一堆字符串被称为`commit id`, 就是 git 中改动的身份标识.

- `HEAD`表示当前版本.
- `HEAD^`表示上个版本, `HEAD^^`就表示上上个版本
- `HEAD~100`就表示之前的第100个版本.



当需要进行时间回溯的时候, 用这个语句:

```bash
$ git reset --hard HEAD^ # 表示回溯到上个版本.
```

如果穿越回去了, 忽然后悔了, 又想穿越回来...

先把窗口往上拉, 找到最后一次版本的`commit id`, 然后同样方法

```bash
$ git reset -- hard 序列号
```

(需要注意的是, 并不需要输入完整的`commit id`, 只要前几位差不多就行, git 会自己查找后面的.)

当然, 如果关闭了 shell 窗口, 忘了回溯前版本的`commit id`的话, 可以"查总账":

```bash
$ git reflog
```

 就可以看到开天辟地以来的所有版本号了.