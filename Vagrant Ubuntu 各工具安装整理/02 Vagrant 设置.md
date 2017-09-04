Vagrant 设置
============

Vagrant 使用
------------
>安装完 git 后，右击鼠标选择“Git Bash Here” 打开 git。 通过 git 使用 vagrant， vagrant 是以 CLI 的形式操作。

Vagrant 常用指令
----------------
>vagrant init
```text
对 vagrant 初始化
```

>vagrant status
```text
查看 vagrant 状态
```

>vagrant global-status
```
查看 vagrant 全局状态
```

>vagrant box list
```text
查看当前所有 vagrant box 列表
```

>vagrant box add box_name path/add.box
```text
新增 vagrant box 名称 路径
```

>vagrant up
```text
打开 vagrant 虚拟机
```

>vagrant ssh
```text
登录 vagrant 虚拟机
```

>vagrant halt
```text
先关闭虚拟机
```

>vagrant 启动后的默认端口映射：
```text
default: 80 (guest) => 7080 (host) (adapter 1)
default: 22 (guest) => 2222 (host) (adapter 1)
```
