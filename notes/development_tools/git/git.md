## git



### 初始化本地仓库

==右键-->git bash here==

```git
# 在当前目录初始化一个git本地仓库
git init
```

![image-20221031114533090](git.assets/image-20221031114533090.png)

**执行操作后在learningnotes目录下生成一个.git文件夹**



### 配置提交者信息

==--golbal在电脑的该用户所有仓库都用同样的配置信息==

名称和邮箱都可以随便填，与远程仓库和github没有任何关系

```git
git config --global user.name "kuangshen"  #名称
git config --global user.email 24736743@qq.com   #邮箱
```



### git基本命令

```git
# 显示工作区和暂存区的状态
git status

# 将文件添加到暂存区
git add  文件名.后缀

# 将工作区所有文件添加到暂存区
git add .

# 从工作区和索引中删除文件
git rm  file

# 将指定文件从暂存区提交到本地仓库
git commit -m "提交信息"  文件名.后缀

# 将暂存区所有文件提交到本地仓库
git commit -m "提交信息"

# 将暂存区的文件删除（不删除工作区）
git rm --cached  file

# 查看版本信息
git reflog

# 查看完整版本信息
git log

```

### 版本穿梭

本地仓库保存了所有提交的版本，所以可以放心穿梭而不用担心丢失信息

==本地仓库靠head指针来调整版本==

 ![image-20221031115533594](git.assets/image-20221031115533594.png)

```git
# 查看版本号
git reflog

```

![image-20221031114016073](git.assets/image-20221031114016073.png)

```git
# 穿梭会历史版本
git reset --hard 历史版本号
```



### git命令

|command| means|
|---|---|
|  |                                             |
|  |  |
|  |  |
|  |  |
|  |  |
| git commit  file | 将更改记录提交到本地库(备注信息，Esc， :wq) |
|  |  |
| git commit -m "提交信息" | 提交暂存区所有文件到本地仓库 |
|  |  |
| git checkout file | 恢复本地仓库内容到工作区                    |
|  |  |
| git commit -m '第二次提交' hello.txt | 提交时添加备注信息 |
| git reset head hello.txt | 将本地库的内容写到暂存区 |
| git remote add origin  ssh | 配置远程仓库 |
| git config -l | 查看配置 |
| git branch | 查看本地所有分支 |
| git branch -r | 查看远程所有分支 |
|  |  |
|  |  |

### git分支中常用指令

**分支的好处**

+ 同时并行推进多个功能开发，提高开发效率

+ 各个分支在开发过程中，如果一个分支开发失败，不会对其他分支有任何影响，失败的分支删除重新开始即可
+ ![image-20221031120240704](git.assets/image-20221031120240704.png)

```git
# 列出所有本地分支
git branch

# 列出所有远程分支
git branch -r

# 新建一个分支，但依然停留在当前分支
git branch [branch-name]

# 新建一个分支，并切换到该分支
git checkout -b [branch]

# 合并指定分支到当前分支
$ git merge [branch]

# 删除分支
$ git branch -d [branch-name]

# 删除远程分支
$ git push origin --delete [branch-name]
$ git branch -dr [remote/branch]
```

### git配置

```

```

### linux命令

| command | means                                                        |
| ------- | ------------------------------------------------------------ |
| cd      | 改变目录                                                     |
| cd..    | 退回到上级目录                                               |
| pwd     | 显示当前目录的路径                                           |
| ls(ll)  | 列出当前目录的所有文件                                       |
| touch   | 新建一个文件                                                 |
| rm      | 删除文件                                                     |
| mkdir   | 新建目录                                                     |
| rm -r   | 删除文件夹                                                   |
| mv      | mv index.html src   <br />index.html 是要移动的文件, src 是目标文件夹<br/>必须保证文件和目标文件夹在同一目录下 |
| clear   | 清屏                                                         |
| exit    | 退出                                                         |
|         |                                                              |
|         |                                                              |
|         |                                                              |



### 在远程创建一个仓库并将本地仓库关联到远程仓库

git remote add origin ssh                 配置远程仓库(origin 别名)

git push -u origin master                  把代码提交到远程仓库   

**出现fatal: 'origin' does not appear to be a git repository问题时**

```git
git remote -v：                        查看远程仓库详细信息，可以看到仓库名称

git remote remove orign：              删除orign仓库（如果把origin拼写成orign，删除错误名称仓库）

git remote add origin ssh：            重新添加远程仓库地址

gti push -u origin master：            提交到远程仓库的master主干
```

### 将本地仓库的内容上传到远程

```git
git add.                               将工作区所有文件添加到暂存区

git commit -m "描述信息"                 将暂存取得文件添加到本地仓库

git push                               将本地仓库文件提交到远程仓库
```



#### clone

git clone ssh 

clone会做三件事：

1. 完整的把远程库下载到本地
2. 创建origin远程地址别名
3. 初始化本地库

#### 拉取代码

git pull origin master     // 相当于git fetch 和git merge

#### git commit操作以及退出日志编辑状态

刚进去时发现怎么也输入都没反应，是因为此时vim编辑器处于不可编辑状态，输入字母 `c` 可以进入编辑状态，这个时候就可以修改注释信息啦 ~

修改完之后按`esc`键退出编辑状态，再按大写`ZZ`就可以保存退出vim编辑器。