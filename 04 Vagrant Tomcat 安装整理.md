Vagrant Tomcat ��װ����
=======================

# Tomcat ׼��
>�����ṩ�� Tomcat ���ذ汾�� 8��[Tomcat 8 ����](http://mirrors.tuna.tsinghua.edu.cn/apache/tomcat/tomcat-8/v8.5.20/bin/apache-tomcat-8.5.20.tar.gz) 

# ��װ����
>���µİ�װ�������� ```Ubuntu 16.04``` Ϊʾ����

## ���� 1����װ JDK
>�� Ubuntu �� JDK �İ�װ�����Ķ�[��Vagrant JDK ��װ����](http://note.youdao.com/share/?id=bdcef13988cb1c4f9e7d11c0aba26ce1&type=note#/)��ƪ���¡�������Ͳ���׸����-_-��

## ���� 2������ Tomcat �û�
>Ϊ�˰�ȫ��������� tomcat �û����顣���������û�������ִ�� tomcat ������ vagrant ���˽����ʡ�ԣ�-_-����

**1. �����û���**
```
   sudo groupadd tomcat
```

**2. ����һ���µ� tomcat**
>�û����û�Ȩ��```(/opt/tomcat)```Ŀ¼����ͨ shell Ȩ��```(/bin/false)```��
```
   sudo useradd -s /bin/false -g tomcat -d /opt/tomcat tomcat
```
>������```tomcat``` �û������ڿ����øմ������û���װ���ص� Tomcat �ˡ�

## ���� 3����װ Tomcat
**1. �л�Ŀ¼**
```
   cd /tmp
```

**2. �� curl ����**
```
   curl -O http://apache.mirrors.ionfish.org/tomcat/tomcat-8/v8.5.5/bin/apache-tomcat-8.5.5.tar.gz
```

**3. �� mkdir ������/opt/tomcat��Ŀ¼**
```
   sudo mkdir /opt/tomcat
```

**4. �� tar ��ѹ�ļ�����/opt/tomcat����**
```
   sudo tar xzvf apache-tomcat-8*tar.gz -C /opt/tomcat --strip-components=1
```

## ���� 4������Ȩ��
>```tomcat```�û�����Ҫ�з��� Tomcat ��װĿ¼��Ȩ�ޡ�

**1. �л� tomcat ��װ��Ŀ¼**
```
cd /opt/tomcat
```

**2. ���� tomcat ����䰲װ��Ŀ¼Ȩ��**
```
sudo chgrp -R tomcat /opt/tomcat
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

## ���� 5������ϵͳ�����ļ�
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
```
JAVA_HOME:
/usr/lib/jvm/java-1.8.0-openjdk-amd64/jre
```
>����������Լ���������Ϊ׼��

>ͨ������������Դ���ϵͳ�����ļ����򿪷���```tomcat.service```��```/etc/systemd/system```Ŀ¼�У�
```
sudo nano /etc/systemd/system/tomcat.service
```
>�����µ�����ճ���������ļ��С���Ҫ�޸ĵ�����```JAVA_HOME```,�п��ܻ�Ҫ�޸���```CATALINA_OPTS```�ڴ����ã�

**/etc/systemd/system/tomcat.service**
```
[Unit]
Description=Apache Tomcat Web Application Container
After=network.target

[Service]
Type=forking

Environment=JAVA_HOME=/usr/lib/jvm/java-1.8.0-openjdk-amd64/jre
Environment=CATALINA_PID=/opt/tomcat/temp/tomcat.pid
Environment=CATALINA_HOME=/opt/tomcat
Environment=CATALINA_BASE=/opt/tomcat
Environment='CATALINA_OPTS=-Xms512M -Xmx1024M -server -XX:+UseParallelGC'
Environment='JAVA_OPTS=-Djava.awt.headless=true -Djava.security.egd=file:/dev/./urandom'

ExecStart=/opt/tomcat/bin/startup.sh
ExecStop=/opt/tomcat/bin/shutdown.sh

User=tomcat
Group=tomcat
UMask=0007
RestartSec=10
Restart=always

[Install]
WantedBy=multi-user.target
```
>������ɺ󣬱��沢�˳���

>������������ִ���ػ���������ϵͳ�ͻ�֪���½��ķ����ļ���
```
sudo systemctl daemon-reload
```
>���� Tomcat ����
```
sudo systemctl start tomcat
```

>�ٴ����� Tomcat ���񣬽��ᱨ��
```
sudo systemctl start tomcat
```

## ���� 6����������ǽ������ Tomcat ����
>���� Tomcat �����Ѿ���������������֤���Ƿ��ܷ��ʵ�Ĭ��ҳ��

>��ʼǰ�����Ǳ����������ǽ�Է��������ǰ����������ǰ�ķ���ǽ```ufw```�ǿ���״̬��

>Tomcat ��ͨ�� 8080 �˿ڽ��ճ�����������������������ָ�
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
sudo systemctl enable tomcat
```

## ���� 7������ Tomcat Web ����ӿ�
>��ʹ�� Tomcat web ����Ӧ���嵥ʱ�����Ǳ������һ�����ǵ� Tomcat �����ʺš�����Ҫ�༭```tomcat-users.xml```�ļ���
```
sudo nano /opt/tomcat/conf/tomcat-users.xml
```

>�����û�˭�ܷ���```manager-gui```��```admin-gui```���������ܶ����û�����������ʾ������```tomcat-users```��ǩ�䡣ȷ���޸����õ��û��������ǰ�ȫ�ģ�
**tomcat-users.xml �� Admin User**
```
<tomcat-users . . .>
    <user username="admin" password="password" roles="manager-gui,admin-gui"/>
</tomcat-users>
```
>������ɺ󱣴沢�ر��ļ���

>Ĭ������£����°汾�� Tomcat Manager �� Host ManagerӦ�õķ����������������Լ��ķ����ϡ����������ǰ�װ��Զ�̷���������������ɾ�����޸����ơ�����Ӧ��```context.xml```�ļ������� IP ��ַ���ơ�

>Manager Ӧ�ã�
```
sudo nano /opt/tomcat/webapps/manager/META-INF/context.xml
```
>Host Manager Ӧ�ã�
```
sudo nano /opt/tomcat/webapps/host-manager/META-INF/context.xml
```
>�ļ��ڣ��������������κεط��� IP ��ַ���ӡ�������ֻ�����Լ��� IP ��ַ���ʣ���������ʵ� ���� IP ��ַ�����б��У�

**context.xml files for Tomcat webapps**
```
<Context antiResourceLocking="false" privileged="true" >
  
</Context>
```
>����ɺ󱣴沢�˳���

>Ϊʹ�޸���Ч��Ҫ���� Tomcat ����
```
sudo systemctl restart tomcat
```

## ���� 8������ Web �ӿ�
>ͨ�������� IP ����
```
#�����
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

**�ο�����**
>https://www.digitalocean.com/community/tutorials/how-to-install-apache-tomcat-8-on-ubuntu-16-04