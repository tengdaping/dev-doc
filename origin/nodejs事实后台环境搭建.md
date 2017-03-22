#nodejs实时后台环境搭建
##virtualbox+centos6+samba+nodejs+pomelo
需要准备:virtualbox安装文件，`CentOS-6.3-x86_64-minimal.iso`
###virtualbox+centos6
###安装virtualbox
新建虚拟电脑
`类型`选择`Linux`
`版本`选择`Linux 2.6/3.x/4.x(64-bit)`
内存分配1G
`现在创建虚拟硬盘`
`VDI(virtualbox磁盘映像)`
`固定大小`
创建
启动
选择启动盘`CentOS-6.3-x86_64-minimal.iso`
`Install or upgrade an existing system`
设备菜单，选择`CentOS-6.3-x86_64-minimal.iso`
后续过程基本上点击下一步即可。
###配置Linux
启动，输入帐号密码
####网络
菜单 设备-->网络-->网卡1-->桥接网卡
~~~
[root@localhost ~]# vi /etc/sysconfig/network-scripts/ifcfg-eth0
...
ONBOOT="yes"
GATEWAY="192.168.1.1"
...
:wq
[root@localhost ~]# service network restart
[root@localhost ~]# ifconfig
~~~
获得ip 我的是`192.168.1.96`
启动babun
~~~
[~]ssh root@192.168.1.96
~~~
连接虚拟机，这种做法好处多多，在此不赘述。
###yum源配置
改为阿里云的源，下载速度快，效率高。
安装wget
~~~
[root@localhost ~]# yum -y install wget
~~~
备份
~~~
[root@localhost ~]# mv /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.backup
~~~
换源
~~~
[root@localhost ~]# wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-6.repo
~~~
~~~
[root@localhost ~]# wget -O /etc/yum.repos.d/epel.repo http://mirrors.aliyun.com/repo/epel-6.repo
~~~

###samba服务
参考网址 
[https://www.server-world.info/query?os=CentOS_6&p=samba](https://www.server-world.info/query?os=CentOS_6&p=samba)
安装
~~~
[root@localhost ~ ~]# yum -y install samba4 samba4-client
~~~
创建共享目录
~~~
[root@localhost ~]# mkdir /home/share
[root@localhost ~]# chmod 777 /home/share/
~~~
编辑配置文件
~~~
[root@localhost ~]# vi /etc/samba/smb.conf
~~~
~~~
...
#120行添加
map to guest = Bad User
...
#末尾添加
[Share]
	path = /home/share
    writable = yes
    guest ok = yes
    guest only = yes
    create mode = 0777
    directory mode = 0777
~~~

~~~
[root@localhost ~]# cd /home
[root@localhost home]# chmod 777 -R ./share
~~~

启动试一下
~~~
[root@localhost ~]# /etc/rc.d/init.d/smb start
Starting SMB services:                                     [  OK  ]
[root@localhost ~]# /etc/rc.d/init.d/nmb start
Starting NMB services:                                     [FAILED]
~~~
nmd启动失败
配置防火墙
~~~
[root@localhost ~]# iptables -I INPUT 5 -p tcp -m state --state NEW -m multiport --dports 139,445 -j ACCEPT
[root@localhost ~]# iptables -I INPUT 5 -p udp -m state --state NEW -m udp --dport 137 -j ACCEPT
~~~
关闭selinux，再次启动nmd
~~~
[root@localhost ~]# setenforce 0
[root@localhost ~]# /etc/rc.d/init.d/nmb start
Starting NMB services:                                     [  OK  ]
~~~
修改配置文件，设置启动即关闭
~~~
[root@localhost ~]# vi /etc/sysconfig/selinux
~~~
~~~
...
# SELINUX=enforcing
  SELINUX=disabled
...
~~~
在windows访问
`\\192.168.1.96\Share`

###安装nodejs
使用下载二进制编码方式安装node和npm，别用yum装了
下载安装包
~~~
[root@localhost ~]# cd /home/share/
[root@localhost share]# wget https://nodejs.org/dist/v6.10.1/node-v6.10.1-linux-x64.tar.xz
[root@localhost share]# yum install xz
[root@localhost share]# yum install tar
[root@localhost share]# xz -d node-v6.10.1-linux-x64.tar.xz
[root@localhost share]# tar -xvf node-v6.10.1-linux-x64.tar
[root@localhost share]# mv node-v6.10.1-linux-x64 /usr/local/bin
cd ~
vi .bash_proflie
...
#修改PATH
export PATH="$PATH:/usr/local/bin/node-v6.10.1-linux-x64/bin"
...
~~~
~~~
[root@localhost ~]# node -v
v6.10.1
~~~
###安装mysql
~~~
[root@localhost ~]# yum install mysql mysql-server mysql-devel -y
~~~
~~~
[root@localhost ~]# chkconfig --list|grep mysql
mysqld          0:off   1:off   2:off   3:off   4:off   5:off   6:off
[root@localhost ~]# chkconfig mysqld on
[root@localhost ~]# chkconfig --list|grep mysql
mysqld          0:off   1:off   2:on    3:on    4:on    5:on    6:off
[root@localhost ~]# service mysqld start
~~~
通过yum安装默认密码是空
设置密码
~~~
[root@localhost ~]# mysqladmin -u root -p password "xxxxxx"
~~~
记住设置的密码

###安装pomelo
~~~
[root@localhost ~]# cd /home/share/
[root@localhost share]# yum -y install git
[root@localhost share]# git clone https://github.com/NetEase/pomelo.git
[root@localhost share]# cd pomelo
[root@localhost pomelo]# npm install -g
~~~
参考网址
`https://github.com/NetEase/pomelo/wiki/%E5%AE%89%E8%A3%85pomelo`

