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
   $ sudo groupadd tomcat
```
**2. ����һ���µ� tomcat**
>�û����û�Ȩ��```(/opt/tomcat)```Ŀ¼����ͨ shell Ȩ��```(/bin/false)```��
```
   $ sudo useradd -s /bin/false -g tomcat -d /opt/tomcat tomcat
```
>������```tomcat``` �û������ڿ����øմ������û���װ���ص� Tomcat �ˡ�

## ���� 3����װ Tomcat
**1. �л�Ŀ¼**
```
   $ cd /tmp
```
**2. �� curl ����**
```
   $ curl -O http://apache.mirrors.ionfish.org/tomcat/tomcat-8/v8.5.5/bin/apache-tomcat-8.5.5.tar.gz
```
**3. �� mkdir ������/opt/tomcat��Ŀ¼**
```
   $ sudo mkdir /opt/tomcat
```
**4. �� tar ��ѹ�ļ�����/opt/tomcat����**
```
   $ sudo tar xzvf apache-tomcat-8*tar.gz -C /opt/tomcat --strip-components=1
```

## ���� 4������Ȩ��
>```tomcat```�û�����Ҫ�з��� Tomcat ��װĿ¼��Ȩ�ޡ�

**1. �л� tomcat ��װ��Ŀ¼**
```
$ cd /opt/tomcat
```
**2. ���� tomcat ����䰲װ��Ŀ¼Ȩ��**
```
$ sudo chgrp -R tomcat /opt/tomcat
```
**3. ���� tomcat ��������```conf```Ŀ¼Ȩ��**
```
$ sudo chmod -R g+r conf
$ sudo chmod g+x conf
```
**4. ���� tomcat �û�����```webapps/```��```work/```��```temp/```��```logs/```Ŀ¼Ȩ��**
```
$ sudo chown -R tomcat webapps/ work/ temp/ logs/
```
>���������ϵ�Ȩ�޺󣬾��ܽ���```Tomcat```ϵͳ����Ĵ����ˡ�

## ���� 5������ϵͳ�����ļ�
>Ϊ������ Tomcat ������Ҫ��װϵͳ�����ļ���

>Tomcat ����������```JAVA_HOME```���鿴��������λ�ã�ִ���������
```
$ sudo update-java-alternatives -l
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
$ sudo nano /etc/systemd/system/tomcat.service
```


**�ο�����**
>https://www.digitalocean.com/community/tutorials/how-to-install-apache-tomcat-8-on-ubuntu-16-04