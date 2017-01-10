# README #

one way to use vagrant to create an Islandora 7.x-1.8 virtual machine using VirtualBox setting the Linux distribution at initial startup

## How do I get set up? ##

* Dependencies
     * install Git, Vagrant, and VirtualBox

* Deployment instructions
     * set environmental variables 
     * defaults are:
          * ISLANDORA_VAGRANT_CPUS  = 2
          * ISLANDORA_VAGRANT_MEMORY = 3048
          * ISLANDORA_VAGRANT_HOSTNAME = islandora
          * ISLANDORA_VAGRANT_OS  = ubuntu/trusty64

```
#!bash
cd [project directory name]
git clone https://bitbucket.org/ed_f/i7x1x.git [optional preferred directory name]
cd  [directory name created from previous command]
vagrant box update *(optional step)*
vagrant up
```

## Setting Environment Variables ##

### Linux ###
```export ISLANDORA_VAGRANT_CPUS="2"```
### OS X ###
```export ISLANDORA_VAGRANT_CPUS="2"```
### Windows ###
```set %ISLANDORA_VAGRANT_CPUS="2"```