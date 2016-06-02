Ubuntu/Linux系统开机自启动，很简单，在`/etc/init.d/`目录下编写一个服务脚本(比如nginx)，然后在`/etc/init.d/rc.local`中添加一行启动命令。操作步骤如下

`vi /etc/init.d/nginx` ,其中具体的脚本展示如下：

```shell
#!/bin/bash
# nginx Startup script for the Nginx HTTP Server
# it is v.0.0.2 version.
# chkconfig: - 85 15
# description: Nginx is a high-performance web and proxy server.
#              It has a lot of features, but it's not for everyone.
# processname: nginx
# pidfile: /var/run/nginx.pid
# config: /usr/local/nginx/conf/nginx.conf

#注意：这里的三个变量需要根据具体的环境而做修改。
nginxd=/opt/nginx/sbin/nginx
nginx_config=/opt/nginx/conf/nginx.conf
nginx_pid=/opt/nginx/logs/nginx.pid
RETVAL=0
prog="nginx"

# Check that networking is up.
[ -x $nginxd ] || exit 0
# Start nginx daemons functions.
start() {
if [ -e $nginx_pid ];then
    echo "nginx already running...."
    exit 1
fi
    echo -n $"Starting $prog.... "
    $nginxd -c ${nginx_config}
    RETVAL=$?
    echo
    [ $RETVAL = 0 ]  
    return $RETVAL
}
# Stop nginx daemons functions.
stop() {
    echo -n $"Stopping $prog:...."
    $nginxd -s stop
    RETVAL=$?
    echo
    [ $RETVAL = 0 ] && rm -f /var/lock/subsys/nginx $nginx_pid
}
# reload nginx service functions.
reload() {
    echo -n $"Reloading $prog.... "
    kill -HUP `cat ${nginx_pid}`
    RETVAL=$?
    echo
}
# See how we were called.
case "$1" in
    start)
        start
        ;;
    stop)
        stop
        ;;
    reload)
        reload
        ;;
    restart)
        stop
        start
        ;;
    status)
        status $prog
        RETVAL=$?
        ;;
*)
echo $"Usage: $prog {start|stop|restart|reload|status|help}"
exit 1
esac
exit $RETVAL
```

`sudo chmod a+x /etc/init.d/nginx `


在`/etc/rc.local`文件的最后中添加如下的一行: `/etc/init.d/nginx start`

最后，nginx可以通过 `/etc/init.d/nginx start`或者 `service nginx start`进行启动。


后记

之所以记下来，是因为在网上的看到的脚本都少有点问题（正对centos，而不是ubuntu），自己看了看，也修改了一下。其实，挺无聊的。

设置完服务器就能访问网站了，对Nginx的了解也仅限于HTTP代理服务器，还需要进一步的学习和认识。