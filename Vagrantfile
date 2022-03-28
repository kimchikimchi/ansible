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

  config.vm.box = "ubuntu/bionic64"

  config.vm.define "control", primary: true do |h|
    h.vm.hostname = "control"
    h.vm.network "private_network", ip: "192.168.56.10"
    h.vm.provision :shell, :inline => <<'EOF'
if [ ! -f "/home/vagrant/.ssh/id_rsa" ]; then
  ssh-keygen -t rsa -N "" -f /home/vagrant/.ssh/id_rsa
fi
cp /home/vagrant/.ssh/id_rsa.pub /vagrant/control.pub
echo control > /etc/hostname
cat << 'SSHEOF' > /home/vagrant/.ssh/config
Host *
  StrictHostKeyChecking no
  UserKnownHostsFile=/dev/null
SSHEOF

# Add Ansible repo and install base Ansible
apt-get install software-properties-common
apt-add-repository ppa:ansible/ansible
apt-get update
apt-get install ansible --yes

chown -R vagrant:vagrant /home/vagrant/.ssh/
EOF
  end

  config.vm.define "lb01" do |h|
    h.vm.hostname = "lb01"
    h.vm.network "private_network", ip: "192.168.56.101"
    h.vm.provision :shell, inline: 'echo lb01 > /etc/hostname;  cat /vagrant/control.pub >> /home/vagrant/.ssh/authorized_keys'
  end

  config.vm.define "app01" do |h|
    h.vm.hostname = "app01"
    h.vm.network "private_network", ip: "192.168.56.111"
    h.vm.provision :shell, inline: 'echo app01 > /etc/hostname; cat /vagrant/control.pub >> /home/vagrant/.ssh/authorized_keys'
  end

  config.vm.define "app02" do |h|
    h.vm.hostname = "app02"
    h.vm.network "private_network", ip: "192.168.56.112"
    h.vm.provision :shell, inline: 'echo app02 > /etc/hostname; cat /vagrant/control.pub >> /home/vagrant/.ssh/authorized_keys'
  end

  config.vm.define "db01" do |h|
    h.vm.hostname = "db01"
    h.vm.network "private_network", ip: "192.168.56.121"
    h.vm.provision :shell, inline: 'echo db01 > /etc/hostname; cat /vagrant/control.pub >> /home/vagrant/.ssh/authorized_keys'
  end
end
