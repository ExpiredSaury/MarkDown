![image.png](https://fynotefile.oss-cn-zhangjiakou.aliyuncs.com/fynote/fyfile/14697/1660709393050/0159a6590bcd4658b23c80e2786b9a3b.png)

8. 点击git bash here
9. git 命令
   * 初始化 git int 文件夹名
   * 初始化 git int  #当前路径下全被管理
   * git status
   * git add a.txt  #把a提交到暂存区
   * git add .   #工作区的所有文件加入暂存区
   * git commit -m “注释” #把暂存区里所有都提交到版本库
   * 需要增加作者信息
     * git config --global user.email "your@email.com"
     * git config --global user.name "your name"
   * 把a的新增提交到版本管理
   * 新建b 在a中新增一行内容
   * git checkout .  #回到指针HEAD指向的版本， a 是空的，b是没有被git管理，所以b不变，a变为空
   * git log #查看版本管理的日志
   * git reset .  #把暂存区的东西拉回到工作区（原来是绿的变红了）
   * git reset --hard
10. **红色代表未被管理**
11. **绿色代表已经提交到暂存区**
12. 忽略文件
    * 空文件夹不被管理
    * 指定某些文件夹或者文件夹不被git管理
    * 在项目根目录，跟.git文件夹一个路径，新建一个.gitignore，在里面配置
    * 语法：
      * #号是注释，没有用
      * 文件夹名字，表示文件夹忽略，不被管理
      * /dist 表示根目录下的dist文件夹，不被管理
      * *.py  表示后缀名为py 的文件，都被忽略
      * *`*.log`**   日志文件忽略
13. 分支操作
    1. 查看分支  git branch
    2. 创建分支  git branch  分支名
    3. 创建并切换分支  git checkout -b 分支名
    4. 切换分支  git checkout 分支名
    5. 删除分支  git branch -d 分支名
    6. 合并分支 git merge 分支名  #把dev合并到master,先切换到master ,执行合并dev分支的命令

**远程连接**

1. 查看远程仓库：git remote
2. 添加远程仓库建立连接：git remote add origin 远程仓库地址
3. 本地master 提交到远程，需要输入用户名密码：git push -u origin master
4. 克隆远程仓库  git clone 远程地址
5. 拉取远程仓库内容   git pull origin master
6. 如果远程仓库内容添加了新的数据，在本地提交远程要先pull远程内容，然后再push 到远程

SSH和HTTPS连接

1. SSH配置，，以后就不用输入密码
2. 如何配置密码
   1. 对称加密：加密和解密都用一套密码
   2. 非对称加密：（公钥和私钥），公钥加密，私钥解密
   3. 生成一对公钥 ` ssh  -keygen -t rsa -C "xxxx@qq.com"`生成到用户家目录.ssh文件夹下（id_rsa私钥，id_rsa.pub公钥）,，，命令输完一路回车就可以
   4. 打开id_rsa.pub公钥文件，把内容复制，再码云上配置粘贴
   5. 删除凭据，之前存的用户名密码删掉

![image.png](https://fynotefile.oss-cn-zhangjiakou.aliyuncs.com/fynote/fyfile/14697/1660709393050/7d81c113ed8f43679a08e12266d501de.png)

6. 克隆远程仓库的时候用SSH 链接，git clone  'ssh的链接'，会让输入yes/no,我们输入yes
7. 再.ssh文件下会多出一个know_hosts文件，存的是gitee的地址

**Pycharm操作git**

1. 安装git
2. 再pycharm中配置，setting---》git---->git.ext的地址
3. git clone --->

![image.png](https://fynotefile.oss-cn-zhangjiakou.aliyuncs.com/fynote/fyfile/14697/1660709393050/0781c043ecc14737963ed7f4ce4d7d90.png)

![image.png](https://fynotefile.oss-cn-zhangjiakou.aliyuncs.com/fynote/fyfile/14697/1660709393050/8f175691d93942c084b1af7aae7d5e7a.png)