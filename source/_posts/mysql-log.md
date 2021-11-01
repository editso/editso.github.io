---
title: mysql8.0踩坑
date: 2019-12-19 10:53:06
tags:
    - Mysql
    - 日志
    - 踩坑记
    - 数据库
---
# Mysql8.0采坑

## 密码无法使用
在服务器中安装`mysql8.0`数据库修改密码发现无法使用`update`语句修改,于是百度了一番参考了[MYSQL8.0以上版本正确修改ROOT密码](https://blog.csdn.net/yi247630676/article/details/80352655)由此问题解决  

### 解决方法:  
1. 使用`mysql`初始生成的密码登录到数据库(密码在数据库产生的日志文件中, 可查看/`etc/my.cnf`中的`log-error`指向的文件)  
2. 登录到数据库 `mysql -uroot -p'首次初始化数据库产生的密码'`  
3. 登录执行 `alter user 'root'@localhost identified by 'new password';` 
4. 执行成功后刷新数据库 `flush privileges;`问题成功解决  

## 连接出现 `cryptography is required for sha256_password or caching_sha2_password`**  

### 解决方法:
1. 首先查询一下数据库`user`表中`连接时报错的用户与主机`  

        mysql> select user,plugin,host  from mysql.user; 
        +------------------+-----------------------+-----------+  
        | user             | plugin                | host      |  
        +------------------+-----------------------+-----------+  
        | root             | mysql_native_password | %         |  
        | mysql.infoschema | caching_sha2_password | localhost |  
        | mysql.session    | caching_sha2_password | localhost |  
        | mysql.sys        | caching_sha2_password | localhost |  
        | root             | mysql_native_password | localhost |  
        +------------------+-----------------------+-----------+  
        5 rows in set (0.00 sec)  
3. `%` 表示通配符
2. 查询得出 `plugin `为 `caching_sha2_password` 就将它改为 `mysql_native_password`执行如下命令:  
     `alter user 'your account'@'your host' identified with mysql_native_password by 'your password'`
3. `flush privileges` #刷新权限
4. 重新连接， 问题解决 。参考了`msql8.0`官方文档[可插拔身份验证](https://dev.mysql.com/doc/refman/8.0/en/pluggable-authentication.html)

### 不成功其他解决方法 
1. 参考 [mysql报错RuntimeError: cryptography is required for sha256_password or caching_sha2_p](https://blog.csdn.net/p_xiaobai/article/details/85334875)
2. `ALTER USER 'root'@'localhost' IDENTIFIED BY 'password' PASSWORD EXPIRE NEVER;` #修改加密规则  
3. ` ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'password';` #更新一下用户的密码 
4. `FLUSH PRIVILEGES;` #刷新权限
5. `alter user 'root'@'localhost' identified by '123456';`  # 再次重置密码
6. 重启服务,问题解决 