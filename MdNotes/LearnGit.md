# 简介





# 安装

**Linux**：

```
查看：git
安装：sudo apt-get install git
```



**Mac OS X**

一、使用 homebrew (第二次看到这个，是什么？)

安装git： brew install git

确认： /usr/local/bin $ git --version





homebrew：Mac OS平台的软件包管理工具。

查看版本： brew --version

确认安装路径： cd /usr/local

​							ls -la

更新homebrew： brew update

使用homebrew更新工具： brew upgrade

清理旧版本包：brew cleanup 

查看可清理的旧版本包：brew cleanup -n

查看安装的包列表：brew list

查看包信息： brew info 包名

查看包的依赖: brew deps



升级git版本：

```
brew install git
```

改变默认 `Git` 指向

```
which git 

git --version


brew link git --overwrite
```

link 成功后，退出终端后，再次打开。然后查看 `git` 指向和版本信息。







二、Xcode 

？？

Xcode是Apple官方IDE。是开发Mac和iOS App的必选装备。

Xcode 集成了git，运行xcode，在首选项中的下载中，选择Command Line Tool 安装



# 创建版本库

版本库，repository, 可以理解成一个 **目录**，这个目录里所有的文件都可以被git管理起来，git可以跟踪每个文件的修改、删除等。

**创建版本库**

初始化一个git仓库： git init

添加文件到git仓库：

- 把文件放进git目录里
- `git add <filename>` 把文件添加到仓库，可以多次执行命令，添加多个文件
- `git commit -m "message"`  把文件提交到仓库





# 时光机穿梭

## 修改文件

`git status` 查看文件状态，能看到文件被修改，但看不到具体修改细节

`git diff` 查看修改的细节，diff 就是 difference，显示的格式是Unix通用的diff格式

提交修改：

`git add <filename>` 

`git commit -m "message"` 提交命令

`git satus` 提交后可以继续查看仓库的状态



- 要随时掌握工作区的状态，使用`git status`命令。
- 如果`git status`告诉你有文件被修改过，用`git diff`可以查看修改内容。





## 回退版本

`git log` 查看提交日志，可以看到最近3次

如果嫌输出信息太多，看得眼花缭乱的，可以试试加上`--pretty=oneline`参数



在Git中，用`HEAD`表示当前版本，，上一个版本就是`HEAD^`，上上一个版本就是`HEAD^^`，当然往上100个版本写100个`^`比较容易数不过来，所以写成`HEAD~100`。

```
// 回退到上一个版本
$ git reset --hard HEAD^
```

```
// 查看回退的版本
$ cat readme.txt
```

```
// 根据commit id 回退版本，版本号可以只写前面，
$ git reset --hard 1094a
HEAD is now at 83b0afe append GPL
```





```
// 记录每次提交版本的id

$ git reflog
```



# 远程仓库



# 分支管理



# 标签管理



# gitHub









命令行

pwd 用于显示当前目录

我的git目录 /Users/neofei/Fei/git



创建仓库

建目录



git init



显示隐藏文件 ls -ah





全部提交

git add .





git config user.name

git config user.email

我的github账号是怎么绑定到了git上？？？

自动？





git add file 就是将文件放入暂存区（staged）

git commit 就是将暂存区的文件提交至仓库





git reset filename 后悔的文件

