## centos安装

安装选择minimal最小安装

## 设置网络

设置网站为DHCP自动获取IP

`vi /etc/sysconfig/network-scripts/ifcfg-eth0`

```shell
NM_CONTROLLED=no
ONBOOT=yes
BOOTPROTO=dhcp
DEVICE=（不变）
HWADDR=（不变）
#其余注释掉
```

`/etc/init.d/network restart #重启网络`

## 常用软件

git：`yum install git`

rz：`yum install lrzsz`

nvm：

```shell
wget https://github.com/creationix/nvm/archive/v0.31.1.tar.gz
tar -zvxf v0.31.1.tar.gz
cd nvm-0.31.1
./install.sh
source ~/.bash_profile
nodejs by nvm：
nvm install v4.4.5
```

nodejs：

```shell
yum -y install gcc make gcc-c++ openssl-devel wget
wget http://nodejs.org/dist/v4.4.5/node-v4.4.5.tar.gz
tar -zvxf node-v4.4.5.tar.gz
make && make install
node -v
```

nginx：

* gzip模块依赖zlib库

```shell
yum install zlib
```

* rewrite模块依赖pcre库

```shell
wget https://sourceforge.net/projects/pcre/files/pcre/8.38/pcre-8.38.tar.gz
tar -zvxf pcre-8.38.tar.gz
cd pcre-8.38
./configure
make && make install
```

* ssl模块依赖openssl库
    
```shell
yum install openssl
wget http://nginx.org/download/nginx-1.8.1.tar.gz
tar -zvxf nginx-1.8.1.tar.gz
cd nginx-1.8.1
./configuration
make && make install
```

java：见 ../软件安装/linux安装jdk
tomcat：见 ../软件安装/linux安装tomcat

zip/unzip：`yum install zip unzip`

## nginx一些经验：

启动nginx提示：error while loading shared libraries: libpcre.so.1: cannot open shared object file: No such file or directory，意思是找不到libpcre.so.1这个模块，而导致启动失败。

```shell
# 如果是32位系统 
ln -s /usr/local/lib/libpcre.so.1 /lib
# 如果是64位系统 
ln -s /usr/local/lib/libpcre.so.1 /lib64
# 然后在启动nginx就OK了 
/usr/local/webserver/nginx/sbin/nginx
```

centos的防火墙开放80端口

```shell
/sbin/iptables -I INPUT -p tcp --dport 80 -j ACCEPT
/etc/rc.d/init.d/iptables save #保存
/etc/init.d/iptables status #查看状态
```

nginx自启动：[nginx自启动](./nginx自启动.md)