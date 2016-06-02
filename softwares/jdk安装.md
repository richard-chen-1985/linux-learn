### 1、源码包准备：

首先到官网[下载jdk](http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html)，下载jdk-7u45-linux-x64.tar.gz，下载到主目录
 
### 2、解压源码包

通过终端在`/usr/local`目录下新建`java`文件夹，命令行：

`sudo mkdir /usr/local/java`

然后将下载到压缩包拷贝到java文件夹中，命令行：

`cp jdk-7u45-linux-x64.tar.gz /usr/local/java`

然后进入java目录，命令行：

`cd /usr/local/java`

解压压缩包，命令行：

`sudo tar -xvf jdk-7u45-linux-x64.tar.gz`

然后可以把压缩包删除，命令行：

`sudo rm jdk-7u45-linux-x64.tar.gz`
 
### 3、设置jdk环境变量

这里采用全局设置方法，就是修改etc/profile，它是是所有用户的共用的环境变量

`sudo gedit /etc/profile`

打开之后在末尾添加

```shell
export JAVA_HOME=/usr/local/java/jdk1.7.0_45
export JRE_HOME=/usr/local/java/jdk1.7.0_45/jre
export CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar:$JRE_HOME/lib:$CLASSPATH
export PATH=$JAVA_HOME/bin:$PATH
```
 
请记住，在上述添加过程中，等号两侧不要加入空格，不然会出现“不是有效的标识符”，因为source /etc/profile 时不能识别多余到空格，会理解为是路径一部分。

然后保存

`source /etc/profile` 使profile生效
 
### 4、检验是否安装成功

在终端

`java -version`

看看是否安装成功

成功则显示如下

java version "1.7.0_45"

Java(TM) SE Runtime Environment (build 1.7.0_45-b18)

Java HotSpot(TM) 64-Bit Server VM (build 24.45-b08, mixed mode)