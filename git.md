
# git入门
@author 余建材
@date 2017-2-4 14:11:07

---

[TOC]

git是分布式版本管理工具。Git可以在Linux、Unix、Mac和Windows这几大平台上正常运行。git不区分服务端和客户端，也就意味着，当你安装好了git，客户端和服务端都有了。


## 需要的工具

1、git 仓库管理，提供命令行界面。
2、github 全球最大的仓库托管中心，作为远程仓库使用。需要注册账号
3、GitBook Editor 一款好用的基于markdown的编辑器，可以关联个人的github仓库。作为编辑文档使用。当然，该工具是可选的。


Git下载地址：
https://git-scm.com/

github地址：
https://github.com/

GitBook Editor地址：
https://www.gitbook.com/editor
可以使用注册好的github账号授权登录，注意验证邮箱操作。

## 如何在公司快速使用git协作
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
通过 `git config -l` 可以查看已有的配置。

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

## 再次使用git
以后本地版本库里有更新,使用 git add 添加,使用命令 git commit 提交。
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

## GitBook Editor编辑文档
1、安装好GitBook Editor后登陆，可以使用注册好的github账户登陆。
2、登录github后克隆要编写的文档（如果是加入别人的项目），例如进入https://github.com/braveryhui/BanYarTeamBuilding，点击Fork进行克隆；
3、进入到克隆后自己的项目，注意地址上用户名已经变成自己的了，示例：https://github.com/52fhy/BanYarTeamBuilding；
4、点击clone or download，选择Use ssh地址，例如：git@github.com:52fhy/BanYarTeamBuilding.git，复制下来；
5、本地PC使用：git clone git@github.com:52fhy/BanYarTeamBuilding.git
6、点击import，选中克隆下来的文档项目，例如`52fhy/BanYarTeamBuilding`；
7、导入成功即可编辑了。第一次需要设置关联的远程仓库地址：Book->repositories Setting:
https://github.com/52fhy/BanYarTeamBuilding.git
8、有新的变动只需要点击Save，然后Publish即可。

## github如何提交变更
当你的文档写到一定程度需要和原始文档合并，例如我这里克隆的braveryhui/BanYarTeamBuilding，只需要在网页你的项目52fhy/BanYarTeamBuilding点击Pull requests，写明原因，提交，等待对方同意合并即可。当然，对方可能拒绝合并哦。

## github如何同步原仓库

第一次需要绑定下原项目：
```
git remote add pm git@github.com:braveryhui/BanYarTeamBuilding.git
```

pm是起的别名。

通过下面命令可以发现远程关联的地址增加了2行：
```
$ git remote -v
origin  git@github.com:52fhy/BanYarTeamBuilding.git (fetch)
origin  git@github.com:52fhy/BanYarTeamBuilding.git (push)
pm      git@github.com:braveryhui/BanYarTeamBuilding.git (fetch)
pm      git@github.com:braveryhui/BanYarTeamBuilding.git (push)
```

假如源项目更新了，可以通过下面代码更新：
```
git fetch pm
git merge pm/master
```
因为指定了pm，只会获取pm的内容。然后与自己的合并。注意冲突，产生了需要自行解决。

更新到自己的代码库：
```
git push
```

因为默认关联自己的远程库，不用指定远程主机名。

## 写在后面
本文仅是Git的基本使用,可以作为新手入门教程。更多Git相关操作，请参考：
>1、Git教程,极力推荐，尤其是新手。本文就参考了该教程。<br/>
2、《Pro Git》一书,百度搜索吧;<br/>
3、Git官方教程






