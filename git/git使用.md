#### 1.在bash终端配置用户名和邮箱

```
git config -- global user.name"Carson"
git config -- global user.email"851278531@qq.com"
```

#### 2.生成私钥和公钥

```
ssh-keygen -t rsa
```

并按回车三下，设置密码为空。

#### 3.在～/.ssh文件夹下

文件下存在一个私钥 id_rsa和一个公钥 id_rsa.pub

#### 4.复制公钥通过ssh连接gitee.com或者github.com

通过 ssh -T git@gitee.com 或者 ssh -T git@github.com判断是否连接成功

出现Hi+名字说明ssh连接成功。

#### 5进行版本控制

1在所需要的工程或者文件夹先建立一个本地仓库（隐藏的.git文件夹）

```
git init
```

2在进行版本控制时先要留下名字和邮箱：相当于建立了一个gitconfig文件

```
git config --global user.name"Carson"
git config --global user.email"851278531@qq.com"
```

3当需要建立版本说明信息时：(工作区通过git add到达暂存区，之后通过git commit到达本地仓库)

```
git add 文件
git commit -m "版本"   （新上传文件进行commit）
git commit -a -m "版本"  （只能帮助我们跟踪那些已经上传的文件，新建上传就不行）
git commit -a (其中如果版本号想多行文字输入，可以使用去掉引号，直接进入nano文本编辑器)
git commit -v (同上-a,增加了不同，做检查)
```

4我们可以用来查看commit的记录以及版本号

```
git log
git log -p（我们可以用来查看修改的内容）
```

由于显示修改不是很直观，因此我们需要前端工具tig

```
brew install tig
```

之后需要查看commit的版本改动只需要输入tig，里面可以根据h输入来看tig的各种用法

或者使用

```
git log --pretty=online
```

5.如果需要回退版本（Head指针）

指向最新版本的指针式HEAD，前一个版本可以用HEAD^或者HEAD～1

```
git reset --hard HEAD^
```

又想回到原来的版本

```
git reset --hard 版本编号
git reflog（如果看不到了，可以使用查看记录来查看编号）
```

#### 6工作区和暂存区

电脑中的代码存放的目录就是工作区，而工作区中有一个隐藏目录.git，是git的版本库，其中有一个stage(index)成为暂存区，git add就是添加修改从工作区到暂存区。

```
git status （可以查看本地工作区做的修改以及git在本地未跟踪的文件）
```

1撤销修改

```
git checkout -- "文件"
```

  读取暂存区的保存，丢弃工作区的修改。（未进行git add）

```
git restore --staged "文件"
git checkout -- "文件"
```

如果已经进行git add，进行暂存区的修改保存。先要撤销暂存区修改，之后再进行工作区修改。

如果已经git commit创建了版本，那就进行版本回退。本地直接回退到之前的版本。

2对比不同

对比工作区和某个版本的不同

```
git diff HEAD -- 文件
```

用来查看本地与暂存区所发生的改变。

对比两个版本的不同

```
git diff HEAD HEAD^ -- 文件
```

3.删除文件

也是文件的改动，可以使用git add/rm，需要撤销删除也就是撤销修改。

#### 7分支管理

Head指向master或者dev，之后再指向修改的版本线。

master主分支。dev开发分支

```
git branch  (查看目前有几个分支，并且在哪个分支下进行工作)
git branch dev (进行创建分支)
```

创建并切换到新建的分支里面：(新建指针，并且HEAD指向新指针进行切换)

```
git checkout -b dev （创建并切换分支）
git checkout master  (直接切换分支)
```

进行分支合并并删除dev（快速合并 master指针移动，dev指针删除）

```
git merge dev (dev和master合并)
git branch -d dev (dev分支删除)
```

1解决冲突

dev在未修改的master进行修改，然后需要合并的时候master已经进行修改了，即两个分支都有了新的提交并且编辑同一个文件。就会产生冲突。

手动解决冲突，将dev和master的修改手动修复，在进行add commit进行提交。

```
git log --graph --pretty=oneline
```

可以查看各分支的提交图。

 如果dev分支新建了文件，而master在老文件中进行修改，直接合并的时候不会起冲突，但是不能进行快速合并。此时合并回会显示一个文件，输入一次新的提交。

为了保存分支提交记录，因此我们需要禁用快速合并，需要有一次新的提交。

```
git merge --no-ff -m "名称" dev
```

当出现紧急任务需要放下手头的工作，去修复新的分支bug

```
git stash  (进行隐藏目前所在的修改)
```

修复bug新开分支，之后不能快速合并，需要保存提交。

修复后恢复之前的修改：

```
git stash list (显示之前隐藏的现场)
git stash pop (恢复之前隐藏的工作现场)
```

#### 8推送分支

```
git push origin 分支名称
```

Origin为远程分支。将自己的写的代码推送上去。

跟踪远程的分支

```
git branch --set-upstream-to=origin/远程分支 本地分支
git status
```

#### 9拉取代码

```
git pull origin 分支名称
```

#### 10克隆共享远程仓库到本地

```
git clone https://github.com/xxxx/xxxx.git
```

