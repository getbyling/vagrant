Windows 下 Vagrant 安装
=======================
Vagrant 环境工具
----------------
### Vagrant 下载
```
https://www.vagrantup.com/downloads.html
```
### VirtualBox 下载
```
https://www.virtualbox.org/wiki/Downloads
```
### Git 下载
```
https://git-scm.com/downloads
```

VirtualBox 配置
---------------
>VirtualBox安装完成后，默认存放虚拟机镜像文件的位置在系统盘，建议存储在其他磁盘下，具体步骤如下：
>   1. 打开VirtualBox，打开管理-> 全局设置 （快捷键是 Ctrl-G ）
>   2. 选择 常规 里的 默认虚拟电脑位置(M)
>   3. 设置为非系统盘的位置

## Vagrant 环境配置
>vagrant对于虚拟机的管理分成两个部分：box和Machine，box是指初始的未部署的虚拟机镜像文件，这个文件相当于是虚拟机的一个模板，可以进行无限制次数的复制，Machine指处于可运行状态下的虚拟机，当使用vagrant添加box(vagrant add)时，对于windows用户，vagrant会默认将这些虚拟机模板镜像文件存放在c:\User.Vagrant.d文件夹下，当使用vagrant添加的box文件较多时，这个目录将会变得非常大，建议转移到其他磁盘分区，具体步骤如下：
>   1. 将 c:\User.vagrant.d目录移动到新的位置
>   2. 设置VAGRANT_HOME 环境变量指向新的位置