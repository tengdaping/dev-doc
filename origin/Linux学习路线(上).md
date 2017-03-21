#Linux学习路线
^pasta_ping^
##1.开机、关机
重启命令：
~~~
[root@localhost /]# reboot
[root@localhost /]# shutdown -r now
//立刻重启(root用户使用)
[root@localhost /]# shutdown -r 10
//过10分钟自动重启(root用户使用)
[root@localhost /]# shutdown -r 20:35
//在时间为20:35时候重启(root用户使用)  如果是通过shutdown命令设置重启的话，可以用shutdown -c命令取消重
~~~

关机命令：
~~~
[root@localhost /]# halt
//立刻关机
[root@localhost /]# poweroff
//立刻关机
[root@localhost /]# shutdown -h now
//立刻关机(root用户使用) 
[root@localhost /]# shutdown -h 10
//10分钟后自动关机  如果是通过shutdown命令设置关机的话，可以用shutdown -c命令取消重启
~~~
##2.远程连接到某台Linux(ssh)
推荐使用Tera
https://ttssh2.osdn.jp/
登录时选择ssh
##3.环境变量PATH
Linux下环境变量设置的三种方法：
如想将一个路径加入到$PATH中，可以像下面这样做：
1.控制台中设置，不赞成这种方式，因为他只对当前的shell 起作用，换一个shell设置就无效了：
~~~
[root@localhost /]# PATH="$PATH":/NEW_PATH
//关闭shell Path会还原为原来的path
~~~
2.修改 /etc/profile 文件，如果你的计算机仅仅作为开发使用时推存使用这种方法，因为所有用户的shell都有权使用这个环境变量，可能会给系统带来安全性问题。这里是针对所有的用户的，所有的shell
在/etc/profile的最下面添加：
~~~
export  PATH="$PATH:/NEW_PATH"
~~~
3.修改bashrc文件，这种方法更为安全，它可以把使用这些环境变量的权限控制到用户级别，这里是针对某一特定的用户，如果你需要给某个用户权限使用这些环境变量，你只需要修改其个人用户主目录下的 .bashrc文件就可以了。
在下面添加：
~~~
export  PATH="$PATH:/NEW_PATH"
~~~
##4.修改.profile文件和.bashrc文件
##5.touch,cat命令

###cat命令
cat命令连接文件并打印到标准输出设备上。cat经常用来显示文件的内容，类似于下的type命令。

一般格式：
```cat [选项] 文件```
说明：该命令有两项功能，其一是用来显示文件的内容，它依次读取由参数file所指明的文件，将它们的内容输出到标准输出上；其二是连接两个或多个文件，如 cat fl f2 > f3将把文件fl和几的内容合并起来，然后通过输出重定向符“>”的作用，将它们放入文件f3中。
常用选项：
-b，--number-noblank 从1开始对所有非空输出行进行编号。
-n，--number 从1开始对所有输出行编号。
-s，--squeeze-blank 将多个相邻的空行合并成一个空行。
-help 打印该命令用法，并退出，其返回码表示成功。
注意：当文件较大时，文本在屏幕上迅速闪过（滚屏），用户往往看不清所显示的内容。因此，一般用more等命令分屏显示。为了控制滚屏，可以按Ctrl+S键，停止滚屏；按Ctrl+Q键可以恢复滚屏。按Ctrl+C（中断）键可以终止该命令的执行，并且返回Shell提示符状态。
示例：（设ml和m2是当前目录下的两个文件）
~~~
[root@localhost /]# cat m1
//屏幕上显示文件ml的内容
[root@localhost /]# cat m1 m2
//同时显示文件ml和m2的内容
[root@localhost /]# cat m1 m2 > file
//将文件ml和m2合并后放入文件file中
~~~

###touch命令
可以修改指定文件的时间标签或者创建一个空文件。

一般格式：
```touch [选项] 文件名...```
说明：touch命令将会修改指定文件的时间标签，把已存在文件的时间标签更新为系统当前的时间（默认方式），它们的数据将原封不动地保留下来。如果该文件尚未存在，则建立一个空的新文件。
选项：
-a 仅改变指定文件的存取时间。
-c 不创建任何文件。
-m 仅改变指定文件的修改时间。
-t STAMP 使用STAMP指定的时间标签，而不是系统当前的时间。STAMP的格式为[[CC]YY]MMDDhhmm[.ss]，其中，CC表示年份的前两位，YY表示年份的后两位，MM表示月份，dd表示日期，hh表示小时，mm表示分钟，ss表示秒。
示例：
[root@localhost /]# touch ex2 	//在当前目录下建立一个空文件ex2。
然后，利用ls -l命令可以发现文件ex2的大小为0，表示它是空文件。
##6.VIM
(插入内容，删除一行，删除一个字，跳到行尾、行首，跳到文件头、文件尾，字符串查找、替换，强制保存退出，Linenum)
一般模式常用操作
[h(或向左方向键)] 光标左移一个字符
[j(或向下方向键)] 光标下移一个字符
[k(或向上方向键)] 光标上移一个字符
[l(或向右方向键)] 光标右移一个字符

[[Ctrl] + f] 屏幕向下移动一页（相当于Page Down键）
[[Ctrl] + b] 屏幕向上移动一页（相当于Page Up键）

[[0]或[Home]] 光标移动到当前行的最前面
[[$]或[End]] 光标移动到当前行的末尾

[G] 光标移动到文件的最后一行（第一个字符处）
[nG] n为数字（下同），移动到当前文件中第n行
[gg] 移动到文件的第一行，相当于"1G"
[n[Enter]] 光标向下移动n行

[/word] 在文件中查找内容为word的字符串（向下查找）
[?word] 在文件中查找内容为word的字符串（向上查找）
[[n]] 表示重复查找动作，即查找下一个
[[N]] 反向查找下一个
[:n1,n2s/word1/word2/g] n1、n2为数字，在第n1行到第n2行之间查找word1字符串，并将其替换成word2
[:1,$s/word1/word2/g] 从第一行（第n行同理）到最后一行查找word1注册，并将其替换成word2
[:1,$s/word1/word2/gc] 功能同上，只不过每次替换时都会让用户确认

[x,X] x为向后删除一个字符，相当于[Delete]，X为向前删除一个字符，相当于[Backspace]
[dd] 删除光标所在的一整行
[ndd] 删除光标所在的向下n行

[yy] 复制光标所在的那一行
[nyy] 复制光标所在的向下n行
[p,P] p为将已经复制的数据在光标下一行粘贴；P为将已经复制的数据在光标上一行粘贴

[u] 撤消上一个操作
[[Ctrl] + r] 多次撤消
[.] 这是小数点键，重复上一个操作

一般模式切换到编辑模式的操作
１、进入插入模式（６个命令）
[i] 从目前光标所在处插入
[I] 从目前光标
[a] 从当前光标所在的下一个字符处开始插入
[A] 从光标所在行的最后一个字符处开始插入
[o] 英文小写字母o，在目前光标所在行的下一行处插入新的一行并开始插入
[O] 英文大写字母O，在目前光标所在行的上一行处插入新的一行并开始插入
2、进入替换模式（2个命令）
[r] 只会替换光标所在的那一个字符一次
[R] 会一直替换光标所在字符，直到按下[ESC]键为止
[[ESC]] 退出编辑模式回到一般模式

一般模式切换到命令行模式
[:w] 保存文件
[:w!] 若文件为只读，强制保存文件
[:q] 离开vi
[:q!] 不保存强制离开vi
[:wq] 保存后离开
[:wq!] 强制保存后离开
[:! command] 暂时离开vi到命令行下执行一个命令后的显示结果
[:set nu] 显示行号
[:set nonu] 取消显示行号
[:w newfile] 另存为

文件恢复模式
[[O]pen Read-Only] 以只读方式打开文件
[[E]dit anyway] 用正常方式打开文件，不会载入暂存文件内容
[[R]ecover] 加载暂存文件内容
[[D]elete it] 用正常方式打开文件并删除暂存文件
[[Q]uit] 按下q就离开vi，不进行其他操作
[[A]bort] 与quit功能类似

块选择（一般模式下用）
[v,V] v:将光标经过的地方反白选择；V：将光标经过的行反白选择
[[Ctrl] + v] 块选择，可用长方形的方式选择文本
[y] 将反白的地方复制到剪贴板
[d] 将反白的内容删除

多文件编辑
[vim file1 file2] 同时打开两个文件
[:n] 编辑下一个文件
[:N] 编辑上一个文件
[:files] 列出当前用vim打开的所有文件

多窗口功能
[:sp [filename]] 打开一个新窗口，显示新文件，若只输入:sp，则两窗口显示同一个文件
[[Ctrl] + w + j] 光标移动到下方窗口
[[Ctrl] + w + k] 光标移动到上方窗口
[[Ctrl] + w + q] 离开当前窗口

vim配置文件
vim的配置文件为/etc/vimrc，但一般不建议直接修改这个文件，而是在用户根目录下创建一个新的隐藏文件：
vim ~/.vimrc
然后编辑这个文件，常用的配置如下：
"双引号后面的内容为注释
set nu "显示行号
set hlsearch "查找的字符串反白显示
set backspace=2 "可随时用退格键进行删除
set autoindent "自动缩排
set ruler "在最下方一行显示状态
set showmode "在左下角显示模式
set bg=dark "显示不同的底色，还可以为light
syntax on "语法检验，颜色显示

Dos与Linux的断行字符（文件转化）
dos2unix [-kn] file [newfile]
unix2dos [-kn] file [newfile]
-k:保留该文件原本的mtime时间格式
-n:保留原本旧文件，将转换后的内容输出到新文件

语系编码转化
iconv --list 列出iconv支持的语系编码
iconv -f 原本编码 -t 新编码 filename [-o new file]
-f:from,后接原本的编码格式
-t:to,后接新编码格式
-o file:可选参数，建立新文件
例：将/tmp/a.txt从big5编码格式转换成utf8编码格式：
iconv -f big5 -t utf8 /tmp/a.txt -o new.txt

##7.删除文件、文件夹，创建文件夹，移动文件夹
```rm [-dfirv][--help][--version][文件或目录...]```
删除
~~~
[root@localhost /]# rm -rf /var/log/httpd/access
~~~
需要提醒的是：使用这个rm -rf的时候一定要格外小心，linux没有回收站的
~~~
[root@localhost /]# rm -f  fileName
~~~
创建文件夹
~~~
[root@localhost /]# mkdir  test
~~~
移动文件夹
~~~
[root@localhost /]# mv [-fiv] source destination
~~~
##8.压缩(tar,unzip)解压缩
常用的tar命令的组合选项是
~~~
[root@localhost /]# tar -xzvf filename.tar.gz
[root@localhost /]# tar -czvf filename.tar.gz file1 file2 ...
~~~
-f选项必须出现在选项参数的最后

-c:	建立压缩档案
-x：解压
-t：查看内容
-r：向压缩归档的文件末尾追加文件
-u：更新原压缩包中的文件
这五个是独立的选项，压缩解压都要用到其中一个，可以和别的选项一起使用，但是这5个只能出现其中一个

-v：压缩解压过程中显示文件
-f: 使用档名，注，f选项后必须跟文档名不能跟其他选项，知道为什么f 选项，总是在参数选项的最后一个出现了吧
-j ：是否同时具有 bzip2 的属性？亦即是否需要用 bzip2 压缩？
-p ：使用原文件的原来属性（属性不会依据使用者而变）
-P ：可以使用绝对路径来压缩！
-N ：比后面接的日期(yyyy/mm/dd)还要新的才会被打包进新建的文件中！
--exclude FILE：在压缩的过程中，不要将 FILE 打包！

unzip详解
-c 将解压缩的结果显示到屏幕上，并对字符做适当的转换。
-f 更新现有的文件。
-l 显示压缩文件内所包含的文件。
-p 与-c参数类似，会将解压缩的结果显示到屏幕上，但不会执行任何的转换。
-t 检查压缩文件是否正确。
-u 与-f参数类似，但是除了更新现有的文件外，也会将压缩文件中的其他文件解压缩到目录中。
-v 执行是时显示详细的信息。
-z 仅显示压缩文件的备注文字。
-a 对文本文件进行必要的字符转换。
-b 不要对文本文件进行字符转换。
-C 压缩文件中的文件名称区分大小写。
-j 不处理压缩文件中原有的目录路径。
-L 将压缩文件中的全部文件名改为小写。
-M 将输出结果送到more程序处理。
-n 解压缩时不要覆盖原有的文件。
-o 不必先询问用户，unzip执行后覆盖原有文件。
-P<密码> 使用zip的密码选项。
-q 执行时不显示任何信息。
-s 将文件名中的空白字符转换为底线字符。
-V 保留VMS的文件版本信息。
-X 解压缩时同时回存文件原来的UID/GID。
-d<目录> 指定文件解压缩后所要存储的目录。
-x<文件> 指定不要处理.zip压缩文件中的哪些文件。
-Z unzip -Z等于执行zipinfo指令

##9.执行脚本(shell)
方法一：切换到shell脚本所在的目录（此时，称为工作目录）执行shell脚本：
代码如下:
~~~
[root@localhost /]# cd /data/shell
[root@localhost /]# ./hello.sh
~~~
./的意思是说在当前的工作目录下执行hello.sh。如果不加上./，bash可能会响应找到不到hello.sh的错误信息。因为目前的工作目录（/data/shell）可能不在执行程序默认的搜索路径之列,也就是说，不在环境变量PASH的内容之中。查看PATH的内容可用 echo $PASH 命令。现在的/data/shell就不在环境变量PASH中的，所以必须加上./才可执行。
方法二：以绝对路径的方式去执行bash shell脚本：
代码如下:
~~~
[root@localhost /]# /data/shell/hello.sh
~~~
方法三：直接使用bash 或sh 来执行bash shell脚本：
代码如下:
~~~
[root@localhost /]# cd /data/shell
[root@localhost /]# bash hello.sh
~~~
或
~~~
[root@localhost /]# cd /data/shell
[root@localhost /]# sh hello.sh
~~~

注意，若是以方法三的方式来执行，那么，可以不必事先设定shell的执行权限，甚至都不用写shell文件中的第一行（指定bash路径）。因为方法三是将hello.sh作为参数传给sh(bash)命令来执行的。这时不是hello.sh自己来执行，而是被人家调用执行，所以不要执行权限。那么不用指定bash路径自然也好理解了。
方法四：在当前的shell环境中执行bash shell脚本：
复制代码代码如下:
~~~
[root@localhost /]# cd /data/shell
[root@localhost /]# . hello.sh
~~~
或
复制代码代码如下:
~~~
[root@localhost /]# cd /data/shell
[root@localhost /]# source hello.sh
~~~
前三种方法执行shell脚本时都是在当前shell（称为父shell）开启一个子shell环境，此shell脚本就在这个子shell环境中执行。shell脚本执行完后子shell环境随即关闭，然后又回到父shell中。而方法四则是在当前shell中执行的。
##10.修改权限(chmod,chown)
ll指令的显示的信息为（当前目录下只有nameservice1一个目录）：
drwxr-xr-x 3 hdfs hdfs 4096 4月  14 16:19 nameservice1
上述信息分别表示：权限（drwxr-xr-x 3）、所属用户（hdfs）和组（hdfs）、大小(4096)、时间(4月 14 16:19)、名称(nameservice1)。
权限中的字母一共有10位数：
其中，
第1位有两种选择：-表示是文件，d表示是目录。此处是d，表示nameservice1是目录；
第2位到第4位rwx表示的是所有者（所属用户hdfs）的权限；
第5位到第7位r-x表示的是组（hdfs）的权限；
第8位到第10位r-x表示的是其他人（other）的权限；
另外，
r 表示文件可以被读（read）
w 表示文件可以被写（write）
x 表示文件可以被执行（如果它是程序的话）
\- 表示相应的权限还没有被授予
1.修改文件的权限
查看当前文件文件temp的权限信息：
~~~
[root@localhost /]# ll | grep temp
-rw-rw-r--  1 root root      4405  3月 17 11:50 temp
~~~
修改文件权限的指令：
~~~
[root@localhost /]# chmod o+w temp
~~~
表示给文件temp授予其他人写权限，现在查看temp的权限信息：
~~~
-rw-rw-rw-  1 root root      4405  3月 17 11:50 temp
~~~
我们发现第9位多出了一个w。
其中参数表示的意义为：
　　u 代表所有者（user）
　　g 代表所有者所在的组群（group）
　　o 代表其他人，但不是u和g （other）
　　a 代表全部的人，也就是包括u，g和o
　　r 表示文件可以被读（read）
　　w 表示文件可以被写（write）
　　x 表示文件可以被执行（如果它是程序的话）
　　其中：rwx也可以用数字来代替
　　r :4
　　w :2
　　x :1
　　- :0
行动：
　　+ 表示添加权限
　　- 表示删除权限
　　= 表示使之成为唯一的权限　　当大家都明白了上面的东西之后，那么我们常见的以下的一些权限就很容易都明白了：
　　-rw------- (600) 只有所有者才有读和写的权限
　　-rw-r--r-- (644) 只有所有者才有读和写的权限，组群和其他人只有读的权限
　　-rwx------ (700) 只有所有者才有读，写，执行的权限
　　-rwxr-xr-x (755) 只有所有者才有读，写，执行的权限，组群和其他人只有读和执行的权限
　　-rwx--x--x (711) 只有所有者才有读，写，执行的权限，组群和其他人只有执行的权限
　　-rw-rw-rw- (666) 每个人都有读写的权限
　　-rwxrwxrwx (777) 每个人都有读写和执行的权限
 
2.修改目录的所有者和群组
将目录的所有者修改为root:root（第一个root表示组，第二个root表示用户）。  
该指令需要在root权限下使用。
~~~
[root@localhost /]# chown -R root:root nameservice1
~~~
此时再执行```ll```,显示结果为：
~~~
[root@localhost /]# drwxr-xr-x 3 root root 4096 4月  14 16:19 nameservice1
~~~
将目录换成文件的名字就可以修改文件的所有者了，例如：
~~~
[root@localhost /]# chown -R root:root test.txt
~~~
只改变文件或目录的所有者
~~~
[root@localhost /]# chown -R owner: test.txt
~~~
只改变文件或目录的群组
~~~
[root@localhost /]# chown -R :group test.txt
~~~
##11.安装软件(yum,apt-get)
1.RedHat系列：Redhat、Centos、Fedora等
2.Debian系列：Debian、Ubuntu等
###RedHat 系列 
1 常见的安装包格式 rpm包,安装rpm包的命令是```rpm -参数```
2 包管理工具 yum 
3 支持tar包
###Debian系列 
1 常见的安装包格式 deb包,安装deb包的命令是```dpkg -参数```
2 包管理工具 apt-get 
3 支持tar包
##12.生成sshkey，常用git操作(pull,push,commit,add)
1.检查SSH keys是否存在
输入下面的命令，如果有文件id_rsa.pub 或 id_dsa.pub，则直接进入步骤3将SSH key添加到GitHub中，否则进入第二步生成SSH key
~~~
[root@localhost /]# ls -al ~/.ssh
//Lists the files in your .ssh directory, if they exist
~~~
2.生成新的ssh key
第一步：生成public/private rsa key pair
在命令行中输入
~~~
[root@localhost /]# ssh-keygen -t rsa -C "your_email@example.com"
~~~
默认会在相应路径下（/your_home_path）生成```id_rsa```和```id_rsa.pub```两个文件，如下面代码所示
~~~
[root@localhost /]# ssh-keygen -t rsa -C "your_email@example.com"
//Creates a new ssh key using the provided email
Generating public/private rsa key pair.
Enter file in which to save the key (/your_home_path/.ssh/id_rsa):
~~~
第二步：输入passphrase（本步骤可以跳过）
设置passphrase后，进行版本控制时，每次与GitHub通信都会要求输入passphrase，以避免某些“失误”
~~~
Enter passphrase (empty for no passphrase): [Type a passphrase]
Enter same passphrase again: [Type passphrase again]
sample result:
Your identification has been saved in /your_home_path/.ssh/id_rsa.
Your public key has been saved in /your_home_path/.ssh/id_rsa.pub.
The key fingerprint is:
#01:0f:f4:3b:ca:85:d6:17:a1:7d:f0:68:9d:f0:a2:db your_email@example.com
~~~
第三步：将新生成的key添加到ssh-agent中:
~~~
//start the ssh-agent in the background
[root@localhost /]# eval "$(ssh-agent -s)"
[root@localhost /]# Agent pid 59566
[root@localhost /]# ssh-add ~/.ssh/id_rsa
~~~
3.将ssh key添加到GitHub中
用自己喜欢的文本编辑器打开id_rsa.pub文件，里面的信息即为SSH key，将这些信息复制到GitHub的Add SSH key页面即可
不同的操作系统，均有一些命令，直接将SSH key从文件拷贝到粘贴板中，如下：
mac
~~~
pbcopy < ~/.ssh/id_rsa.pub
//Copies the contents of the id_rsa.pub file to your clipboard
~~~
windows
~~~
clip < ~/.ssh/id_rsa.pub
//Copies the contents of the id_rsa.pub file to your clipboard
~~~
linux
~~~
sudo apt-get install xclip
//Downloads and installs xclip. If you don't have `apt-get`, you might need to use another installer (like `yum`)
xclip -sel clip < ~/.ssh/id_rsa.pub
//Copies the contents of the id_rsa.pub file to your clipboard
~~~