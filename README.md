# ansible

## To Get Started (running Vagrant machines int his case)
#### Start vagrant VM sessions by typing `vagrant up`
#### The following has been incorporated to VagrantFile setup already
Install ansible packages (Ubuntu in this case) on control machine
First log onto control server
```
vagrant ssh
sudo -s 
apt-get install software-properties-common
apt-add-repository ppa:ansible/ansible
apt-get update
apt-get install ansible
```
#### To Log into Ansible control session.
type `vagrant ssh`



