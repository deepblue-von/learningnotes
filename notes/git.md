### git

mkdir  folder    创建文件夹

vim  file             创建文件

#### git第一个命令

|command| means|
|---|---|
| git init | 在当前目录初始化一个git本地仓库 |
| git status | 显示工作区和暂存区的状态 |
| git add  file | 将文件提交到暂存区 |
| git rm  file | 从工作区和索引中删除文件 |
| git rm --cached  file | 用于将暂存区的文件恢复到工作区 |
| git commit  file | 将更改记录提交到本地库(备注信息，Esc， :wq) |
| git checkout file | 恢复暂存区内容到工作区 |
| git commit -m '第二次提交' hello.txt | 提交时添加备注信息 |
| git reset head hello.txt | 将本地库的内容写到暂存区 |
| git remote add origin  ssh | 配置远程仓库 |

#### 在远程创建一个仓库

git remote add origin ssh                 配置远程仓库(origin 别名)

git push -u origin master                  把代码提交到远程仓库   

#### clone命令

git clone ssh 

clone会做三件事：

1. 完整的把远程库下载到本地
2. 创建origin远程地址别名
3. 初始化本地库

#### 拉取代码

git pull origin master     // 相当于git fetch 和git merge

