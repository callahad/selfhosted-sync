# -*- mode: ruby -*-
# vim: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.define "syncserver"

  config.vm.box = "ubuntu/trusty64"

  config.vm.network "private_network", ip: "192.168.33.78"

  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "ansible.yml"
  end
end
