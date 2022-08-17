### 基本理论

### Git本地有三个工作区域：

***(1)工作目录** (Working Direct**ory)

***(2)*暂存区**(Stage/Index)**

***(3)资源库(Repository)***

**(4)如果在远程的git 仓库(Remote Directory)**就可以分为四个工作区域，文件在这四个区域之间的转换关系如下：

* Workspace:工作区，就是平时存放代码的地方
* Index/Stage：暂存区 ，用于临时存放你的改动，事实上它只是一个文件，保存即将提交到文件列表信息
* Repository：仓库区（本地仓库）：就是安全存放数据的位置，这里面有你提交到所有版本的数据，其中HEAD指向最新放入仓库的版本
* Remote：远程仓库，托管代码的服务器，简单地认为是你项目组中的一台电脑用于远程数据交换

1. Git工作流程一般是：
   1. 在工作目录中添加、修改文件
   2. 将需要进行版本管理的文件放入暂存区域
   3. 将暂存区域的文件提交到git仓库
      因此，git 管理的文件有三种状态，：**已修改**（modified ）  **以暂存**（staged）**已提交**（committed ）

### Git项目搭建

> **本地仓库搭建**：

1. 创建全新的仓库 用GIT管理的项目根目录执行：

```bash
#在当前目录创建一个GIT代码库
git init
```

2.执行后可以看到，仅仅在项目目录多出了一个.git目录，关于版本等的所有信息都在这个项目里面

> **克隆远程仓库**

1. 克隆远程目录，由于是将远程服务器上的仓库完全镜像一份至本地

```bash
#克隆一个项目和它的整个代码历史 （版本信息）
git clone [url]
```

2. 去gitee或者github上克隆一个测试

### Git基本操作命令

**Git文件操作**

**查看文件状态**

```bash
#查看所有文件状态
git status [filename]
#查看所有文件状态
git status
#显示日志，提交的一些版本信息
git log --oneline
git log --graph --oneline
git add  .  #添加所有文件到暂存区
git --version 查看版本
git commit -m "消息内容"  #提交暂存区中的内容到本地仓库  -m 提交信息
```

**忽略文件**`

### 码云

1. 注册登录码云，完善个人信息
2. 设置本机绑定的SSH公钥，实现免密码登录！

   ```bash
   #进入C:\users\Administrator\.ssh目录
   #生成公钥
   ssh-keygen -t rsa #一路按回车就可以
   
   ```
3. 将公钥信息public key 添加到码云账户中即可
4. 使用码云创建自己的仓库

### IDEA中集成Git

1. 新建项目，绑定git
2. 修改文件，使用IDEA操作Gitpu

   1. 添加到暂存区
   2. commit
   3. git push 到远程仓库
3. 提交测试

### 详细教程

#### 初期配置

安装Git之后，请输入您的用户名和电子邮件地址。该设置操作在安装Git后进行一次就够了。这些信息将作为提交者信息显示在更新历史中。

Git的设定被存放在用户本地目录的.gitconfig档案里。虽然可以直接编辑配置文件，但在这个教程里我们使用config命令。

```
$ git config --global user.name "<用户名>"
$ git config --global user.email "<电子邮件>"
```

修改git 提交缓存大小

```bash
$ git config --global http.postBuffer 524288000
 
$ git config https.postBuffer 524288000
```



以下命令能让Git以彩色显示。

```
$ git config --global color.ui auto
```

您可以为Git命令设定别名。例如：把「checkout」缩略为「co」，然后就使用「co」来执行命令。

```
$ git config --global alias.co checkout
```

```

如果在Windows使用命令行 (Git Bash), 含非ASCII字符的文件名会显示为 "\346\226\260\350\246..."。若设定如下，就可以让含非ASCII字符的文件名正确显示了。

$ git config --global core.quotepath off
若在Windows使用命令行，您只能输入ASCII字符。所以，如果您的提交信息包含非ASCII字符，请不要使用-m选项，而要用外部编辑器输入。

外部编辑器必须能与字符编码UTF-8和换行码LF兼容。

git config --global core.editor "\"[使用编辑区的路径]\""
```

#### 新建本地仓库

```
git init
```

#### 文件放入缓存区

将文件加入到索引，要使用add命令。在`<file>`指定加入索引的文件。用空格分割可以指定多个文件。

```
$ git add <file>..
```

指定参数「.」，可以把所有的文件加入到索引。

```
$ git add .
```

#### 提交文件到本地仓库

```
 git commit -m ""
```

~从status响应我们可以看到没有新的变更要提交。~

使用**log命令**，我们可以在数据库的提交记录看到新的提交。

#### Push 到远程仓库

向远程数据库推送本地数据库的修改记录

请使用remote指令添加远程数据库。在 `<name>`处输入远程数据库名称，在 `<url>`处指定远程数据库的URL。

```
$ git remote add <name> <url>
```

> 执行推送或者拉取的时候，如果省略了远程数据库的名称，则默认使用名为”origin“的远程数据库。因此一般都会把远程数据库命名为origin。

使用push命令向数据库推送更改内容。`<repository>`处输入目标地址，`<refspec>`处指定推送的分支。

```
$ git push <repository> <refspec>...
```

当执行命令时，如果您指定了-u选项，那么下一次推送时就可以省略分支名称了。但是，首次运行指令向空的远程数据库推送时，必须指定远程数据库名称和分支名称。

```
$ git push -u origin master
```

#### 克隆远程仓库

使用clone指令可以复制数据库，在 `<repository>`指定远程数据库的URL，
在 `<directory>`指定新目录的名称。

```
$ git clone <repository> <directory>
```

#### 从远程pull拉取到本地

使用pull指令进行拉取操作。省略数据库名称的话，会在名为origin的数据库进行pull。

```
$ git pull <repository> <refspec>...
```

#### **合并修改记录**

在执行pull之后，进行下一次push之前，如果其他人进行了推送内容到远程数据库的话，那么你的push将被拒绝。

这种情况下，在读取别人push的变更并进行合并操作之前，你的push都将被拒绝。这是因为，如果不进行合并就试图覆盖已有的变更记录的话，其他人push的变更就会丢失。

# **远程链接步骤**

```bash


#建立本次仓库 
git init 
#文件添加到缓存区域
 git add file file
#提交到本地仓库
 git commit -m "描述信息"
#配置远程仓库地址
 git remote add origin [仓库地址]
#拉取
 git pull --rebase origin master
#发送 
git push origin master
```
