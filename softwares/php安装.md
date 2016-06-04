## 下载

http://ee1.php.net/get/php-5.5.36.tar.gz/from/this/mirror

## 安装

建议您访问 Nginx Wiki » 安装 页面以获取并在您的系统上安装 Nginx。

### 获取并解压 PHP 源代码:

`tar zxf php-x.x.x`

配置并构建 PHP。在此步骤您可以使用很多选项自定义 PHP，例如启用某些扩展等。 运行 `./configure --help` 命令来获得完整的可用选项清单。 在本示例中，我们仅进行包含 PHP-FPM 和 MySQL 支持的简单配置。

可能提示的依赖：
```
yum install libxml2
yum install libxml2-devel -y
yum install bzip2
yum install bzip2-devel
yum install libcurl
yum install libcurl-devel
#mb_string
cd /export/servers/php-5.5.36/ext/mbstring
/usr/local/bin/phpize
./configure --with-php-config=/usr/local/bin/php-config
make && make install
#mysqli
cd /export/servers/php-5.5.36/ext/mysqli
/usr/local/bin/phpize
./configure --with-php-config=/usr/local/bin/php-config
make && make install
```

创建配置文件，并将其复制到正确的位置。

```
cp php.ini-development /usr/local/lib/php.ini
cp /usr/local/etc/php-fpm.conf.default /usr/local/etc/php-fpm.conf
cp sapi/fpm/php-fpm /usr/local/bin
```

需要着重提醒的是，如果文件不存在，则阻止 Nginx 将请求发送到后端的 PHP-FPM 模块， 以避免遭受恶意脚本注入的攻击。

将 `php.ini` 文件中的配置项 `cgi.fix_pathinfo` 设置为 `0` 。

打开 `php.ini:`

`vim /usr/local/lib/php.ini`

定位到 `cgi.fix_pathinfo=` 并将其修改为如下所示：

`cgi.fix_pathinfo=0`

在启动服务之前，需要修改 `php-fpm.conf` 配置文件，确保 `php-fpm` 模块使用 `www-data` 用户和 `www-data` 用户组的身份运行。

`vim /usr/local/etc/php-fpm.conf`

找到以下内容并修改：

```
; Unix user/group of processes
; Note: The user is mandatory. If the group is not set, the default user's group
;       will be used.
user = www-data
group = www-data
```

然后启动 `php-fpm` 服务：

`/usr/local/bin/php-fpm`

本文档未涵盖对 `php-fpm` 进行进一步配置的信息，如果您需要更多信息，请查阅相关文档。

### 重启PHP

```
killall php-fpm
/usr/loca/bin/php-fpm &
```

### 配置 Nginx 使其支持 PHP 应用：

`vim /usr/local/nginx/conf/nginx.conf`

修改默认的 `location` 块，使其支持 `.php` 文件：

```
location / {
    root   html;
    index  index.php index.html index.htm;
}
```

下一步配置来保证对于 .php 文件的请求将被传送到后端的 PHP-FPM 模块， 取消默认的 PHP 配置块的注释，并修改为下面的内容：

```
location ~* \.php$ {
    fastcgi_index   index.php;
    fastcgi_pass    127.0.0.1:9000;
    include         fastcgi_params;
    fastcgi_param   SCRIPT_FILENAME    $document_root$fastcgi_script_name;
    fastcgi_param   SCRIPT_NAME        $fastcgi_script_name;
}
```

重启 Nginx。

```
sudo /usr/local/nginx/sbin/nginx -s stop
sudo /usr/local/nginx/sbin/nginx
```

创建测试文件。

```
rm /usr/local/nginx/html/index.html
echo "<?php phpinfo(); ?>" >> /usr/local/nginx/html/index.php
```

打开浏览器，访问 `http://localhost`，将会显示 `phpinfo()` 。

通过以上步骤的配置，Nginx 服务器现在可以以 SAPI 模块的方式支持 PHP 应用了。 当然，对于 Nginx 和 PHP 的配置，还有很多可用的选项， 请在对应的源代码目录执行 `./configure --help` 来查阅更多配置选项。

## php自启动

在 `/etc/rc.local`中加入 `/usr/local/bin/php-fpm &`