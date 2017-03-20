#SshKey生成和配置


检查sshkey是否存在
~~~
[root@localhost ~]# ls -al ~/.ssh
~~~
在命令行中输入
~~~
[root@localhost ~]# ssh-keygen -t rsa -C "your_email@example.com"
~~~
默认会在相应路径下（/your_home_path）生成id_rsa和id_rsa.pub两个文件，如下面代码所示
~~~
ssh-keygen -t rsa -C "your_email@example.com"
#Creates a new ssh key using the provided email
Generating public/private rsa key pair.
Enter file in which to save the key (/your_home_path/.ssh/id_rsa):
~~~
设置passphrase后，进行版本控制时，每次与GitHub通信都会要求输入passphrase，以避免某些“失误”
~~~
Enter passphrase (empty for no passphrase): [Type a passphrase]
Enter same passphrase again: [Type passphrase again]
~~~
(注，直接输入回车就是空的passphrase)

~~~
sample result:
Your identification has been saved in /your_home_path/.ssh/id_rsa.
Your public key has been saved in /your_home_path/.ssh/id_rsa.pub.
The key fingerprint is:
#01:0f:f4:3b:ca:85:d6:17:a1:7d:f0:68:9d:f0:a2:db your_email@example.com
~~~
这时可以在主机A上看到生成的秘钥~/.ssh/id_rsa 和公钥 ~/.ssh/ id_rsa.pub
把主机A的公钥放在主机B上
~~~
[root@localhost ~]# scp -r /root/.ssh/id_rsa.pub 192.168.31.147:/root/.ssh/authorized_keys
~~~

这里解释下scp命令：
不同的Linux之间copy文件常用有3种方法：
第一种就是ftp，也就是其中一台Linux安装ftp Server，这样可以另外一台使用ftp的client程序来进行文件的copy。
第二种方法就是采用samba服务，类似Windows文件copy 的方式来操作，比较简洁方便。
第三种就是利用scp命令来进行文件复制。
scp是有Security的文件copy，基于ssh登录。操作起来比较方便，比如要把当前一个文件copy到远程另外一台主机上，可以如下命令。
~~~
[root@localhost ~]# scp /home/daisy/full.tar.gz root@172.19.2.75:/home/root
~~~
然后会提示你输入另外那台172.19.2.75主机的root用户的登录密码，接着就开始copy了。

(注 额外妙招)
~~~
[root@localhost ~]# chmod 755 ~/.ssh/  
[root@localhost ~]# chmod 600 ~/.ssh/id_rsa ~/.ssh/id_rsa.pub   
[root@localhost ~]# chmod 644 ~/.ssh/known_hosts 
~~~