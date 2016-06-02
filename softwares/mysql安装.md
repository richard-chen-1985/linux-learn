## 安装包下载

访问[http://downloads.mysql.com/archives/community/](http://downloads.mysql.com/archives/community/)

* `Select Platform` 中选择 `Linux - Generic`

* 下载 `Linux - Generic (glibc 2.5) (x86, 64-bit), Compressed TAR Archive`

* 依赖包：`apt-get install libaio`

## 安装命令如下:

```
groupadd mysql
# /bin/fase表示用户没有登录 os 的权限
useradd -r -g mysql -s /bin/false mysql
cd /usr/local
# upload mysql package
tar -zxvf /path/to/mysql-VERSION-OS.tar.gz
ln -s full-path-to-mysql-VERSION-OS mysql
cd mysql
chown -R mysql .
chgrp -R mysql .
scripts/mysql_install_db --user=mysql
chown -R root .
chown -R mysql data
# 开启 mysql 服务
bin/mysqld_safe --user=mysql &
# Next command is optional
cp support-files/mysql.server /etc/init.d/mysql.server
```

## 设置密码

```
mysql> SELECT User, Host, Password FROM mysql.user;
+------+--------------------+----------+
| User | Host               | Password |
+------+--------------------+----------+
| root | localhost          |          |
| root | myhost.example.com |          |
| root | 127.0.0.1          |          |
| root | ::1                |          |
|      | localhost          |          |
|      | myhost.example.com |          |
+------+--------------------+----------+
```

```
mysql -u root
```

```
SET PASSWORD FOR 'root'@'localhost' = PASSWORD('123456');
SET PASSWORD FOR 'root'@'127.0.0.1' = PASSWORD('123456');
SET PASSWORD FOR 'root'@'::1' = PASSWORD('123456');
```

也可以一条命令设置 `root`

```
mysql -u root
```

```
UPDATE mysql.user SET Password = PASSWORD('new_password') WHERE User = 'root';
FLUSH PRIVILEGES;
```

## 开机自启动

在`/etc/rc.local`文件的最后中添加如下的一行: `/usr/local/mysql/bin/mysqld_safe --user=mysql &`