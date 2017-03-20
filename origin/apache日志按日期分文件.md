#apache日志按日期分文件

^pasta_ping^

两个配置文件所在位置为
~~~
httpd.conf:
/etc/httpd/conf/httpd.conf
ssl.conf:
/etc/httpd/conf.d/ssl.conf
~~~

首先请备份两个文件
~~~
cp httpd.conf httpd.conf.DATE.bk
cp ssl.conf ssl.conf.DATE.bk
~~~

修改以下字段
~~~
httpd.conf
ErrorLog "|/usr/sbin/rotatelogs /etc/httpd/logs/%Y_%m_%d_error_log 86400 540"
CustomLog "|/usr/sbin/rotatelogs /etc/httpd/logs/%Y_%m_%d_access_log 86400 540"
~~~

~~~
ssl.conf
<VirtualHost _default_:443>
ErrorLog "|/usr/sbin/rotatelogs /etc/httpd/logs/%Y_%m_%d_ssl_error_log 86400 540"
TransferLog "|/usr/sbin/rotatelogs /etc/httpd/logs/%Y_%m_%d_ssl_access_log 86400 540"
~~~
重启apache服务
~~~
service httpd restart
~~~

说明
1.使用Apache自带的轮循工具rotatelogs来对日志文件进行轮循。rotatelogs基本是按时间或大小来控制日志的。
2.86400是一天的秒数
3.540是时区偏移量，由于日本服务器是+0900，故偏移量=9*60，可以用``date -R``查看本地偏移量




