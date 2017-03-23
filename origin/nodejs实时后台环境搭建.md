# nodejs实时后台环境搭建
pasta_ping
March 22, 2017 10:40 PM

- - -

开动前的材料准备：
- 	VirtualBox安装文件
-	CentOS-6.3-x86_64-minimal.iso
-	Windows/Mac宿主机(我的是Win10)
-	babun1.1.0

- - -

#### 安装VirtualBox
新建虚拟电脑
-	类型选择`Linux`
-	版本选择`Linux 2.6/3.x/4.x(64-bit)`
-	内存分配1G
-	现在创建虚拟硬盘
	VDI(virtualbox磁盘映像)
    固定大小
-	创建
-	启动
	选择启动盘`CentOS-6.3-x86_64-minimal.iso`
	`Install or upgrade an existing system`
	设备菜单，选择`CentOS-6.3-x86_64-minimal.iso`
-	后续过程基本上点击下一步即可。

- - -

#### 配置Linux
-	启动，输入帐号密码

##### 网络
-	菜单 设备-->网络-->网卡1-->桥接网卡
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

##### yum源配置
改为阿里云的源，下载速度快且稳定。
-	安装wget
~~~
[root@localhost ~]# yum -y install wget
~~~
-	备份原yum源
~~~
[root@localhost ~]# mv /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.backup
~~~
-	更换yum源
~~~
[root@localhost ~]# wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-6.repo
~~~
-	下载epel源
~~~
[root@localhost ~]# wget -O /etc/yum.repos.d/epel.repo http://mirrors.aliyun.com/repo/epel-6.repo
~~~

##### samba服务
参考网址 
`https://www.server-world.info/query?os=CentOS_6&p=samba`
-	安装
~~~
[root@localhost ~ ~]# yum -y install samba4 samba4-client
~~~
-	创建共享目录
~~~
[root@localhost ~]# mkdir /home/share
//授权操作，后面也需要
[root@localhost ~]# chmod 777 -R /home/share/
~~~
-	编辑配置文件
~~~
[root@localhost ~]# vi /etc/samba/smb.conf
~~~
~~~
...
#120行添加
map to guest = Bad User
...
#文件末尾添加
[Share]
	path = /home/share
    writable = yes
    guest ok = yes
    guest only = yes
    create mode = 0777
    directory mode = 0777
~~~

-	尝试启动一下
~~~
[root@localhost ~]# /etc/rc.d/init.d/smb start
Starting SMB services:                                     [  OK  ]
[root@localhost ~]# /etc/rc.d/init.d/nmb start
Starting NMB services:                                     [FAILED]
~~~
nmd启动失败
-	配置Linux防火墙
~~~
[root@localhost ~]# iptables -I INPUT 5 -p tcp -m state --state NEW -m multiport --dports 139,445 -j ACCEPT
[root@localhost ~]# iptables -I INPUT 5 -p udp -m state --state NEW -m udp --dport 137 -j ACCEPT
~~~
-	关闭selinux，再次启动nmd
~~~
[root@localhost ~]# setenforce 0
[root@localhost ~]# /etc/rc.d/init.d/nmb start
Starting NMB services:                                     [  OK  ]
~~~
-	修改selinux配置文件，设置启动即关闭
~~~
[root@localhost ~]# vi /etc/sysconfig/selinux
~~~
~~~
...
# SELINUX=enforcing
  SELINUX=disabled
...
~~~
-	从Windows资源管理器尝试访问
`\\192.168.1.96\Share`
此后可以用sublimetext在Windows下操作工程了。

- - -

#### 安装nodejs
由于yum方式下载的nodejs版本过低，故使用下载二进制编码方式安装node和npm
-	下载安装包
~~~
[root@localhost ~]# cd /home/share/
[root@localhost share]# wget https://nodejs.org/dist/v6.10.1/node-v6.10.1-linux-x64.tar.xz
[root@localhost share]# yum install xz
[root@localhost share]# yum install tar
[root@localhost share]# xz -d node-v6.10.1-linux-x64.tar.xz
[root@localhost share]# tar -xvf node-v6.10.1-linux-x64.tar
[root@localhost share]# mv node-v6.10.1-linux-x64 /usr/local/bin
~~~
-	修改PATH
~~~
[root@localhost share]# cd ~
[root@localhost ~]# vi .bash_proflie
...
export PATH="$PATH:/usr/local/bin/node-v6.10.1-linux-x64/bin"
...
~~~
-	查看node版本
~~~
[root@localhost ~]# node -v
v6.10.1
~~~

- - -

#### 安装mysql
-	yum安装
~~~
[root@localhost ~]# yum install mysql mysql-server mysql-devel -y
~~~
-	设置mysql
~~~
[root@localhost ~]# chkconfig --list|grep mysql
mysqld          0:off   1:off   2:off   3:off   4:off   5:off   6:off
[root@localhost ~]# chkconfig mysqld on
[root@localhost ~]# chkconfig --list|grep mysql
mysqld          0:off   1:off   2:on    3:on    4:on    5:on    6:off
[root@localhost ~]# service mysqld start
~~~
-	通过yum安装默认密码是空的，设置密码
~~~
[root@localhost ~]# mysqladmin -u root -p password "yourpassword"
~~~


- - -

#### 安装pomelo
-	参考网址
`https://github.com/NetEase/pomelo/wiki/%E5%AE%89%E8%A3%85pomelo`
-	安装git
~~~
[root@localhost ~]# cd /home/share/
[root@localhost share]# yum -y install git
~~~
-	clone下来
~~~
[root@localhost share]# git clone https://github.com/NetEase/pomelo.git
[root@localhost share]# cd pomelo
~~~
-	全局安装pomelo
~~~
[root@localhost pomelo]# npm install -g
~~~




