# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/focal64"

  config.vm.network "private_network", ip: "192.168.50.4"
  config.vm.network "forwarded_port", guest: 3000, host: 3000
  

  config.vm.provider "virtualbox" do |vb|
    vb.name = "ubuntu-monitoring-vm"
    vb.memory = 2048
    vb.cpus = 2
    vb.gui = false
  end

  config.vm.provision "ansible_local" do |ansible|
    ansible.playbook = "playbook.yml"
    ansible.extra_vars = { "host_ip" => "192.168.50.4" }
  end
end