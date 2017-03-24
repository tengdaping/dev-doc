# Mysql自动备份数据库表
pasta_ping


环境:

- centos6

- crontab

#### crontab配置
crontab是强大好用的定时执行命令工具
-	安装
~~~shell
[root@localhost ~]# yum install -y vixie-cron
[root@localhost ~]# chkconfig –level 35 crond on
~~~
-	配置定时任务
~~~shell
[root@localhost ~]# crontab -e
~~~

- 格式说明


|    *     |    *     |    *    |    *    |    *     | command |
| :------: | :------: | :-----: | :-----: | :------: | :-----: |
| 分钟(0-59) | 小时(0-23) | 日(1-31) | 月(1-12) | 星期几(0-6) |  执行的命令  |

- 配置后需重启服务
~~~shell
[root@localhost ~]# service cronb restart
~~~

#### shell编写

- `backupdatabase.sh`

~~~shell
#!/bin/bash  
#将mailinfo_tthl库中的mailinfo和twitterinfo表加上时间标记的名字备份
TIMESTAMP=`date +%Y%m%d%H%M%S`
echo "Start execute sql statement at `date`."
# execute sql stat  
mysql -uusername -ppassword -e"
use mailinfo_tthl;
create table mailinfo_bk_${TIMESTAMP} as select * from mailinfo;
create table twitterinfo_bk_${TIMESTAMP} as select * from twitterinfo;
quit"
exit;
~~~
~~~shell
[root@localhost ~]# chmod 777 backupdatabase.sh
~~~

-	`stopbackup.sh`
~~~shell
#!/bin/bash
#更换crontab配置
TIMESTAMP=`date +%Y%m%d%H%M%S`
echo "Stop backup mailinfo&twitterinfo at "$TIMESTAMP"."
crontab -l > /tmp/crontab.$TIMESTAMP.bk
crontab /tmp/crontab.bk
service crond restart
~~~
~~~shell
[root@localhost ~]# chmod 777 stopbackup.sh
~~~

~~~shell
[root@localhost ~]# crontab -e
#每天早上五点备份
0 5 * * * /root/backupdatabase.sh
#4月7日5：01失效
1 5 7 4 * /root/stopbackup.sh
~~~

#### 部署
-	在/tmp下准备crontab.bk文件，在此案例中，这个文件是空的。