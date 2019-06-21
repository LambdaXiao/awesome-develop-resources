

![Image text](https://github.com/x1971481259/Notes/raw/master/screenshot/gitflows.png)

图中几个专用名词的译名如下。

- Workspace：工作区
- Index / Stage：暂存区
- Repository：本地仓库
- Remote：远程仓库

下面是我整理的常用 Git 命令清单。

#### 一、配置

Git的设置文件为.gitconfig，它可以在用户主目录下（全局配置），也可以在项目目录下（项目配置）。

* 显示当前的Git配置

```
$ git config --list
```

* 编辑Git配置文件

```
$ git config -e [--global]
```

* 设置提交代码时的用户信息

```
$ git config [--global] user.name "[name]"
$ git config [--global] user.email "[email address]"
```
* 修改远程地址

```
$ git remote set-url origin [url]
```

#### 二、新建代码库

* 在当前目录新建一个Git代码库
 
```
$ git init
```

* 新建一个目录，将其初始化为Git代码库

```
$ git init [project-name]
```

* 下载一个项目和它的整个代码历史
 
```
$ git clone [url]
```

#### 三、增加/删除文件

* 添加指定文件到暂存区

```
$ git add [file1] [file2] ...
```

* 添加指定目录到暂存区，包括子目录

```
$ git add [dir]
```
* 提交被修改和被删除文件，不包括新文件

```
$ git add -u
```

* 添加当前目录的所有文件到暂存区

```
$ git add .
```

* 删除工作区文件，并且将这次删除放入暂存区

```
$ git rm [file1] [file2] ...
```

* 停止追踪指定文件，但该文件会保留在工作区

```
$ git rm --cached [file]
```

* 改名文件，并且将这个改名放入暂存区

```
$ git mv [file-original] [file-renamed]
```


#### 四、代码提交

* 提交暂存区到本地仓库区

```
$ git commit -m [message]
```

* 提交暂存区的指定文件到本地仓库区

```
$ git commit [file1] [file2] ... -m [message]
```

* 提交工作区自上次commit之后的变化，直接到本地仓库区

```
$ git commit -a
```

* 提交时显示所有diff信息

```
$ git commit -v
```

* 使用一次新的commit，替代上一次提交（如果代码没有任何新变化，则用来改写上一次commit的提交信息）

```
$ git commit --amend -m [message]
```

* 重做上一次commit，并包括指定文件的新变化

```
$ git commit --amend [file1] [file2] ...
```


#### 五、远程同步

* 显示所有远程仓库

```
$ git remote -v
```

* 显示某个远程仓库的信息

```
$ git remote show [remote]
```

* 增加一个新的远程仓库，并命名

```
$ git remote add [shortname] [url]
//例如
$ git remote add origin git@github.com:Wbiokr/chatApp.git
```

* 下载远程仓库的所有变动到本地仓库

```
$ git fetch [remote]
```

* 取回远程仓库的变化，并与本地分支合并

```
$ git pull [remote] [branch]
```

* 上传本地指定分支到远程仓库

```
$ git push [remote] [branch]
```

* 强行推送当前分支到远程仓库，即使有冲突
 
```
$ git push [remote] --force
```

* 推送所有分支到远程仓库

```
$ git push [remote] --all
```

#### 六、撤销与合并

> 以下主要适用于未commit代码前，而去撤销工作区或暂存区的修改变化
* 将工作区的指定文件恢复为和暂存区的指定文件保持一致

```
$ git checkout [file]
```

* 将工作区所有文件恢复为和暂存区的保持一致

```
$ git checkout .
```

* 将工作区和暂存区的指定文件恢复为和某个commit的指定文件保持一致

```
$ git checkout [commit] [file]
```

> 以下主要适用于已commit代码了，而去撤销commit，使工作区保持未commit前的状态，工作区所做的修改不能丢失。

> 注意：记住git reset不会产生commits,它本身做的事情就是重置HEAD(当前分支的版本顶端）到另外一个commit，并且删除该commit后面的所有commmit。--mixed是reset的默认参数，它将重置HEAD到另外一个commit,并且重置暂存区以便和HEAD相匹配，但工作区不变；--soft参数告诉Git重置HEAD到另外一个commit，但暂存区与工作区都不变；--hard参数将重置HEAD返回到另外一个commit，同时重置暂存区与工作区，三者保持一致，这很容易丢失数据。

* 重置暂存区的指定文件，与上一次commit保持一致，但工作区不变

```
$ git reset [file]
```

* 重置当前分支的指针为指定commit，同时重置暂存区，但工作区不变

```
$ git reset [commit]
```

* 重置暂存区与工作区，与上一次commit保持一致

```
$ git reset --hard
```

* 重置当前分支的HEAD为指定commit，同时重置暂存区和工作区，与指定commit一致

```
$ git reset --hard [commit]
```

* 重置当前HEAD为指定commit，但保持暂存区和工作区不变

```
$ git reset --keep [commit]
```

* 新建一个commit，用来撤销指定commit（指定的commit的所有变化都将被新的commit抵消，并且应用到当前分支）

```
$ git revert [commit]
```

* 合并指定分支到当前分支

```
$ git merge [branch-name]
```

* 变基操作
> 即改变基础，把当前分支的基础变为[branch-name]，该命令会把你的当前分支里的每个提交(commit)取消掉，并且把它们临时保存为补丁(patch)(这些补丁放到".git/rebase"目录中),然后把当前分支更新 为最新的[branch-name]分支，最后把保存的这些补丁应用到当前分支上。在rebase的过程中，也许会出现冲突(conflict). 在这种情况，Git会停止rebase并会让你去解决 冲突；在解决完冲突后$ git rebase --continue，这样git会继续应用(apply)余下的补丁。在任何时候，你可以用--abort参数来终止rebase的行动，并且当前分支会回到rebase开始前的状态。

```
$ git rebase [branch-name]
```

> merge与rebase的区别：merge操作会生成一个新的commit，之前的提交分开显示。而rebase操作不会生成新的commit，是将两个分支融合成一个线性的提交。


* 抛弃本地所有的修改，回到远程仓库的状态。

```
$ git fetch --all && git reset --hard origin/master
```
* 把所有的改动都重新放回工作区，并清空所有的 commit，这样就可以重新提交第一个 commit 了

```
$ git update-ref -d HEAD
```

#### 七、分支
> 注意：[remote-branch]表示：如果远程分支已存在[remote-branch] = origin/branch-name,不存在[remote-branch] = origin branch-name

* 列出所有本地分支

```
$ git branch
```

* 列出所有远程分支

```
$ git branch -r
```

* 列出所有本地分支和远程分支

```
$ git branch -a
```

* 新建一个分支，但依然停留在当前分支

```
$ git branch [branch-name]
```

* 新建一个分支，并切换到该分支

```
$ git checkout -b [branch-name]
```

* 新建一个分支，指向指定commit

```
$ git branch [branch-name] [commit]
```

* 新建一个分支，与指定的远程分支建立追踪关系

```
$ git branch --track [branch-name] [remote-branch]
```
* 重命名本地分支

```
$ git branch -m <new-branch-name>
```

* 建立追踪关系，在现有分支与指定的远程分支之间

```
$ git branch --set-upstream [branch-name] [remote-branch]
```

*删除指定的远程分支与某个本地分支之间的追踪关系

```
$ git branch -dr [remote-branch]
```
* 展示本地分支关联远程仓库的情况

```
$ git branch -vv
```
* 切换到指定分支，并更新工作区

```
$ git checkout [branch-name]
```
* 快速切换到上一个分支

```
$ git checkout -
```
* 选择一个commit，合并进当前分支

```
$ git cherry-pick [commit]
```

* 删除本地分支

```
$ git branch -d [branch-name]
```

* 强制删除本地分支，尤其适用于内容有了新修改但没提交的情况

```
$ git branch -D [branch-name]
```

* 删除远程分支

```
$ git push origin --delete [branch-name]
```

#### 八、查看信息

* 显示有变更的文件

```
$ git status
```

* 查看未同步到远程代码库的提交描述/说明

```
$ git cherry -v
```

* 显示当前分支的版本历史，不能查看已经删除的commit记录
 
```
$ git log
```

* 单行显示当前分支的版本历史

```
$ git log --pretty=oneline
```
* 显示本地所有分支更新过 HEAD 的 git 命令记录（包括已经被删除的 commit 记录和 reset 的操作）
 > 每次更新了 HEAD 的 git 命令比如 commint、amend、cherry-pick、reset、revert 等都会被记录下来（不限分支）
```
$ git reflog
```

* 显示commit历史，以及每次commit发生变更的文件

```
$ git log --stat
```

* 显示某个文件的版本历史，包括文件改名

```
$ git log --follow [file]
$ git whatchanged [file]
```
* 查看两个星期内的改动

```
$ git whatchanged --since='2 weeks ago'
```

* 在 commit log 中查找相关内容

```
$ git log --all --grep='查找内容'
```

* 显示指定文件相关的每一次diff
 
```
$ git log -p [file]
```

* 显示指定文件是什么人在什么时间修改过

```
$ git blame [file]
```

* 显示暂存区和工作区的差异

```
$ git diff
```

* 显示暂存区和上一个commit的差异

```
$ git diff --cached [file]
```

* 显示工作区与当前分支最新commit之间的差异

```
$ git diff HEAD
```

* 显示两次提交之间的差异
 
```
$ git diff [first-branch]...[second-branch]
```

* 显示某次提交的元数据和内容变化

```
$ git show [commit]
```

* 显示某次提交发生变化的文件

```
$ git show --name-only [commit]
```

* 显示某次提交时，某个文件的内容
 
```
$ git show [commit]:[filename]
```

* 显示分支合并图

```
$ git  log --graph
```

* 单行显示分支合并图

```
$ git log --oneline --graph 
```

#### 九、标签

* 查看所有tag

```
$ git tag
```

* 查看tag详细信息

```
$ git tag -ln
```
* 展示当前分支的最近的 tag

```
$ git describe --tags --abbrev=0
```
* 新建一个tag指向当前commit

```
$ git tag [tag-name]
```

* 新建一个tag指向指定commit

```
$ git tag [tag-name] [commit]
```

* 查看tag信息

```
$ git show [tag-name]
```

* 提交指定tag

```
$ git push [remote] [tag-name]
```

* 提交所有tag

```
$ git push [remote] --tags
```

* 新建一个分支，指向某个tag

```
$ git checkout -b [branch-name] [tag-name]
```

#### 十、存储工作区的变化

* 把当前分支的工作现场存储起来，等以后恢复现场后继续工作，适用于还没有添加到暂存区的分支代码

```
$ git stash
```

* 查看存储的工作现场记录列表

```
$ git stash list
```

* 恢复最近stash的工作现场，但是恢复后，stash内容并不删除。后面可以加stash_id 或stash@{num}

```
$ git stash apply
```

* 删除最近stash的工作现场，后面可以加stash_id 或stash@{num}

```
$ git stash drop
```

* 相当于上面两条命令，后面可以加stash_id 或stash@{num}

```
$ git stash pop
```

* 清空所有暂存区的stash记录
 
```
$ git stash clear
```
#### git命令的别名设置

```
//比如：git status 改成 git st，这样可以简化命令
$ git config --global alias.st status
```

别名 Alias | 命令 Command | 如何设置 What to Type
---|---|---
git cm | git commit | git config --global alias.cm commit
git co | git checkout | git config --global alias.co checkout
git ac | git add . -A git commit|git config --global alias.ac '!git add -A && git commit'
git st|git status -sb|git config --global alias.st 'status -sb'
git tags|git tag -l|git config --global alias.tags 'tag -l'
git branches|git branch -a|git config --global alias.branches 'branch -a'
git cleanup|git branch --merged | grep -v '*' | xargs git branch -d|git config --global alias.cleanup "!git branch --merged | grep -v '*' | xargs git branch -d"
git remotes|git remote -v|git config --global alias.remotes 'remote -v'

别名 Alias | 命令 Command | 如何设置 What to Type
---|---|---
git lg|git log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit --|git config --global alias.lg "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit --"
#### git bash中的快捷键
 
 
```
tab 自动补全，连按两次会将所有匹配内容显示出来

history 查看操作历史

pwd (Print Working Directory) 查看当前目录
 
cd (Change Directory) 切换目录，如 cd /etc

ls (List) 查看当前目录下内容，如 ls -al

mkdir (Make Directory) 创建目录，如 mkdir blog

touch 创建文件，如 touch index.html

cat 查看文件全部内容，如 cat index.html

Ctrl + l：清屏
```


#### 使用gitlab,安装git后获取公钥key
 
```
$ ssh-keygen

$ cat ~/.ssh/

$ cat ~/.ssh/id_rsa.pub
```

另外在C:\Windows\System32\drivers\etc\hosts 添加代码远程服务器地址

#### Git命令行窗口使用vi或vim命令打开、关闭、保存文件

**1、vi & vim 有两种工作模式：**

（1） 命令模式：接受、执行 vi & vim 操作命令的模式，打开文件后的默认模式；

（2） 编辑模式：对打开的文件内容进行 增、删、改 操作的模式；

> 键盘输入字母 “i”或“Insert”键进入最常用的插入编辑模式。

>  在编辑模式下按下 ESC 键，回退到命令模式。

**2、创建、打开文件**

> 使用 vi 加 文件路径（或文件名）的模式打开文件，如果文件存在则打开现有文件，如果文件不存在则新建文件，并在终端最下面一行显示打开的是一个新文件。

```
$ vi [filename]
```

**3、保存文件**

（1）在编辑模式下编辑文件。

（2）按下 “ESC” 键，退出编辑模式，切换到命令模式。

（3）在命令模式下键入"ZZ"或者":wq"保存修改并且退出 vi 。

（4）如果只想保存文件，则键入":w"，回车后底行会提示写入操作结果，并保持停留在命令模式。

**4、放弃所有文件修改**

（1）放弃所有文件修改：按下 "ESC" 键进入命令模式，键入 ":q!" 回车后放弃修改并退出vi。

（2）放弃所有文件修改，但不退出 vi ，即回退到文件打开后最后一次保存操作的状态，继续进行文件操作：按下 "ESC" 键进入命令模式，键入 ":e!" ，回车后回到命令模式。

●  底行模式 

    :w  保存    
    :w filenme  另存为
    :q  退出
    :wq 保存并退出
    :e！ 撤销更改返回到上一次保存状态
    :q！不保存强制退出
    :set nu 设置行号
  
  ●  命令模式
  
    ZZ（大写）保存并退出
    u 辙销操作，可多次使用
    dd 删除当前行
    yy 复制当前行
    p 粘贴内容
    ctrl+f 向前翻页
    ctrl+b 向后翻页
 
    敲 i 进入编辑模式，当前光标处插入
    敲 a 进入编辑模式，当前光标后插入
    敲 A 进入编辑模式，光标移动到行尾
    敲 o 进入编辑模式，当前行下面插入新行
    敲 O 进入编辑模式，当前行上面插入新行

  ●  编辑模式 

    当我们处在编辑模式的情况下，和我们在Windows编辑器的使用相似。
  
 
