# -*- mode: ruby -*-
# vi: set ft=ruby :

$timeStamp             = Time.now.strftime("%Y-%m%d-%H%M")
$cpus                  = ENV.fetch("ISLANDORA_VAGRANT_CPUS", "2")
$memory                = ENV.fetch("ISLANDORA_VAGRANT_MEMORY", "3048")
$hostname              = ENV.fetch("ISLANDORA_VAGRANT_HOSTNAME", "islandora")
$virtualBoxDescription = ENV.fetch("ISLANDORA_VAGRANT_VIRTUALBOXDESCRIPTION", "Islandora\nmemory: " + $memory + "\ncreated: " + $timeStamp)
$virtualBoxName        = ENV.fetch("ISLANDORA_VAGRANT_VIRTUALBOXNAME", "Islandora-" + $timeStamp)
# $virtualBoxOsRaw       = ENV.fetch("ISLANDORA_VAGRANT_OS", "ubuntu")
$virtualBoxOsRaw       = ENV["ISLANDORA_VAGRANT_OS"]
$virtualBoxOsLC        = $virtualBoxOsRaw.downcase unless $virtualBoxOsRaw.nil?

if ( $virtualBoxOsLC  == 'centos/7' or $virtualBoxOsLC  == 'centos7')
	 # virtualBox guest addiions installation needs verification ; rsync not found on Win7 
	 # http://stackoverflow.com/questions/34176041/vagrant-with-virtualbox-on-windows10-rsync-could-not-be-found-on-your-path
         #$virtualBoxOs = "centos/7" 
         $virtualBoxOs = "geerlingguy/centos7"
    elsif (  $virtualBoxOsLC == 'rhel7')  # https://ttboj.wordpress.com/2015/02/23/building-rhel-vagrant-boxes-with-vagrant-builder/
         $virtualBoxOs = "rhel-7.2"
    elsif ( $virtualBoxOsLC == 'trusty64')
         $virtualBoxOs =  "ubuntu/trusty64"
    else
         $virtualBoxOs = "ubuntu/trusty64"
end

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.

Vagrant.configure("2") do |config|
  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://atlas.hashicorp.com/search.

  config.vm.box = $virtualBoxOs

  # Ignore this line on VM reload

  #require 'time'
  #offset = ((Time.zone_offset(Time.now.zone) / 60) / 60)
  #timezone_suffix = offset >= 0 ? "+#{offset.to_s}" : "#{offset.to_s}"
  #timezone = 'Etc/GMT' + timezone_suffix
  #config.vm.provision :shell, :inline => "sudo rm /etc/localtime && sudo ln -s /usr/share/zoneinfo/" + timezone + " /etc/localtime", run: "always"

  config.vm.provider "virtualbox" do |v|
    v.name = $virtualBoxName
    #config.vm.network :forwarded_port, guest: 80, host: 8000 # Apache
  end

  config.vm.hostname = $hostname

  config.vm.network :forwarded_port, guest: 8080, host: 9080 # Tomcat
  config.vm.network :forwarded_port, guest: 3306, host: 3306 # MySQL
  config.vm.network :forwarded_port, guest: 8000, host: 8000 # Apache

  config.vm.provider "virtualbox" do |vb|
      vb.customize ["modifyvm", :id, "--memory", $memory]
      vb.customize ["modifyvm", :id, "--cpus", $cpus]
      vb.customize ["modifyvm", :id, "--description", $virtualBoxDescription]
      vb.customize ["modifyvm", :id, "--name", $virtualBoxName]
      # https://github.com/chef/bento/issues/688#issuecomment-252404560
      vb.customize ["modifyvm", :id, "--cableconnected1", "on"]   
  end

  # Setup the shared folder
    shared_dir = "/home/vagrant"
    config.vm.synced_folder "../", shared_dir + "/islandora", type: "virtualbox" 
    # OS X - config.vm.synced_folder "../", shared_dir + "/islandora"
    config.vm.synced_folder "../downloads", "/downloads", type: "virtualbox" 

  # scripts
    #works on Ubuntu guest not CentOS/7 ----- config.vm.provision :shell, inline: "sudo sed -i '/tty/!s/mesg n/tty -s \\&\\& mesg n/' /root/.profile", :privileged =>false
    #    config.vm.provision :shell, path: "./scripts/bootstrap.sh", :args => shared_dir
    #    config.vm.provision :shell, path: "./scripts/post-install.sh", :args => shared_dir
    if File.exist?("./scripts/custom.sh") then
      config.vm.provision :shell, path: "./scripts/custom.sh", :args => shared_dir
    end

  # Run Ansible from the Vagrant VM
  config.vm.provision "ansible_local" do |ansible|
    #ansible.verbose  = true
    ansible.verbose = "vvv"
    #ansible.sudo      = "true"
    #ansible.provisioning_path   = "/vagrant"
    #ansible.limit     = "all"
    ansible.install = "true"
    ansible.sudo = true
    #ansible.playbook = "playbook.yml"              # playbook is not running using vagrant 1.8.6 or 1.9.1
    ansible.playbook  = "./ansible/playbook.yml"   # playbook is not running using vagrant 1.8.6 or 1.9.1
    #ansible.playbook  = "/vagrant/playbook.yml"    # playbook is not running using vagrant 1.8.6 or 1.9.1

  end

  config.vm.provision "shell", privileged: false, inline: <<-EOF
    echo " "
    echo "Vagrant Box provisioned!"
    echo "Local server address is http://"
  EOF
  

#puts "virtualBox guest OS -  #{$virtualBoxOs}  inital value was #{$virtualBoxOsRaw} "
#puts "virtualBox guest CPUs -  #{$cpus}"

  # Disable automatic box update checking. If you disable this, then
  # boxes will only be checked for updates when the user runs
  # `vagrant box outdated`. This is not recommended.
  # config.vm.box_check_update = false

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  # config.vm.network "forwarded_port", guest: 80, host: 8080

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  # config.vm.network "private_network", ip: "192.168.33.10"

  # Create a public network, which generally matched to bridged network.
  # Bridged networks make the machine appear as another physical device on
  # your network.
  # config.vm.network "public_network"

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  # config.vm.synced_folder "../data", "/vagrant_data"

  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  # Example for VirtualBox:
  #
  # config.vm.provider "virtualbox" do |vb|
  #   # Display the VirtualBox GUI when booting the machine
  #   vb.gui = true
  #
  #   # Customize the amount of memory on the VM:
  #   vb.memory = "1024"
  # end
  #
  # View the documentation for the provider you are using for more
  # information on available options.

  # Define a Vagrant Push strategy for pushing to Atlas. Other push strategies
  # such as FTP and Heroku are also available. See the documentation at
  # https://docs.vagrantup.com/v2/push/atlas.html for more information.
  # config.push.define "atlas" do |push|
  #   push.app = "YOUR_ATLAS_USERNAME/YOUR_APPLICATION_NAME"
  # end

  # Enable provisioning with a shell script. Additional provisioners such as
  # Puppet, Chef, Ansible, Salt, and Docker are also available. Please see the
  # documentation for more information about their specific syntax and use.
  # config.vm.provision "shell", inline: <<-SHELL
  #   apt-get update
  #   apt-get install -y apache2
  # SHELL
end
