---
title: 如何部署全新的CentOS7
date: 2016-12-30 17:41:44
tags:
---

# CentOS 7 流程
## java环境
* 查看CentOS自带JDK是否已安装
```
yum list installed |grep java 
```
* 卸载CentOS系统自带Java环境  
```
yum -y remove java-1.7.0-openjdk\*
```
* 查看yum库中的Java安装包            
```
yum -y list java\* 
```
* 使用yum安装Java环境                    
```
yum -y install java-1.8.0-openjdk           //以1.8.0为例
```
- - - -
* 下载``jdk``

[jdk下载](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)
* 使用``scp``命令至环境
```
scp -r jdk-8u111-linux-x64.tar.gz root@www.yuezy.site:/
```
* 解压环境包
```
tar -zxvf  jdk
```
* 编辑文件
```
vim ~/.bash_profile

export JAVA_HOME=/usr/local/jdk1.7.0_71
export CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
export PATH=$JAVA_HOME/bin:$PATH

注意标点符号，JAVA_HOME是刚才mv到路径
```

* 执行文件
```
source ~/.bash_profile
```
* 查看是否成功
```
java -version
javac
```

## git
* 安装``git``
```
yum install git
```

## tomcat  
* 下载tomcat

[tomcat 7.0.x 下载](http://tomcat.apache.org/download-70.cgi)

* 解压完成后
```
/home/apache-tomcat-7.0.73/bin/startup.sh
```
## nginx

* ``gcc``安装
安装``nginx``需要先将官网下载的源码进行编译，编译依赖``gcc``环境，如果没有``gcc``环境，则需要安装：
```
yum install gcc-c++
```
* ``PCRE pcre-devel`` 安装
``PCRE(Perl Compatible Regular Expressions)`` 是一个``Perl``库，包括 ``perl`` 兼容的正则表达式库。``nginx`` 的 ``http`` 模块使用 ``pcre`` 来解析正则表达式，所以需要在 ``linux`` 上安装 ``pcre`` 库，``pcre-devel`` 是使用 ``pcre`` 开发的一个二次开发库。``nginx``也需要此库。命令：
```
yum install -y pcre pcre-devel
```
* ``zlib`` 安装
``zlib`` 库提供了很多种压缩和解压缩的方式， ``nginx`` 使用 ``zlib`` 对 ``http`` 包的内容进行 ``gzip`` ，所以需要在 ``Centos`` 上安装 ``zlib`` 库。
```
yum install -y zlib zlib-devel
```
* ``OpenSSL`` 安装
OpenSSL 是一个强大的安全套接字层密码库，囊括主要的密码算法、常用的密钥和证书封装管理功能及 SSL 协议，并提供丰富的应用程序供测试或其它目的使用。
nginx 不仅支持 http 协议，还支持 https（即在ssl协议上传输http），所以需要在 Centos 安装 OpenSSL 库。
```
yum install -y openssl openssl-devel
```

* 下载``nginx``

[nginx下载](https://nginx.org/en/download.html)
* 解压``nginx``
```
tar -zxvf nginx-xx
```
* 配置   
到``nginx``目录下:
```
./configure   //默认配置
```
* 编译安装
```
make
make install
whereis nginx   // 查找安装路径
```
*  启动``nginx``
```
cd /usr/local/nginx/sbin/
./nginx 
./nginx -s reload
./nginx -s quit  此方式停止步骤是待nginx进程处理任务完毕进行停止。
./nginx -s stop  此方式相当于先查出nginx进程id再使用kill命令强制杀掉进程。

查询nginx进程：
ps aux|grep nginx
```
* 重启``nginx``
1. 先停止再启动（推荐）：
对 ``nginx`` 进行重启相当于先停止再启动，即先执行停止命令再执行启动命令。如下：
```
./nginx -s quit
./nginx
```
2. 重新加载配置文件：
当 ``nginx``的配置文件``nginx.conf`` 修改后，要想让配置生效需要重启 ``nginx``，
使用``-s reload``不用先停止 ``nginx``再启动 ``nginx`` 即可将配置信息在 ``nginx`` 中生效，如下：
```
./nginx -s reload
```


## Node 
使用国内镜像

```
npm config set registry https://registry.npm.taobao.org
npm info underscore （如果上面配置正确这个命令会有字符串response）
```