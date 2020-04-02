# 又踩一个postgreSQL的坑

今日在家里的笔记本下安装`postgreSQL`跑一个测试项目，`brew`一键安装，安装之后`psql --version`，一看轻松搞定，可是试图连接时它却这样告诉我

```
psql: could not connect to server: No such file or directory
	Is the server running locally and accepting
	connections on Unix domain socket "/tmp/.s.PGSQL.5432"?
```
<div align=center><img width="200" height="200" src="https://jiangbao-1258001083.cos.ap-shanghai.myqcloud.com/20200101203725.jpeg"/></div>
什么？正常步骤操作怎么能报错呢？看了一下官方文档提到了这个问题：
  
> This means that the server was not started, or it was not started where createdb expected it. Again, check the installation instructions or consult the administrator.

未正常启动？怎么会犯这么低级的错误，检查发现已经正常启动，显然对我没啥帮助，stackoverflow上输入关键词`mac OSX、/tmp/.s.PGSQL.5432`找一找，发现遇到同样问题的还不少，大多数提到的一个方法就是删除安装地址下的`postmaster.pid`文件：
```
rm -f /usr/local/var/postgres/postmaster.pid
```
可是我在本地路径下居然没找到这个文件，算了，再看看其它解决方法：重新启动进程、卸载干净再重装安装程序、添加PATH...一一尝试后都以失败告终，甚至一度为这答案陷入小事重启大事重装的死循环:(。
就在准备放弃的时候，尝试了[这个回答](https://stackoverflow.com/a/19484986/9629136)的解决方案，终于成功连接，愉快接入本地postgreSQL进行测试~
<div align=center><img width="711" height="445" src="https://jiangbao-1258001083.cos.ap-shanghai.myqcloud.com/20200101203804.png"/></div>

