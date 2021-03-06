## 安装包下载

访问[http://downloads.mysql.com/archives/community/](http://downloads.mysql.com/archives/community/)

* `Select Platform` 中选择 `Linux - Generic`

* 下载 `Linux - Generic (glibc 2.5) (x86, 64-bit), Compressed TAR Archive`

* 依赖包：`apt-get install libaio`

## 安装命令如下:

```bash
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
# basedir 为安装目录
# datadir 为数据目录
scripts/mysql_install_db --user=mysql \
  --basedir=/opt/mysql/mysql \
  --datadir=/opt/mysql/mysql/data
chown -R root .
chown -R mysql data
# 开启 mysql 服务
bin/mysqld_safe --user=mysql &
# Next command is optional
cp support-files/mysql.server /etc/init.d/mysql.server
```

## 设置密码

```bash
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

```bash
mysql -u root
```

```bash
SET PASSWORD FOR 'root'@'localhost' = PASSWORD('123456');
SET PASSWORD FOR 'root'@'127.0.0.1' = PASSWORD('123456');
SET PASSWORD FOR 'root'@'::1' = PASSWORD('123456');
```

也可以一条命令设置 `root`

```bash
mysql -u root
```

```bash
UPDATE mysql.user SET Password = PASSWORD('new_password') WHERE User = 'root';
FLUSH PRIVILEGES;
```

## 支持localhost的socket链接

```bash
# 修改MySQL的配置文件my.cnf，指定mysql.socket的位置：
/var/lib/mysql/mysql.sock #(你的mysql.socket路径)。
```

## 开机自启动

在`/etc/rc.local`文件的最后中添加如下的一行: `/usr/local/mysql/bin/mysqld_safe --user=mysql &`
