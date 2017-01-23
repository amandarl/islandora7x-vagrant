# README #

Create an Islandora 7.x-1.8 virtual machine using VirtualBox.  
This will set the Linux distribution at initial startup. CentOS 7 and Ubuntu 14.04 (Trusty) are inital distributions implemented. 
The latest available Ansible version for the distro is automatically installed.

# How do I get set up? #

Install Dependencies.

## Dependencies ##

Git
Vagrant 1.9.x (https://www.vagrantup.com/downloads.html)
VirtualBox 5.x (https://www.virtualbox.org/)

## Deployment Instructions ##
* set environmental variables 
     * defaults are:
          * ISLANDORA_VAGRANT_CPUS  = 2
          * ISLANDORA_VAGRANT_MEMORY = 3048
          * ISLANDORA_VAGRANT_HOSTNAME = islandora
          * ISLANDORA_VAGRANT_OS  = ubuntu/trusty64

* creating virtual machine

```
#!bash
cd [project directory name]
git clone https://github.com/edf/islandora7x-vagrant [optional preferred directory name]
cd  [directory name created from previous command]
vagrant box update *(optional step)*
vagrant up
vagrant ssh
```
then for testing VM
```
sudo ansible-playbook /vagrant/playbook.yml 
```
or run the Ansible based Islandora install

```
git clone https://github.com/edf/islandora-7.x-enterprise-ansible.git yourDirNameHere
cd yourDirNameHere
sudo ansible-playbook install-islandora-7.x-enterprise.yaml
```

### examples of Setting Environment Variables on host computer ###

#### Linux or Git Bash on Windows ####
```export ISLANDORA_VAGRANT_CPUS="2"```
#### OS X ####
```export ISLANDORA_VAGRANT_CPUS="2"```
#### Windows ####
```set %ISLANDORA_VAGRANT_CPUS%="2"```

# Notes

## Common Errors

### required for initial run if using CentOS 7

```
#!php
$ vagrant box update
==> default: Box 'geerlingguy/centos7' not installed, can't check for updates
```

substitute the following:
```
vagrant box add geerlingguy/centos7 
vagrant box update
vagrant up
```
