# gitilite安装
````
#su root 
#git clone git://github.com/sitaramc/gitolite
#mkdir -p $HOME/bin  
./install -to $HOME/bin
#ssh-keygen -t rsa 
#gitolite setup -pk  /root/.ssh/id_rsa.pub 

```