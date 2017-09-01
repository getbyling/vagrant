Vagrant Tomcat 安装整理
=======================

# Tomcat 准备
>这里提供的 Tomcat 下载版本是 8。[Tomcat 8 下载](http://mirrors.tuna.tsinghua.edu.cn/apache/tomcat/tomcat-8/v8.5.20/bin/apache-tomcat-8.5.20.tar.gz) 

# 安装步骤
>以下的安装步骤是以 ```Ubuntu 16.04``` 为示例。

## 步骤 1：安装 JDK
>在 Ubuntu 下 JDK 的安装，可阅读[《Vagrant JDK 安装整理》](http://note.youdao.com/share/?id=bdcef13988cb1c4f9e7d11c0aba26ce1&type=note#/)这篇文章。在这里就不在赘述（-_-）

## 步骤 2：创建 Tomcat 用户
>为了安全起见，创建 tomcat 用户或组。可以在这用户或组内执行 tomcat 服务。在 vagrant 个人建议可省略（-_-）。

**1. 创建用户组**
```
   $ sudo groupadd tomcat
```
**2. 创建一个新的 tomcat**
>用户。用户权限```(/opt/tomcat)```目录，开通 shell 权限```(/bin/false)```。
```
   $ sudo useradd -s /bin/false -g tomcat -d /opt/tomcat tomcat
```
>创建完```tomcat``` 用户后，现在可以用刚创建的用户安装下载的 Tomcat 了。

## 步骤 3：安装 Tomcat
**1. 切换目录**
```
   $ cd /tmp
```
**2. 用 curl 下载**
```
   $ curl -O http://apache.mirrors.ionfish.org/tomcat/tomcat-8/v8.5.5/bin/apache-tomcat-8.5.5.tar.gz
```
**3. 用 mkdir 创建（/opt/tomcat）目录**
```
   $ sudo mkdir /opt/tomcat
```
**4. 用 tar 解压文件到（/opt/tomcat）下**
```
   $ sudo tar xzvf apache-tomcat-8*tar.gz -C /opt/tomcat --strip-components=1
```

## 步骤 4：更新权限
>```tomcat```用户必须要有访问 Tomcat 安装目录的权限。

**1. 切换 tomcat 安装包目录**
```
$ cd /opt/tomcat
```
**2. 设置 tomcat 组分配安装包目录权限**
```
$ sudo chgrp -R tomcat /opt/tomcat
```
**3. 设置 tomcat 组读与访问```conf```目录权限**
```
$ sudo chmod -R g+r conf
$ sudo chmod g+x conf
```
**4. 设置 tomcat 用户设置```webapps/```、```work/```、```temp/```、```logs/```目录权限**
```
$ sudo chown -R tomcat webapps/ work/ temp/ logs/
```
>设置完以上的权限后，就能进行```Tomcat```系统服务的创建了。

## 步骤 5：创建系统服务文件
>为了运行 Tomcat 服务，先要安装系统服务文件。

>Tomcat 必须先配置```JAVA_HOME```。查看本地配置位置，执行以下命令：
```
$ sudo update-java-alternatives -l
```
>输出
```
java-1.8.0-openjdk-amd64       1081       /usr/lib/jvm/java-1.8.0-openjdk-amd64
```
>通过获取的```JAVA_HOME```环境变量，后面追加```/jre```。更改组合成的这个服务如下：
```
JAVA_HOME:
/usr/lib/jvm/java-1.8.0-openjdk-amd64/jre
```
>具体情况以自己本机环境为准。

>通过以下命令，可以创建系统服务文件。打开访问```tomcat.service```在```/etc/systemd/system```目录中：
```
$ sudo nano /etc/systemd/system/tomcat.service
```


**参考资料**
>https://www.digitalocean.com/community/tutorials/how-to-install-apache-tomcat-8-on-ubuntu-16-04