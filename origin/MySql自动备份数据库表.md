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
tmp="2017-4-07 04:59:59"
STOPTIME=`date -d"$tmp" +%Y%m%d%H%M%S`
CURTIME=`date +%Y%m%d%H%M%S`

if [[ $CURTIME -ge $STOPTIME ]]
then
        exit 0
fi

# execute sql stat  
mysql -ufyweb -pfyweb_123 -e"
use mailinfo_tthl;
create table mailinfo_bk_${CURTIME} as select * from mailinfo;
create table twitterinfo_bk_${CURTIME} as select * from twitterinfo;
quit"
exit;
~~~
~~~shell
[root@localhost ~]# chmod 777 backupdatabase.sh
~~~

~~~shell
[root@localhost ~]# crontab -e
0 5 * * * /root/backupdatabase.sh
~~~