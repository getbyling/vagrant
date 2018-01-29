01 Vagrant Vagrantfile 配置说明
===============================

>Vagrant Box 名称
```text
config.vm.box = "ubuntu_xenial"
```

>Vagrant Box 路径
```ext
config.vm.box_url = "E:/vagrant-tools/vagrant-box/xenial-server-cloudimg-amd64-vagrant.box"
```

>连接超时时间
```text
config.vm.boot_timeout = 600
```

>连接端口设置
```text
config.vm.network "forwarded_port", guest: 8080, host: 9080
config.vm.network "forwarded_port", guest: 8081, host: 9081
config.vm.network "forwarded_port", guest: 80, host: 7080
config.vm.network "forwarded_port", guest: 9200, host: 19200
```

>设置 vagrant box 内存大小及名称
```text
config.vm.provider "virtualbox" do |vb|
#   # Display the VirtualBox GUI when booting the machine
#   vb.gui = true
#
#   # Customize the amount of memory on the VM:
    vb.memory = "1024"
    vb.name = "ubuntu_tomcat_vb"
end
```