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



## 04. 仓库重装

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

这个"重装"会把整个仓库都重装.



## 05. 地板理论

明面上的文件夹像是仓库地板上的货物, 我们只能知道有木有, 但并不关心发不发生变化.

所以假如向文件夹里扔进一个新文件, 用`git status`只能知道多了一个文件, 你对这个文件做什么了, git 并不关心.

用`git add`添加这个文件后, 就好比把货物放在了货架上, 里面发生了什么变化, 管家都会一一记录下来.

最后用`git commit`就是让老板签字, 同意记录下所有的修改.

这之中:

- 地板就是工作区
- 货架就是缓存区
- 老板的签字就是写进 git 的记录.

而且, 更神奇的是, git 的管理理念可以这么理解.

在地板上处理货物, add 上货架, 这个时候又想改一下货物, 那么改完之后, 还是需要 add 上货架, 才能被记录下来.

也就是说, 多次修改, 必须要按照这个流程:

```
修改1 --> add --> 修改2 --> add --> commit
```

如果没有第二次的 add 的话:

```
修改1 --> add --> 修改2 --> commit
```

最后被 commit 的, 只是修改1, 因为改了之后没有像管家备案, 所以老板也不知道这货物改过了.



## 06. 货品重装

有时候只是仓库里的单个货品出错了, 我们用超能力把它回溯回去, 这时就用不着用`git reset`这种大手笔, 用`git checkout`就可以了.

首先要看看, 修改有没有被记录到管家的小本本上:(有没有 add 过)



- 没有:

```bash
$ git checkout -- 后悔药.txt
```

就会把文件恢复到上次 commit 之后(上个版本库)的状态;



- 有: 分两种情况

  - 恢复到管家本本上记录的状态:

    同上: `git checkout`

  - 恢复到上次老板封存的状态: (上个版本库的状态)

    需要先把它从管家的小本本上抹掉:

    ```bash
    $ git reset HEAD 要抹掉的.txt
    ```

    然后再`git checkout`

> 归根到底, `git checkout`的作用就是把文件恢复到上个储存点, 优先从缓存区(管家的本本)恢复, 缓存区没有的话, 从储存区(老板那)恢复.



>长工:
>
>​	修整货物, 将修好的货物告诉 (add) 管家(缓存区)记录.
>
>管家:
>
>​	记录每次货品的状况, 往小本本上记录变动(diff), 完事后给老板签字; 涂抹掉小本本上的内容(`git reset HEAD 要抹掉的.txt`)
>
>老板:
>
>​	对货物进行封存(commit); 万一办砸了, 把上一车东西拉回来(git reset), 对单件物品进行返厂(git checkout)



## 07. 注销货品

当把一个文件删除之后, 同样需要告诉 Git 消除这个文件的有关记录.

```bash
git rm 删除了的文件.txt
```

 然后 commit.

万一要是删错了, 别忘了用这个

```bash
git checkout -- 删错了的文件.txt
```









