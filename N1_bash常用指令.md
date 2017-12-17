# bash 常用指令



## 01. Mac上隐藏显示文件

- 显示隐藏文件

```bash
$ defaults write com.apple.Finder AppleShowAllFiles YES
```

- 隐藏隐藏文件

```bash
$ defaults write com.apple.Finder AppleShowAllFiles NO
```

然后按着`Alt`键右键 Finder 重启.



## 02. 返回主目录

- 在我 mac 上也就是返回`/Users/fan`

```bash
$ cd ~
```

`~`就表示根目录的意思



## 03. 删除文件

```bash
rm 要删除的文件.txt
```

