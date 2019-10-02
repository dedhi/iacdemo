# -*- mode: ruby -*-
# vi: set ft=ruby :

# README
#
# Getting Started:
# 1. vagrant plugin install vagrant-hostmanager
# 2. vagrant up
# 3. vagrant ssh
#
# This should put you at the control host
#  with access, by name, to other vms
Vagrant.configure(2) do |config|
  config.hostmanager.enabled = true

  config.vm.box = "ubuntu/trusty64"

  config.vm.define "control", primary: true do |control|
    control.vm.hostname = "control"
	control.vm.synced_folder ".", "/vagrant"
	control.vm.network "private_network", ip: "192.168.135.10"
    control.vm.provision :shell, :inline => <<'EOF'
if [ ! -f "/home/vagrant/.ssh/id_rsa" ]; then
  ssh-keygen -t rsa -N "" -f /home/vagrant/.ssh/id_rsa
fi
cp /home/vagrant/.ssh/id_rsa.pub /vagrant/control.pub

cat << 'SSHEOF' > /home/vagrant/.ssh/config
Host *
  StrictHostKeyChecking no
  UserKnownHostsFile=/dev/null
SSHEOF

chown -R vagrant:vagrant /home/vagrant/.ssh/
EOF
  end

  config.vm.define "lb01" do |lb01|
    lb01.vm.hostname = "lb01"
	lb01.vm.synced_folder ".", "/vagrant"
	lb01.vm.network "private_network", ip: "192.168.135.101"
    lb01.vm.provision :shell, inline: 'cat /vagrant/control.pub >> /home/vagrant/.ssh/authorized_keys'
  end

  config.vm.define "app01" do |app01|
    app01.vm.hostname = "app01"
	app01.vm.synced_folder ".", "/vagrant"
	app01.vm.network "private_network", ip: "192.168.135.111"
    app01.vm.provision :shell, inline: 'cat /vagrant/control.pub >> /home/vagrant/.ssh/authorized_keys'
  end

  config.vm.define "app02" do |app02|
    app02.vm.hostname = "app02"
	app02.vm.synced_folder ".", "/vagrant"
	app02.vm.network "private_network", ip: "192.168.135.112"
    app02.vm.provision :shell, inline: 'cat /vagrant/control.pub >> /home/vagrant/.ssh/authorized_keys'
  end

  config.vm.define "db01" do |db01|
    db01.vm.hostname = "db01"
	db01.vm.synced_folder ".", "/vagrant"
	db01.vm.network "private_network", ip: "192.168.135.121"
    db01.vm.provision :shell, inline: 'cat /vagrant/control.pub >> /home/vagrant/.ssh/authorized_keys'
  end
end
