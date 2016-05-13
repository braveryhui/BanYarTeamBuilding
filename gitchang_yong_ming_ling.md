# Git常用命令
生成公钥
ssh-keygen -t rsa

PC用户端上传公钥到服务器 
scp ~/.ssh/id_rsa.pub git@115.28.212.80:/tmp/

克隆目录
 git clone git@115.28.212.80:/App/shell/repositories/www.banyar.cn.git
/App/shell

git add .  #检查所有本地所有文件的改动 并准备同步  
git commit -m "add comment"  添加我同步上去的 描述 一般是添加了那些功能
git pull   #把服务器上行的代码同步下 除了我改动过的，其他的人改动同时同步过来
git push #把本地的改动同步到服务器上面 


git log -p -2	查看最近提交日志

git checkout 95d0d66d6b2b3feddc1cd792c2e4678928399ece Lib/Action/Admin/InitAc
ion.class.php	git checkout + 版本号 + 需要恢复的文件路径



git config --global user.email "buckyang@163.com"
git config --global user.name "雅方同学"



git tag 相关操作（打标签 checkout 提交 删除 合并）

打标签
- git add .
- git commit -m [log名]
- git tag -a [标签名] -m [备注]
- git push

提交单个标签到git上
- git push [标签名]
提交所有的标签到git上
- git push origin --tags

 查看当前的分支
- git branch

 列出所有的tag
- git tag

切换到指定的分支
- git checkout [标签名]

切换到主分支
- git checkout master

合并指定的tag到主分支
- git merge [标签名]

删除标签
- git tag -d [标签名]



更改git密码：

passwd git


#!/bin/sh
unset GIT_DIR
PathVersion=/App/banyar1.1/www.banyar.cn

git-update-server-info
git --work-tree=/App/banyar checkout -f

cd $PathVersion
git add .
git pull origin version1.1_local


＃分支操作 比如开发A新建一个分支 并且同步到服务器上
git branch                #查看分支 
git branch v1.1.5        ＃新建分支 
git checkout v1.1.5       #切换到v1.1.5分支上 

git add .
git commit -m "dele code in slidebase"
git pull origin v1.1.5
git push origin v1.1.5:v1.1.5   #将本地的分支推送到远程服务器上

#多人协作一个分支的时候检出 
git checkout origin/version1.2.2 -b local_version1.2.2  ＃拉取服务器上的 origin/version1.2.2 分支 并且 新建local_version1.2.2本地分支同步origin/version1.2.2的代码 
                          ＃该命令会直接切换 到新建的 local_version1.2.2本地分支上 
 git add .
 git commit -m "push file braveryhui2.txt"
 git pull
 git push origin HEAD:version1.2.2