# -*- mode: ruby -*-
# vi: set ft=ruby :

ssh_path = File.readlines("#{Dir.home}/.ssh/id_rsa.pub").first.strip
home_dir = File.expand_path('~').strip
memory = 4096
cpus = 4
num_of_vms = 2

Vagrant.configure("2") do |config|

  config.vagrant.plugins = "vagrant-libvirt"

  config.vm.box = "debian/buster64"
  config.vm.synced_folder './', '/vagrant', disabled: true
  
  config.vm.provider :libvirt do |domain|
    domain.memory = memory
    domain.cpus = cpus
    domain.disk_driver['cache'] = 'none'
  end

  config.vm.define "dhcp_server" do |server|
    server.vm.network :private_network, :ip => "192.168.121.10"
    server.vm.hostname = "dhcp.server.com"
    server.vm.provision "shell",
      inline: "echo #{ssh_path} >> /home/vagrant/.ssh/authorized_keys"
  end

  config.vm.provision "ansible" do |ansible|  
    ansible.become = "yes"
    ansible.become_user = "root"
    ansible.playbook = "/home/ibrahim/Documents/ansible/dhcp/server.yml"
  end

end
