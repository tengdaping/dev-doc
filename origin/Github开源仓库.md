#Github开源仓库
~pasta_ping~
___
sshkey等操作略过。

以罗老师的dev-doc项目为例。

`https://github.com/shuidong/dev-doc`


1.在Github的该项目目录下，点击fork按钮。
进入自己的profile，可以看到repositories下面增加了刚才fork的项目。点击进入项目链接。点击Clone or download按钮，选择clone as ssh，复制链接。

2.clone该项目至本地

`git clone git@github.com:tengdaping/dev-doc.git`

3.cd进入dev-doc，确定一下是否建立了主repo的远程源

`git remote -v`

4.如果里面只能看到你自己的两个源(fetch和push)，那就需要添加主repo的源：
`git remote add upstream git@github.com:shuidong/dev-doc.git`
`git remote -v`
然后你就能看到upstream了。
5.如果想与主repo合并：
`git fetch upstream`
`git merge upstream/master`

6.pull request说明
当本地完成工作后
`git add xxxx`
`git commit -m "xxxx"`
`git push`
成功后，在Github网站自己的repositores下发起New pull reqeust，
点击进入后，添加注释，提交pull reqeust，等待管理员审核批准。



