#Linux学习路线
##13.查看ip
###命令格式
~~~
ifconfig [网络设备] [参数]
~~~
###命令功能
ifconfig 命令用来查看和配置网络设备。当网络环境发生改变时可通过此命令对网络进行相应的配置。
###命令参数
up 启动指定网络设备/网卡。
down 关闭指定网络设备/网卡。该参数可以有效地阻止通过指定接口的IP信息流，如果想永久地关闭一个接口，我们还需要从核心路由表中将该接口的路由信息全部删除。
arp 设置指定网卡是否支持ARP协议。
-promisc 设置是否支持网卡的promiscuous模式，如果选择此参数，网卡将接收网络中发给它所有的数据包
-allmulti 设置是否支持多播模式，如果选择此参数，网卡将接收网络中所有的多播数据包
-a 显示全部接口信息
-s 显示摘要信息（类似于 netstat -i）
add 给指定网卡配置IPv6地址
del 删除指定网卡的IPv6地址
<硬件地址> 配置网卡最大的传输单元
mtu<字节数> 设置网卡的最大传输单元 (bytes)
netmask<子网掩码> 设置网卡的子网掩码。掩码可以是有前缀0x的32位十六进制数，也可以是用点分开的4个十进制数。如果不打算将网络分成子网，可以不管这一选项；如果要使用子网，那么请记住，网络中每一个系统必须有相同子网掩码。
tunel 建立隧道
dstaddr 设定一个远端地址，建立点对点通信
-broadcast<地址> 为指定网卡设置广播协议
-pointtopoint<地址> 为网卡设置点对点通讯协议
multicast 为网卡设置组播标志
address 为网卡设置IPv4地址
txqueuelen<长度> 为网卡设置传输列队的长度
###使用实例
~~~
[root@localhost ~]# ifconfig
eth0      Link encap:Ethernet  HWaddr 00:50:56:BF:26:20  
          inet addr:192.168.120.204  Bcast:192.168.120.255  Mask:255.255.255.0
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:8700857 errors:0 dropped:0 overruns:0 frame:0
          TX packets:31533 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000 
          RX bytes:596390239 (568.7 MiB)  TX bytes:2886956 (2.7 MiB)

lo        Link encap:Local Loopback  
          inet addr:127.0.0.1  Mask:255.0.0.0
          UP LOOPBACK RUNNING  MTU:16436  Metric:1
          RX packets:68 errors:0 dropped:0 overruns:0 frame:0
          TX packets:68 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:0 
          RX bytes:2856 (2.7 KiB)  TX bytes:2856 (2.7 KiB)
~~~

##14查看CPU，内存、磁盘空间等

查看系统版本:lsb_release -a
~~~
[root@localhost /]# lsb_release -a
LSB Version:    :core-4.0-amd64:core-4.0-noarch:graphics-4.0-amd64:graphics-4.0-noarch:printing-4.0-amd64:printing-4.0-noarch
Distributor ID: CentOS
Description:    CentOS release 6.1 (Final)
Release:        6.1
Codename:       Final
~~~

查看内核版本:uname -a
~~~
[root@localhost /]# uname -a
Linux localhost.localdomain 2.6.32-131.0.15.el6.x86_64 #1 SMP Sat Nov 12 15:11:58 CST 2011 x86_64 x86_64 x86_64 GNU/Linux
~~~

查看cpu型号:cat /proc/cpuinfo
~~~
[root@localhost /]# cat /proc/cpuinfo
processor       : 0
vendor_id       : GenuineIntel
cpu family      : 6
model           : 26
model name      : Intel(R) Xeon(R) CPU           E5506  @ 2.13GHz
......
~~~

查看硬盘空间情况df -lm
~~~
[root@localhost /]# df -l
文件系统                 1K-块      已用      可用 已用% 挂载点
/dev/sda3            604758736  20379244 553659492   4% /
tmpfs                  8160484         0   8160484   0% /dev/shm
/dev/sda1               495844     35901    434343   8% /boot
/dev/sda5            302379456   4760128 282259328   2% /opt
~~~

查看内存:free -m 
~~~
[root@localhost /]# free -mt
             total       used       free     shared    buffers     cached
Mem:         15938      14204       1734          0        356      10597
-/+ buffers/cache:       3250      12688
Swap:        19997          0      19997
Total:       35936      14204      21732
~~~
Swap为虚拟内存，当物理内存不够时,系统就会使用到swap,但swap速度比真实内存的速度要慢的多,当你电脑用到swap的时候,说明你内存不够了,需要加内存


free
说    明：显示内存状态。
语　　法： free [-bkmotV][-s <间隔秒数>]
补充说明：free指令会显示内存的使用情况，包括实体内存，虚拟的交换文件内存，共享内存区段，以及系统核心使用的缓冲区等。
参　　数：
　-b 　以Byte为单位显示内存使用情况。
　-k 　以KB为单位显示内存使用情况。
　-m 　以MB为单位显示内存使用情况。
　-o 　不显示缓冲区调节列。
　-s<间隔秒数> 　持续观察内存使用状况。
　-t 　显示内存总和列。
　-V 　显示版本信息。

##15.数据库，MySql的使用(连接，选库，查表，备份)
###安装
####卸载掉原有mysql
通过如下命令来查看我们的操作系统上是否已经安装了mysql数据库
~~~
[root@localhost ~]# rpm -qa | grep mysql　　
//这个命令就会查看该操作系统上是否已经安装了mysql数据库
~~~
有的话，我们就通过 ```rpm -e``` 命令 或者 ```rpm -e --nodeps``` 命令来卸载掉
~~~
[root@localhost ~]# rpm -e mysql　　
//普通删除模式
[root@localhost ~]# rpm -e --nodeps mysql　　
//强力删除模式，如果使用上面命令删除时，提示有依赖的其它文件，则用该命令可以对其进行强力删除。
~~~
在删除完以后我们可以通过 ```rpm -qa | grep mysql``` 命令来查看mysql是否已经卸载成功。

####通过yum来进行mysql的安装

我是通过yum的方式来进行mysql的数据库安装，首先我们可以输入 ```yum list | grep mysql``` 命令来查看yum上提供的mysql数据库可下载的版本：
~~~
[root@localhost ~]# yum list | grep mysql
~~~
就可以得到yum服务器上mysql数据库的可下载版本信息。
####安装
~~~
[root@localhost ~]# yum install -y mysql-server mysql mysql-devel
~~~
###操作
####启动
~~~
[root@localhost ~]# service mysqld start
~~~
~~~
[root@localhost ~]# service mysqld restart
~~~
我们在使用mysql数据库时，都得首先启动mysqld服务，可以通过  ```chkconfig --list | grep mysqld``` 命令来查看mysql服务是不是开机自动启动，如：
~~~
[root@localhost ~]# chkconfig --list | grep mysqld
mysqld      0:关闭    1:关闭    2:关闭    3:关闭    4:关闭    5:关闭    6:关闭
~~~
发现mysqld服务并没有开机自动启动，可以通过 ```chkconfig mysqld on``` 命令来将其设置成开机启动，这样就不用每次都去手动启动了 
~~~
[root@localhost ~]# chkconfig mysqld on
[root@localhost ~]# chkconfig --list | grep mysql
mysqld      0:关闭    1:关闭    2:启用    3:启用    4:启用    5:启用    6:关闭
~~~
mysql数据库安装完以后只会有一个root管理员账号，但是此时的root账号还并没有为其设置密码，在第一次启动mysql服务时，会进行数据库的一些初始化工作，在输出的一大串信息中，看到有这样一行信息 ：

~~~
/usr/bin/mysqladmin -u root password 'new-password'　　// 为root账号设置密码
~~~

所以我们可以通过 该命令来给我们的root账号设置密码(注意：这个root账号是mysql的root账号，非Linux的root账号)
~~~
[root@localhost ~]# mysqladmin -u root password '123456'　　// 通过该命令给root账号设置密码为 123456
~~~
此时我们就可以通过 mysql -u root -p 命令来登录mysql数据库了

###选库
~~~
[root@localhost]# mysql -u root -p
Enter password:******
mysql> use MYSQLDATABASENAME;
Database changed
mysql>
~~~
##16.web服务器的应用(Apache/Nginx/Node.js)
###Apache
安装Apache前准备：
1.检查该环境中是否已经存在httpd服务的配置文件，默认存储路径：/etc/httpd/httpd.conf（这是centos预装的Apache的一个ent版本，一般我们安装源代码版的Apache）。如果已经存在/etc/httpd/httpd.conf，请先卸载或者关闭centos系统自带的web服务，执行命令：chkconfig  httpd off，再或者把centos自带的httpd服务的80端口改为其他端口，只要不与我们安装的Apache服务的端口冲突就可以啦。
停止并卸载Linux系统自带的httpd服务：
```service httpd stop```
```ps -ef | grep httpd```
```kill -9 pid号（逐个删除）```
```rpm -qa |grep httpd```
```rpm -e httpd软件包```
~~~
[root@localhost ~]# find / -name httpd.conf
[root@localhost ~]#
~~~
2.安装Apache
~~~
[root@localhost ~]# yum install httpd
~~~
3.设置Apache服务的启动级别
~~~
[root@localhost ~]# chkconfig --levels 235 httpd on
~~~
Apache是一个服务，所以，可以通过设置服务的启动级别来让它启动。
4.启动
~~~
[root@localhost ~]# /etc/init.d/httpd start
~~~
安装之后的说明：
1.验证是否安装成功
打开```http://ip```地址，看看有没有Apache的默认页面出来了？如果有就对了。
2.安装目录介绍
Apache默认将网站的根目录指向/var/www/html 目录 
默认的主配置文件是/etc/httpd/conf/httpd.conf
配置存储在的/etc/httpd/conf.d/目录
##17.防火墙，开/闭 端口，添加限制
临时关闭，即时生效，重启失效
~~~
# 关闭防火墙
[root@localhost ~]# service iptables stop
# 开启防火墙
[root@localhost ~]# service iptables start
~~~
永久关闭，重启生效
~~~
# 关闭防火墙
[root@localhost ~]# chkconfig iptables off
# 开启防火墙
[root@localhost ~]# chkconfig iptables on
~~~
```iptables -t filter -A INPUT -s 192.168.1.5 -i eth0 -j DROP```
禁止IP为192.168.1.5的主机从eth0访问本机
```iptables -t filter -I INPUT 2 -s 192.168.5.0/24 -p tcp --dport 80 -j DROP```
禁止子网192.168.5.0访问web服务
```iptables -t filter -I INPUT 2 -s 192.168.7.9 -p tcp --dport ftp -j DROP```
禁止IP为192.168.7.9访问FTP服务
```iptables -t filter -L INPUT```
查看filter表中INPUT链的规则
```iptables -t nat -F```
删除nat表中的所有规则
```iptables -I FORWARD -d wwww.playboy.com -j DROP```
禁止访问www.playboy.com网站
```iptables -I FORWARD -s 192.168.5.23 -j DROP```
禁止192.168.5.23上网

##18.命令行下的FTP,SFTP
ftp安装部分，操作步骤如下：
可以使用yum命令直接安装ftp
~~~
[root@localhost ~]# yum install vsftpd
~~~
ftp服务的开启与关闭命令：

开启：
[root@localhost ~]# service vsftpd start

关闭:
[root@localhost ~]# service vsftpd stop

安装成功后，可以在本地使用ftp软件连接，默认账号是虚拟机的账号和密码。

在linux中添加ftp用户，并设置相应的权限，操作步骤如下：

1.环境：
ftp为vsftp 被限制用户名为test。被限制路径为/home/test
2.建用户：在root用户下：
~~~
[root@localhost ~]# useradd -d /home/test test
//增加用户test，并制定test用户的主目录为/home/test
[root@localhost ~]# passwd test
//为test设置密码
~~~
3.更改用户相应的权限设置：
~~~
[root@localhost ~]# usermod -s /sbin/nologin test
//限定用户test不能telnet，只能ftp
[root@localhost ~]# usermod -s /sbin/bash test
//用户test恢复正常
[root@localhost ~]# usermod -d /test test
//更改用户test的主目录为/test
~~~
4.限制用户只能访问/home/test，不能访问其他路径：
修改 
~~~
/etc/vsftpd/vsftpd.conf
~~~
如下：
~~~
chroot_list_enable=YES //限制访问自身目录
# (default follows)
chroot_list_file=/etc/vsftpd/vsftpd.chroot_list
~~~
编辑 vsftpd.chroot_list文件，将受限制的用户添加进去，每个用户名一行
改完配置文件，不要忘记重启vsftpd服务器。
~~~
[root@localhost ~]# /etc/init.d/vsftpd restart
~~~
5.如果需要允许用户修改密码，但是又没有telnet登录系统的权限：
~~~
[root@localhost ~]#  usermod -s /usr/bin/passwd test //用户telnet后将直接进入改密界面
~~~
##19.hosts文件修改
~~~
/etc/hosts
~~~
~~~
[root@localhost ~]# vi /etc/hosts
~~~

##20.文件搜索，按内容(find,grep)
1.在某个路径下查文件。
在/etc下查找“*.log”的文件
~~~
[root@localhost ~]# find /etc -name “*.log”
~~~
2.扩展，列出某个路径下所有文件，包括子目录。
~~~
[root@localhost ~]# find /etc -name “*”
~~~
3.在某个路径下查找所有包含“hello abcserver”字符串的文件。
~~~
[root@localhost ~]# find /etc -name “*” | xargs grep “hello abcserver”
~~~
或者
~~~
[root@localhost ~]# find /etc -name “*” | xargs grep “hello abcserver” > ./cqtest.txt
~~~

grep命令使用简单实例
~~~
[root@localhost ~]# grep 'test' d*
~~~
显示所有以d开头的文件中包含 test的行。
~~~
[root@localhost ~]# grep 'test' aa bb cc
~~~
显示在aa，bb，cc文件中匹配test的行。
~~~
[root@localhost ~]# grep '[a-z]\{5\}' aa
~~~
显示所有包含每个字符串至少有5个连续小写字符的字符串的行。
~~~
[root@localhost ~]# grep 'w\(es\)t.*\1' aa
~~~
如果west被匹配，则es就被存储到内存中，并标记为1，然后搜索任意个字符(.\*)，这些字符后面紧跟着 另外一个es(\1)，找到就显示该行。如果用egrep或grep -E，就不用”\”号进行转义，直接写成'w(es)t.*\1′就可以了。