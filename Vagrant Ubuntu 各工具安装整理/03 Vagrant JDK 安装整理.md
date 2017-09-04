Vagrant Ubuntu JDK ��װ����
===========================
### JDK ׼��
>�����ṩ���� JDK 8 ���ص�ַ����Ҫע����� JDK ��λ���汾Ҫ�� Vagrant ������ϵ�ϵͳλ���汾����һ�£�����װ�ϲ���ʹ�á�[JDK ����](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)


### ���û�������
```
 cd ~
```
```
 sudo vi .bashrc
```
**�ļ�ĩβ׷���������ݣ�**
```
#set oracle jdk environment
#����Ҫע��Ŀ¼Ҫ�����Լ���ѹ��jdk Ŀ¼���磺environment/jdk1.8.0_144
export JAVA_HOME=/vagrant/environment/jdk1.8.0_144
export JRE_HOME=${JAVA_HOME}/jre
export CLASSPATH=.:${JAVA_HOME}/lib:${JRE_HOME}/lib
export PATH=${JAVA_HOME}/bin:$PATH
```
**ʹ��������������Ч**
```
 source .bashrc
```
### ����ϵͳĬ�� JDK �汾
```
 sudo update-alternatives --install "/usr/bin/java" "java" "/vagrant/environment/jdk1.8.0_144/bin/java" 1 
 sudo update-alternatives --install "/usr/bin/javac" "javac" "/vagrant/environment/jdk1.8.0_144/bin/javac" 1 
 sudo update-alternatives --install "/usr/bin/jar" "jar" "/vagrant/environment/jdk1.8.0_144/bin/jar" 1
 sudo update-alternatives --install "/usr/bin/javah" "javah" "/vagrant/environment/jdk1.8.0_144/bin/javah" 1 
 sudo update-alternatives --install "/usr/bin/javap" "javap" "/vagrant/environment/jdk1.8.0_144/bin/javap" 1 
```
**ִ�����ã�**
```
 sudo update-alternatives --config java
```
### ���� JDK
```
 java -version
```

**OK����װ�ɹ�**