Vagrant Ubuntu JDK 安装整理
===========================
### JDK 准备
>这里提供的是 JDK 8 下载地址，需要注意的是 JDK 的位数版本要与 Vagrant 虚拟机上的系统位数版本保持一致，否则装上不能使用。[JDK 下载](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)


### 设置环境变量
```
 cd ~
```
```
 sudo vi .bashrc
```
**文件末尾追加以下内容：**
```
#set oracle jdk environment
#这里要注意目录要换成自己解压的jdk 目录，如：environment/jdk1.8.0_144
export JAVA_HOME=/vagrant/environment/jdk1.8.0_144
export JRE_HOME=${JAVA_HOME}/jre
export CLASSPATH=.:${JAVA_HOME}/lib:${JRE_HOME}/lib
export PATH=${JAVA_HOME}/bin:$PATH
```
**使环境变量马上生效**
```
 source .bashrc
```
### 设置系统默认 JDK 版本
```
 sudo update-alternatives --install "/usr/bin/java" "java" "/vagrant/environment/jdk1.8.0_144/bin/java" 1 
 sudo update-alternatives --install "/usr/bin/javac" "javac" "/vagrant/environment/jdk1.8.0_144/bin/javac" 1 
 sudo update-alternatives --install "/usr/bin/jar" "jar" "/vagrant/environment/jdk1.8.0_144/bin/jar" 1
 sudo update-alternatives --install "/usr/bin/javah" "javah" "/vagrant/environment/jdk1.8.0_144/bin/javah" 1 
 sudo update-alternatives --install "/usr/bin/javap" "javap" "/vagrant/environment/jdk1.8.0_144/bin/javap" 1 
```
**执行设置：**
```
 sudo update-alternatives --config java
```
### 测试 JDK
```
 java -version
```

**OK，安装成功**