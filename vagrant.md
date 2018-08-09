```
$ vagrant box add {title} {url} 

$ vagrant init {title}

$ vagrant up 
```

| vagrant box add       | 添加box的操作                                           |
| --------------------- | ------------------------------------------------------- |
| vagrant init          | 初始化box的操作，会生成vagrant的配置文件Vagrantfile     |
| vagrant up            | 启动本地环境                                            |
| vagrant ssh           | 通过 ssh 登录本地环境所在虚拟机                         |
| vagrant halt          | 关闭本地环境                                            |
| vagrant suspend       | 暂停本地环境                                            |
| vagrant resume        | 恢复本地环境                                            |
| vagrant reload        | 修改了 Vagrantfile 后，使之生效（相当于先 halt，再 up） |
| vagrant destroy       | 彻底移除本地环境                                        |
| vagrant box list      | 显示当前已经添加的box列表                               |
| vagrant box remove    | 删除相应的box                                           |
| vagrant package       | 打包命令，可以把当前的运行的虚拟机环境进行打包          |
| vagrant plugin        | 用于安装卸载插件                                        |
| vagrant status        | 获取当前虚拟机的状态                                    |
| vagrant global-status | 显示当前用户Vagrant的所有环境状态                       |



# 多机配置

```
# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.

Vagrant.configure("2") do |config|
 

  config.vm.define :node01 do |node01|
    node01.vm.provider "virtualbox" do |v|
          v.customize ["modifyvm", :id, "--name", "node01", "--memory", "512"]
    end
    node01.vm.box = "centos7"
    node01.vm.hostname = "node01"
    node01.vm.network :private_network, ip: "192.168.33.10"
  end

  config.vm.define :node02 do |node02|
    node02.vm.provider "virtualbox" do |v|
          v.customize ["modifyvm", :id, "--name", "node02", "--memory", "512"]
    end
    node02.vm.box = "centos7"
    node02.vm.hostname = "node02"
    node02.vm.network :private_network, ip: "192.168.33.11"
  end

  
end
```

 ip addr

# 修改root密码

sudo passwd root

# 使用密钥登陆

.vagrant.d/insecure_private_key  保存的是私钥,我下载的box仅支持公钥登陆可以使用这个方式

用户名使用vagrant