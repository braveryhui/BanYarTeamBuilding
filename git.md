# Git使用笔记
标签： git
 
[TOC]
git是分布式版本管理工具。Git可以在Linux、Unix、Mac和Windows这几大平台上正常运行。git不区分服务端和客户端，也就意味着，当你安装好了git，客户端和服务端都有了。
本文以github为例讲解如何使用git。
##  如何在公司快速使用git协作
如果之前没有接触过git，公司里使用git协作，为了快速能用起git，你不用去学git，只要会下面的命令即可。
需要克隆项目：
```
git clone git@github.com:52fhy/gitlearn.git
```
记得替换成实际的项目路径。
之后每天只要使用下面4条命令即可：
```
git add .
git commit -m "备注"
git pull
git push
```
记住顺序不能乱，pull之前先提交本地到版本库，防止被拉下来的代码覆盖；提交之前先拉线上的代码下来进行合并，防止冲突。
pull拉取线上代码到本地并执行合并，相当于执行了`git fetch;git merge`；
push推送本地(默认是master分支)到线上并合并，注意观察是否有冲突。有冲突必须手动修改，再从头执行一遍。
每天基本上就上面4条命令就足够了。
如果第一次你还没有ssh公钥，现象就是你克隆不了，没有权限。如果本地的用户目录下面没有`.ssh`目录没有
`id_rsa.pub`公钥文件，需要新建公钥私钥：
```
$ ssh-keygen -t rsa -C "youremail@example.com"
```
接下来一路回车即可。
## 第一次使用github
1)github注册账号，做远程仓库使用。网址：
https://github.com/

使用邮箱注册账号
先不要创建版本库。
2)安装git
Linux请参考网上教程，这里演示windows操作。
实际命令行操作基本是一样的。
Git - Downloads

https://git-scm.com/downloads

安装完成后，在开始菜单里找到`"Git"->"Git Bash"`，蹦出一个类似命令行窗口的东西，就说明Git安装成功！
说明：git命令操作和Linux命令差不多，很多命令可以直接使用,比如`cd,vi`
3)安装完成后，还需要最后一步设置，即**配置本地git用户名和邮箱**。在命令行输入：
```
$ git config --global user.name "Your Name"
$ git config --global user.email "email@example.com"
```
>因为Git是分布式版本控制系统，所以，每个机器都必须自报家门：你的名字和Email地址。你也许会担心，如果有人故意冒充别人怎么办？这个不必担心，首先我们相信大家都是善良无知的群众，其次，真的有冒充的也是有办法可查的。
注意`git config`命令的`--global`参数，用了这个参数，表示你这台机器上所有的Git仓库都会使用这个配置，当然也可以对某个仓库指定不同的用户名和Email地址。
下次重新使用这两个命令可以更新用户名和邮箱。
通过 `git config -l`  可以查看已有的配置。
4)创建SSH Key
在用户家目录下，看看有没有`.ssh`目录，如果有，再看看这个目录下有没有`id_rsa`和`id_rsa.pub`（公钥）这两个文件，如果已经有了，可直接跳到下一步。如果没有，打开Shell（Windows下打开Git Bash），创建SSH Key：
```
$ ssh-keygen -t rsa -C "youremail@example.com"
```
其中`-t`指定rsa加密；`-C`是备注，可选。
会让你输入 `.ssh/id_rsa` 文件的路径，默认即可（需要保存多个公钥时可以更换路径）
然后输入自定义私钥密码（下次pull或者push操作时用来确认你的身份），确认即可：
```
Generating public/private rsa key pair.
Enter file in which to save the key (/c/Users/YJC/.ssh/id_rsa):
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /c/Users/YJC/.ssh/id_rsa.
Your public key has been saved in /c/Users/YJC/.ssh/id_rsa.pub.
The key fingerprint is:
xx:80:50:0f:25:xx:c8:f1:3c:fe:5e:90:be:9d:d5:xx
```
注意：
如果由于其他原因导致git提交时提示 `Could not create directory '//.ssh'.` ，可以删除之前的`id_rsa`文件，重新进行：
```
$ ssh-keygen -t rsa -C "youremail@example.com"
```
windows上id_rsa文件位于用户主目录：`C:/Users/YJC/.ssh/`
```
id_rsa
id_rsa.pub
```
当提示 `Enter file in which to save the key (//.ssh/id_rsa):` 默认可以为空，如果不行，输入：`C:/Users/YJC/.ssh/id_rsa`
当提示
```
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
```
输入一个密码即可。下次提交代码会让你输入密码。
5)在github上保存刚生成好的公钥。登陆GitHub，打开"Account settings"，"SSH Keys"页面：
然后，点"Add SSH Key"，填上任意Title，在Key文本框里粘贴 `id_rsa.pub` 文件的内容：
>为什么GitHub需要SSH Key呢？因为GitHub需要识别出你推送的提交确实是你推送的，而不是别人冒充的，而Git支持SSH协议，所以，GitHub只要知道了你的公钥，就可以确认只有你自己才能推送。
>
当然，GitHub允许你添加多个Key。假定你有若干电脑，你一会儿在公司提交，一会儿在家里提交，只要把每台电脑的Key都添加到GitHub，就可以在每台电脑上往GitHub推送了。
6)创建本地版本库(我选的D盘)
```
$ cd /d/phpsetup/www/git/
$ mkdir fhyblog
$ cd fhyblog
$ pwd
/d/phpsetup/www/git/fhyblog
```
7)然后通过 `git init` 命令把这个目录变成Git可以管理的仓库：
```
$ git init
Initialized empty Git repository in /Users/52fhy/fhyblog/.git/
```
瞬间Git就把仓库建好了，而且告诉你是一个空的仓库（empty Git repository），细心的读者可以发现当前目录下多了一个 `.git` 的目录，这个目录是Git来跟踪管理版本库的，没事千万不要手动修改这个目录里面的文件，不然改乱了，就把Git仓库给破坏了。
>注意几个概念：
工作区就是创建仓库的文件夹如（fhyblog文件夹就是一个工作区）
版本库就是工作区的隐藏目录.git
版本库中有暂存区（stage/index）和分支（master）
git add 实际是把文件添加到暂存区
git commit 把暂存区的内容提交到当前分支
8)在本地版本库fhyblog里放入一些代码或文件
我放了src目录和一个readme.txt文件
`git status`  可以查看工作区有改动。
9)进入版本库目录：
```
$ cd /d/phpsetup/www/git/fhyblog/
```
10)更新本地版本库(.指当前所有目录及文件)
```
$ git add .
```
当然,如果你仅仅是提交一个文件,可以这样写
```
$ git add readme.txt
```
更新一个目录这样写：
```
$ git add src/
```
此时还没有真正提交到版本库,只是从工作区放到暂存区。提交请继续往下看：
11)执行更新操作：
```
$ git commit -m "相关说明"
[master 91115af] .
1 file changed, 53 insertions(+)
create mode 100644 "\345\215\207\347\272\247\346\227\245\345\277\227.txt"
```
这样就提交到了本地版本库的master分支（默认）。通过` git log` 可以查看提交记录，通过` git status `可以查看工作区情况。
一般情况，如果是个人开发，主要就是 `git add` 命令和 `git commit` 命令：提交更改至暂存区，然后提交到分支。
如果是团队协作，需要统一在远程版本库交换更改，就要用到 `git pull` 和 `git push` 命令了。
注意：git本身并不依赖远程版本库，这点和svn不一样。远程版本库的作用是方便交换更改，如github。
12)更新至远程(Github):
查看远程库：
```
$ git remote -v
```
要关联一个远程库，使用命令
```
$ git remote add origin git@github.com:yourname/yourgit.git
```
其中`origin`是远程主机名。
注意：
```
$ git remote add <主机名> <网址>   //添加远程主机
$ git remote rm <主机名>  //删除远程主机
$ git remote rename <原主机名> <新主机名> //远程主机的改名
```
关联后，使用命令
```
git push -u origin master
```
进行第一次推送master分支的所有内容；
所以，远程github上确保你的版本库是空的，否则你在这一步可能会不成功。
此后，每次本地提交后，只要有必要，就可以使用命令 `git push origin master` 推送最新修改；
```
$ git push origin master
Warning: Permanently added the RSA host key for IP address '192.30.252.129' to the list of known hosts.
Enter passphrase for key '/c/Users/YJC/.ssh/id_rsa':
Counting objects: 3, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (2/2), done.
Writing objects: 100% (2/2), 292 bytes | 0 bytes/s, done.
Total 2 (delta 0), reused 0 (delta 0)
To git@github.com:52fhy/fhyblog.git
efe4969..91115af master -> master
Branch master set up to track remote branch master from origin.
Admin@YJC-PC /d/phpsetup/www/git/fhyblog (master)
```
如果完成到这里,恭喜你!你已经有了本地和远程版本库了。
注意：
`origin` 是默认的远程版本库（主机）名称（可以在 `.git/config` 之中进行修改）
`git push origin master` 的意思是 `git push origin master:master` （将本地的 `master` 分支推送至远端的 `master` 分支，如果没有就新建一个）
`git push origin master`和`git push`有什么区别？
>不带任何参数的git push，默认只推送当前分支，这叫做simple方式。此外，还有一种matching方式，会推送所有有对应的远程分支的本地分支。Git 2.0版本之前，默认采用matching方法，现在改为默认采用simple方式。如果要修改这个设置，可以采用git config命令。
`master`其实是一个`refspec`，正常的`refspec`的形式为`+<src>:<dst>`，冒号前表示`local branch`的名字，冒号后表示`remote repository`下 `branch`的名字。注意，如果你省略了`<dst>`，git就认为你想push到remote repository下和local branch相同名字的branch。
删除远程库：
```
$ git rm git@github.com:yourname/yourgit.git
```
## 再次使用git
以后本地版本库里有更新,使用 git add  添加,使用命令 git commit 提交。
更新至远程使用命令 git push origin master 推送
```
git add .
git commit -m "update something..."
git pull
git push
```
先git pull再git push防止覆盖他人代码造成冲突。
可以使用`git status`命令查看git仓库状况。
## 从远程更新至本地版本库
要克隆一个仓库，首先必须知道仓库的地址，然后使用git clone命令克隆。
```
$ git clone git@github.com:52fhy/fhyBlog.git
Cloning into 'fhyBlog'...
Enter passphrase for key '/c/Users/YJC/.ssh/id_rsa':
remote: Counting objects: 284, done.
remote: Compressing objects: 100% (238/238), done.
remote: Total 284 (delta 28), reused 283 (delta 27)R
Receiving objects: 94% (267/284), 644.00 KiB | 12.00 KiB/
Receiving objects: 100% (284/284), 676.81 KiB | 12.00 KiB/s, done.
Resolving deltas: 100% (28/28), done.
```
## 写在后面
本文仅是Git的基本使用,可以作为新手入门教程。更多Git相关操作，请参考：
>1、Git教程,极力推荐，尤其是新手。本文就参考了该教程。<br/>
2、《Pro Git》一书,百度搜索吧;<br/>
3、Git官方教程
参考文档：
>1、Git 使用及原理 总结 - 人间奇迹 - 博客园

http://www.cnblogs.com/yaozhongxiao/p/3794963.html

<br/>
2、learngit/git学习笔记.md at master · michaelliao/learngit · GitHub

https://github.com/michaelliao/learngit/blob/master/Git%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0/git%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.md

<br/>
3、Git远程操作详解 - 阮一峰的网络日志

http://www.ruanyifeng.com/blog/2014/06/git\_remote.html
