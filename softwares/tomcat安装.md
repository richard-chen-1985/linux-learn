## 安装

```shell
cd /export/servers
wget http://mirrors.hust.edu.cn/apache/tomcat/tomcat-7/v7.0.69/bin/apache-tomcat-7.0.69.tar.gz
tar -zvxf apache-tomcat-7.0.69.tar.gz
mv apache-tomcat-7.0.69 tomcat
```

启动tomcat

`/export/servers/tomcat/bin/startup.sh`

开放8080端口，分别执行以下命令

```shell
ufw enable  
ufw allow 8080  
ufw allow 8080/tcp  
cat /etc/services | less  
ufw allow webcache  
ufw status
```

## 一台机器部署多应用的tomcat配置 

假设我们目录如下规划
```
/export/
  ├─ app/ #应用程序目录
  |  ├─ test1/
  |  |  └─ webapps/
  ├─ domains/ #应用配置
  |  ├─ test1
  |  |  ├─ tomcat.sh #tomcat脚本
  |  |  ├─ conf/ #servers.xml等配置
  |  |  ├─ logs/
  |  |  └─ work/
  └─ servers/ #服务器软件安装目录
     └─ tomcat #tomcat安装目录
```

`mkdir -p /export/app/test1`# 放置应用目录统一用app，test1为第一个测试应用目录

`mkdir -p /export/domains/test1` # 放置tomcat配置目录统一在domains目录，test1为第一个测试应用的tomcat配置目录

将应用程序代码上传到 `/export/app/test1/`

拷贝tomcat的配置目录

```shell
cd /export/servers/tomcat
cp conf /export/domains/test1/
cd /export/domains/test1/
mkdir logs
mkdir work
```

设置本应用的root目录

`vi -p conf/Catalina/localhost/ROOT.xml`

```xml
<?xml version="1.0" encoding="UTF-8"?>
<Context path="/" docBase="/export/app/test1"></Context>
```

将tomcat的配置目录`/export/servers/tomcat/conf` 拷贝到 `/export/domains/test1/` 目录

在`/export/domains/test1/` 新建tomcat的shell文件`tomcat.sh`

```shell
#!/bin/bash
# tomcat安装目录
export CATALINA_HOME=/export/servers/tomcat
# tomcat配置目录
export CATALINA_BASE=/export/domains/test1
export CATALINA_PID=$CATALINA_BASE/work/catalina.pid
export LANG=zh_CN.UTF-8
# JAVA
export JAVA_HOME=/usr/local/java/jdk1.7.0_79
export JAVA_BIN=/usr/local/java/jdk1.7.0_79/bin
export PATH=$JAVA_BIN:/usr/sbin:/usr/bin:/usr/local/sbin:/usr/local/bin:/sbin:/bin:/root/bin
export CLASSPATH=.:/lib/dt.jar:/lib/tools.jar
export JAVA_OPTS="-Djava.library.path=/usr/local/lib -server -Xms3072m -Xmx3072m -XX:MaxPermSize=1024m -Djava.awt.headless=true -Dsun.net.client.defaultConnectTimeout=60000 -Dsun.net.client.defaultReadTimeout=60000 -Djmagick.systemclassloader=no -Dnetworkaddress.cache.ttl=300 -Dsun.net.inetaddr.ttl=300 -XX:+HeapDumpOnOutOfMemoryError -XX:HeapDumpPath=$CATALINA_BASE/logs -XX:ErrorFile=$CATALINA_BASE/logs/java_error_%p.log"
export JAVA_HOME JAVA_BIN PATH CLASSPATH JAVA_OPTS

start() {
    $CATALINA_HOME/bin/startup.sh -config $CATALINA_BASE/conf/server.xml
    echo "test1 started..."
}
stop() {
    $CATALINA_HOME/bin/shutdown.sh -config $CATALINA_BASE/conf/server.xml
    echo "test1 stopped..."
    sleep 3
    ps -aef | grep java|grep "/test1"| grep -v grep | sed 's/ [ ]*/:/g' |cut -d: -f2|kill -9 `cat`
}

CMD=$1

if [ $CMD == "stop" ]; then
    stop;
elif [ $CMD == "restart" ]; then
    stop;
    start;
else
    start;
fi
```

启动该应用只需要`/export/domains/test1/tomcat.sh start`

将启动命令加到`/etc/rc.local`中即可开机启动