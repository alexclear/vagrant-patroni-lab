# -*- mode: ruby -*-
# vi: set ft=ruby :

$PATRONI1_IP = "172.16.137.32"
$PATRONI2_IP = "172.16.137.33"
$PATRONI3_IP = "172.16.137.34"

ANSIBLE_RAW_SSH_ARGS = []

ANSIBLE_RAW_SSH_ARGS << " -o IdentityFile=./.vagrant/machines/patroni1/virtualbox/private_key "
ANSIBLE_RAW_SSH_ARGS << " -o IdentityFile=./.vagrant/machines/patroni2/virtualbox/private_key "
ANSIBLE_RAW_SSH_ARGS << " -o IdentityFile=./.vagrant/machines/patroni3/virtualbox/private_key "

Vagrant.configure("2") do |config|
  config.vm.define "patroni1" do |patroni1|
    patroni1.vm.box = "debian/stretch64"
    patroni1.vm.hostname = "patroni1"
    patroni1.vm.network "private_network", ip: $PATRONI1_IP
    patroni1.vm.network "forwarded_port", guest: 3000, host: 3000

    patroni1.vm.provider :virtualbox do |v, override|
      v.gui = false
      v.customize ["modifyvm", :id, "--cpus", 2]
      v.customize ["modifyvm", :id, "--memory", 4096]
      v.customize ["modifyvm", :id, "--cableconnected1", "on"]
      v.customize ["modifyvm", :id, "--cableconnected2", "on"]
    end

    patroni1.vm.provision "shell", inline: "apt-get install -y python"
  end

  config.vm.define "patroni2" do |patroni2|
    patroni2.vm.box = "debian/stretch64"
    patroni2.vm.hostname = "patroni2"
    patroni2.vm.network "private_network", ip: $PATRONI2_IP

    patroni2.vm.provider :virtualbox do |v, override|
      v.gui = false
      v.customize ["modifyvm", :id, "--cpus", 2]
      v.customize ["modifyvm", :id, "--memory", 4096]
      v.customize ["modifyvm", :id, "--cableconnected1", "on"]
      v.customize ["modifyvm", :id, "--cableconnected2", "on"]
    end

    patroni2.vm.provision "shell", inline: "apt-get install -y python"
  end

  config.vm.define "patroni3" do |patroni3|
    patroni3.vm.box = "debian/stretch64"
    patroni3.vm.hostname = "patroni3"
    patroni3.vm.network "private_network", ip: $PATRONI3_IP

    patroni3.vm.provider :virtualbox do |v, override|
      v.gui = false
      v.customize ["modifyvm", :id, "--cpus", 2]
      v.customize ["modifyvm", :id, "--memory", 4096]
      v.customize ["modifyvm", :id, "--cableconnected1", "on"]
      v.customize ["modifyvm", :id, "--cableconnected2", "on"]
    end

    patroni3.vm.provision "shell", inline: "apt-get install -y python"
    patroni3.vm.provision "ansible" do |ansible|
      ansible.playbook = "ansible/site.yml"
      ansible.config_file = "ansible/ansible.cfg"
      ansible.inventory_path = "ansible/inventory"
      ansible.become = true
      ansible.limit = "all"
      ansible.raw_ssh_args = ANSIBLE_RAW_SSH_ARGS
    end
  end
end
