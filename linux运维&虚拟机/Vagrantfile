# -*- mode: ruby -*-
# vi: set ft=ruby :

app_servers = {
    :hadoop1 => '192.168.137.123',
    :hadoop2 => '192.168.137.124',
    :hadoop3 => '192.168.137.125'
}
Vagrant.configure(2) do |config|
  config.vm.synced_folder ".", "/home/vagrant/sync", disabled: true
  config.ssh.insert_key = false
  # config.ssh.username = 'hadoop'
  # config.ssh.password = '123456'
  
  app_servers.each do |app_server_name, app_server_ip| 
    config.vm.define app_server_name do |coreos|
	  coreos.vm.box = "CentOS-7"
      coreos.vm.hostname = app_server_name.to_s
      coreos.vm.network :private_network, ip: app_server_ip,name: "VirtualBox Host-Only Ethernet Adapter"
      coreos.vm.provider "virtualbox" do |vb|
	    vb.memory = "2048"
      end
    end
  end
end