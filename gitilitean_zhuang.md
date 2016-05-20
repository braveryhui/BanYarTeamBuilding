# gitilite安装
参考链接
[gitolite-installation-step-by-step](http://www.bigfastblog.com/gitolite-installation-step-by-step)

````
#su root 
#git clone git://github.com/sitaramc/gitolite
#mkdir -p $HOME/bin  
./install -to $HOME/bin
#ssh-keygen -t rsa 
#gitolite setup -pk  /root/.ssh/id_rsa.pub 

```