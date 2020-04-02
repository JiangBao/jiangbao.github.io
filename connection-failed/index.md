# Connection failed

今天将新购买的个人服务器数据库升级MySQL8.0，出现了本地Navicat Premium无法连接的问题。

<!--more-->

第一反应: 是否远程连接未打开或者服务器安全组未设置，检查了远程连接设置以及服务器安全组开放端口，都正常。

连接提示错误为：  
```Bash
MySQL said: Authentication plugin 'caching_sha2_password' 
cannot be loaded: dlopen(/usr/local/lib/plugin/caching_sha2_password.so, 2): image not found
```
最终发现是因为MySQL8之后加密规则导致的：  

> MySQL8版本之前之前的加密规则是`mysql_native_password`，在MySQL8之后，加密规则是`caching_sha2_password`

通过还原登录密码加密规则解决此问题：
```Bash
> mysql -u root -p

mysql> ALTER USER 'root'@'%' IDENTIFIED WITH mysql_native_password BY 'password';

mysql> FLUSH PRIVILEGES;
```

