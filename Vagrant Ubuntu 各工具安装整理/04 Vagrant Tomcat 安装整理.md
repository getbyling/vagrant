Vagrant Tomcat 安装整理
=======================

Tomcat 准备
-----------
>这里提供的 Tomcat 下载版本是 8。[Tomcat 8 下载](http://mirrors.tuna.tsinghua.edu.cn/apache/tomcat/tomcat-8/v8.5.20/bin/apache-tomcat-8.5.20.tar.gz) 

安装步骤
--------
>以下的安装步骤是以 ```Ubuntu 16.04``` 为示例。

步骤 1：安装 JDK
-----------------
>在 Ubuntu 下 JDK 的安装，可阅读[《Vagrant JDK 安装整理》](http://note.youdao.com/share/?id=bdcef13988cb1c4f9e7d11c0aba26ce1&type=note#/)这篇文章。在这里就不在赘述（-_-）

步骤 2：创建 Tomcat 用户
------------------------
>为了安全起见，创建 tomcat 用户或组。可以在这用户或组内执行 tomcat 服务。在 vagrant 个人建议可省略（-_-）。

**1. 创建用户组**
```
   sudo groupadd tomcat
```

**2. 创建一个新的 tomcat**
>用户。用户权限```(/vagrant/environment/apache-tomcat-8080)```目录，开通 shell 权限```(/bin/false)```。
```
   sudo useradd -s /bin/false -g tomcat -d /vagrant/environment/apache-tomcat-8080 tomcat
```
>创建完```tomcat``` 用户后，现在可以用刚创建的用户安装下载的 Tomcat 了。

步骤 3：安装 Tomcat
-------------------
**1. 切换目录**
```
   cd /vagrant/environment
```

**2. 用 curl 下载**
```
   curl -O http://apache.mirrors.ionfish.org/tomcat/tomcat-8/v8.5.5/bin/apache-tomcat-8.5.5.tar.gz
```

**3. 用 mkdir 创建（/vagrant/environment/apache-tomcat-8080）目录**
```
   sudo mkdir /vagrant/environment/apache-tomcat-8080
```

**4. 用 tar 解压文件到（/vagrant/environment/apache-tomcat-8080）下**
```
   sudo tar xzvf apache-tomcat-8*tar.gz -C /vagrant/environment/apache-tomcat-8080 --strip-components=1
```

步骤 4：更新权限
----------------
>```tomcat```用户必须要有访问 Tomcat 安装目录的权限。

**1. 切换 tomcat 安装包目录**
```
cd /vagrant/environment/apache-tomcat-8080
```

**2. 设置 tomcat 组分配安装包目录权限**
```
sudo chgrp -R tomcat /vagrant/environment/apache-tomcat-8080
```

**3. 设置 tomcat 组读与访问```conf```目录权限**
```
sudo chmod -R g+r conf
sudo chmod g+x conf
```

**4. 设置 tomcat 用户设置```webapps/```、```work/```、```temp/```、```logs/```目录权限**
```
sudo chown -R tomcat webapps/ work/ temp/ logs/
```

>设置完以上的权限后，就能进行```Tomcat```系统服务的创建了。

步骤 5：创建系统服务文件
------------------------
>为了运行 Tomcat 服务，先要安装系统服务文件。

>Tomcat 必须先配置```JAVA_HOME```。查看本地配置位置，执行以下命令：
```
sudo update-java-alternatives -l
```

>输出
```
java-1.8.0-openjdk-amd64       1081       /usr/lib/jvm/java-1.8.0-openjdk-amd64
```

>通过获取的```JAVA_HOME```环境变量，后面追加```/jre```。更改组合成的这个服务如下：
```text
JAVA_HOME:
/vagrant/environment/apache-tomcat-8080/jre
```
>具体情况以自己本机环境为准。

>通过以下命令，可以创建系统服务文件。打开访问```tomcat.service```在```/etc/systemd/system```目录中：
```
sudo vi /etc/systemd/system/tomcat8080.service
```
>将以下的内容粘贴到服务文件中。需要修改的配置```JAVA_HOME```,有可能还要修改下```CATALINA_OPTS```内存配置：

**/etc/systemd/system/tomcat8080.service**
```text
[Unit]
Description=Apache Tomcat Web Application Container
After=network.target

[Service]
Type=forking

Environment=JAVA_HOME=/vagrant/environment/jdk1.8.0_144/jre
Environment=CATALINA_PID=/vagrant/environment/apache-tomcat-8080/temp/tomcat.pid
Environment=CATALINA_HOME=/vagrant/environment/apache-tomcat-8080
Environment=CATALINA_BASE=/vagrant/environment/apache-tomcat-8080
Environment='CATALINA_OPTS=-Xms512M -Xmx1024M -server -XX:+UseParallelGC'
Environment='JAVA_OPTS=-Djava.awt.headless=true -Djava.security.egd=file:/dev/./urandom'

ExecStart=/vagrant/environment/apache-tomcat-8080/bin/startup.sh
ExecStop=/vagrant/environment/apache-tomcat-8080/bin/shutdown.sh

User=tomcat
Group=tomcat
UMask=0007
RestartSec=10
Restart=always

[Install]
WantedBy=multi-user.target
```
>配置说明：
```text
1. CATALINA_PID：进程锁；
2. CATALINA_OPTS：TOMCAT 使用参数配置。
```
>配置完成后，保存并退出。

>接下来，重新执行守护程序，这样系统就会知道新建的服务文件：
```
sudo systemctl daemon-reload
```
>启动 Tomcat 服务：
```
sudo systemctl start tomcat8080
```

>再次启动 Tomcat 服务，将会报错：
```
sudo systemctl start tomcat8080
```

>停止当前的运行服务，可以使用```stop```命令替代：
```text
sudo systemctl stop tomcat8080.service
```

步骤 6：调整防火墙并测试 Tomcat 服务
------------------------------------
>现在 Tomcat 服务已经启动，我们来验证下是否能访问到默认页。
 开始前，我们必须调整防火墙对服务的请求。前题条件，当前的防火墙```ufw```是开启状态。
 Tomcat 是通过 8080 端口接收常规请求。设置允许访问请求的指令：
```
sudo ufw allow 8080
```

>修改完防火墙后，你可以在浏览器上通过域名或 IP 地址加端口```8080```访问默认页面。
```
#打开浏览器
http://server_domain_or_IP:8080
```

>如果你成功的访问 Tomcat，现在最好同时开启自动启动 Tomcat 服务：
```
sudo systemctl enable tomcat8080
```

步骤 7：配置 Tomcat Web 管理接口
--------------------------------
>在使用 Tomcat web 管理应用清单时，我们必须添加一个我们的 Tomcat 服务帐号。我们要编辑```tomcat-users.xml```文件：
```
sudo nano /vagrant/environment/apache-tomcat-8080/conf/tomcat-users.xml
```

>你想用户谁能访问```manager-gui```和```admin-gui```。所以你能定义用户，类似如下示例，在```tomcat-users```标签间。确保修改配置的用户和密码是安全的：
**tomcat-users.xml — Admin User**
```xml
<tomcat-users>
    <user username="admin" password="password" roles="manager-gui,admin-gui"/>
</tomcat-users>
```
>当你完成后保存并关闭文件。

>默认情况下，较新版本的 Tomcat Manager 和 Host Manager应用的访问限制是连到它自己的服务上。由于我们是安装在远程服务机器，你可能想删除或修改限制。打开相应的```context.xml```文件，更改 IP 地址限制。

>Manager 应用：
```
sudo nano /vagrant/environment/apache-tomcat-8080/webapps/manager/META-INF/context.xml
```
>Host Manager 应用：
```
sudo nano /vagrant/environment/apache-tomcat-8080/webapps/host-manager/META-INF/context.xml
```
>文件内，配置允许来自任何地方的 IP 地址连接。或配置只允许自己的 IP 地址访问，将允许访问的 公共 IP 地址放入列表中：

**context.xml files for Tomcat webapps**
```xml
<Context antiResourceLocking="false" privileged="true" >
  
</Context>
```
>配置说明：
```text
提供两个参数配置：RemoteHostValve 和 RemoteAddrValve，前者用于限制主机名，后者用于限制 IP 地址；
allow：允许访问
deny：拒绝访问
```
```xml
<Valve className="org.apache.catalina.valves.RemoteAddrValve" allow="192.168.0.47" deny=""/>
```
>当完成后保存并退出。

>为使修改生效，要重启 Tomcat 服务：
```
sudo systemctl restart tomcat8080
```

步骤 8：访问 Web 接口
---------------------
>通过域名或 IP 访问
```
http://server_domain_or_IP:8080
```

>Manager App 访问
```
http://server_domain_or_IP:8080/manager/html
```

>Host Manager
```
http://server_domain_or_IP:8080/host-manager/html/
```

参考资料
--------
- [How To Install Apache Tomcat 8 on Ubuntu 16.04](https://www.digitalocean.com/community/tutorials/how-to-install-apache-tomcat-8-on-ubuntu-16-04)
- [Tomcat 中实现 IP 访问限制](http://fanli7.net/a/bianchengyuyan/C__/20140225/473164.html)
- [How To Use Systemctl to Manage Systemd Services and Units](https://www.digitalocean.com/community/tutorials/how-to-use-systemctl-to-manage-systemd-services-and-units)