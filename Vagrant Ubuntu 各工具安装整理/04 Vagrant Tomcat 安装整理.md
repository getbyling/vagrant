Vagrant Tomcat ��װ����
=======================

Tomcat ׼��
-----------
>�����ṩ�� Tomcat ���ذ汾�� 8��[Tomcat 8 ����](http://mirrors.tuna.tsinghua.edu.cn/apache/tomcat/tomcat-8/v8.5.20/bin/apache-tomcat-8.5.20.tar.gz) 

��װ����
--------
>���µİ�װ�������� ```Ubuntu 16.04``` Ϊʾ����

���� 1����װ JDK
-----------------
>�� Ubuntu �� JDK �İ�װ�����Ķ�[��Vagrant JDK ��װ����](http://note.youdao.com/share/?id=bdcef13988cb1c4f9e7d11c0aba26ce1&type=note#/)��ƪ���¡�������Ͳ���׸����-_-��

���� 2������ Tomcat �û�
------------------------
>Ϊ�˰�ȫ��������� tomcat �û����顣���������û�������ִ�� tomcat ������ vagrant ���˽����ʡ�ԣ�-_-����

**1. �����û���**
```
   sudo groupadd tomcat
```

**2. ����һ���µ� tomcat**
>�û����û�Ȩ��```(/vagrant/environment/apache-tomcat-8080)```Ŀ¼����ͨ shell Ȩ��```(/bin/false)```��
```
   sudo useradd -s /bin/false -g tomcat -d /vagrant/environment/apache-tomcat-8080 tomcat
```
>������```tomcat``` �û������ڿ����øմ������û���װ���ص� Tomcat �ˡ�

���� 3����װ Tomcat
-------------------
**1. �л�Ŀ¼**
```
   cd /vagrant/environment
```

**2. �� curl ����**
```
   curl -O http://apache.mirrors.ionfish.org/tomcat/tomcat-8/v8.5.5/bin/apache-tomcat-8.5.5.tar.gz
```

**3. �� mkdir ������/vagrant/environment/apache-tomcat-8080��Ŀ¼**
```
   sudo mkdir /vagrant/environment/apache-tomcat-8080
```

**4. �� tar ��ѹ�ļ�����/vagrant/environment/apache-tomcat-8080����**
```
   sudo tar xzvf apache-tomcat-8*tar.gz -C /vagrant/environment/apache-tomcat-8080 --strip-components=1
```

���� 4������Ȩ��
----------------
>```tomcat```�û�����Ҫ�з��� Tomcat ��װĿ¼��Ȩ�ޡ�

**1. �л� tomcat ��װ��Ŀ¼**
```
cd /vagrant/environment/apache-tomcat-8080
```

**2. ���� tomcat ����䰲װ��Ŀ¼Ȩ��**
```
sudo chgrp -R tomcat /vagrant/environment/apache-tomcat-8080
```

**3. ���� tomcat ��������```conf```Ŀ¼Ȩ��**
```
sudo chmod -R g+r conf
sudo chmod g+x conf
```

**4. ���� tomcat �û�����```webapps/```��```work/```��```temp/```��```logs/```Ŀ¼Ȩ��**
```
sudo chown -R tomcat webapps/ work/ temp/ logs/
```

>���������ϵ�Ȩ�޺󣬾��ܽ���```Tomcat```ϵͳ����Ĵ����ˡ�

���� 5������ϵͳ�����ļ�
------------------------
>Ϊ������ Tomcat ������Ҫ��װϵͳ�����ļ���

>Tomcat ����������```JAVA_HOME```���鿴��������λ�ã�ִ���������
```
sudo update-java-alternatives -l
```

>���
```
java-1.8.0-openjdk-amd64       1081       /usr/lib/jvm/java-1.8.0-openjdk-amd64
```

>ͨ����ȡ��```JAVA_HOME```��������������׷��```/jre```��������ϳɵ�����������£�
```text
JAVA_HOME:
/vagrant/environment/apache-tomcat-8080/jre
```
>����������Լ���������Ϊ׼��

>ͨ������������Դ���ϵͳ�����ļ����򿪷���```tomcat.service```��```/etc/systemd/system```Ŀ¼�У�
```
sudo vi /etc/systemd/system/tomcat8080.service
```
>�����µ�����ճ���������ļ��С���Ҫ�޸ĵ�����```JAVA_HOME```,�п��ܻ�Ҫ�޸���```CATALINA_OPTS```�ڴ����ã�

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
>����˵����
```text
1. CATALINA_PID����������
2. CATALINA_OPTS��TOMCAT ʹ�ò������á�
```
>������ɺ󣬱��沢�˳���

>������������ִ���ػ���������ϵͳ�ͻ�֪���½��ķ����ļ���
```
sudo systemctl daemon-reload
```
>���� Tomcat ����
```
sudo systemctl start tomcat8080
```

>�ٴ����� Tomcat ���񣬽��ᱨ��
```
sudo systemctl start tomcat8080
```

>ֹͣ��ǰ�����з��񣬿���ʹ��```stop```���������
```text
sudo systemctl stop tomcat8080.service
```

���� 6����������ǽ������ Tomcat ����
------------------------------------
>���� Tomcat �����Ѿ���������������֤���Ƿ��ܷ��ʵ�Ĭ��ҳ��
 ��ʼǰ�����Ǳ����������ǽ�Է��������ǰ����������ǰ�ķ���ǽ```ufw```�ǿ���״̬��
 Tomcat ��ͨ�� 8080 �˿ڽ��ճ�����������������������ָ�
```
sudo ufw allow 8080
```

>�޸������ǽ����������������ͨ�������� IP ��ַ�Ӷ˿�```8080```����Ĭ��ҳ�档
```
#�������
http://server_domain_or_IP:8080
```

>�����ɹ��ķ��� Tomcat���������ͬʱ�����Զ����� Tomcat ����
```
sudo systemctl enable tomcat8080
```

���� 7������ Tomcat Web ����ӿ�
--------------------------------
>��ʹ�� Tomcat web ����Ӧ���嵥ʱ�����Ǳ������һ�����ǵ� Tomcat �����ʺš�����Ҫ�༭```tomcat-users.xml```�ļ���
```
sudo nano /vagrant/environment/apache-tomcat-8080/conf/tomcat-users.xml
```

>�����û�˭�ܷ���```manager-gui```��```admin-gui```���������ܶ����û�����������ʾ������```tomcat-users```��ǩ�䡣ȷ���޸����õ��û��������ǰ�ȫ�ģ�
**tomcat-users.xml �� Admin User**
```xml
<tomcat-users>
    <user username="admin" password="password" roles="manager-gui,admin-gui"/>
</tomcat-users>
```
>������ɺ󱣴沢�ر��ļ���

>Ĭ������£����°汾�� Tomcat Manager �� Host ManagerӦ�õķ����������������Լ��ķ����ϡ����������ǰ�װ��Զ�̷���������������ɾ�����޸����ơ�����Ӧ��```context.xml```�ļ������� IP ��ַ���ơ�

>Manager Ӧ�ã�
```
sudo nano /vagrant/environment/apache-tomcat-8080/webapps/manager/META-INF/context.xml
```
>Host Manager Ӧ�ã�
```
sudo nano /vagrant/environment/apache-tomcat-8080/webapps/host-manager/META-INF/context.xml
```
>�ļ��ڣ��������������κεط��� IP ��ַ���ӡ�������ֻ�����Լ��� IP ��ַ���ʣ���������ʵ� ���� IP ��ַ�����б��У�

**context.xml files for Tomcat webapps**
```xml
<Context antiResourceLocking="false" privileged="true" >
  
</Context>
```
>����˵����
```text
�ṩ�����������ã�RemoteHostValve �� RemoteAddrValve��ǰ������������������������������ IP ��ַ��
allow���������
deny���ܾ�����
```
```xml
<Valve className="org.apache.catalina.valves.RemoteAddrValve" allow="192.168.0.47" deny=""/>
```
>����ɺ󱣴沢�˳���

>Ϊʹ�޸���Ч��Ҫ���� Tomcat ����
```
sudo systemctl restart tomcat8080
```

���� 8������ Web �ӿ�
---------------------
>ͨ�������� IP ����
```
http://server_domain_or_IP:8080
```

>Manager App ����
```
http://server_domain_or_IP:8080/manager/html
```

>Host Manager
```
http://server_domain_or_IP:8080/host-manager/html/
```

�ο�����
--------
- [How To Install Apache Tomcat 8 on Ubuntu 16.04](https://www.digitalocean.com/community/tutorials/how-to-install-apache-tomcat-8-on-ubuntu-16-04)
- [Tomcat ��ʵ�� IP ��������](http://fanli7.net/a/bianchengyuyan/C__/20140225/473164.html)
- [How To Use Systemctl to Manage Systemd Services and Units](https://www.digitalocean.com/community/tutorials/how-to-use-systemctl-to-manage-systemd-services-and-units)