# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
  
  config.vm.box = "ubuntu/xenial64"

  # This is where you set up the Virtual Machine power paremeters like RAM etc.
  config.vm.provider "virtualbox" do |vb|
    vb.memory = 4096
    vb.cpus = 4
#    -this option makes the virtual machine faster and is unimportant
    vb.customize ["modifyvm", :id, "--ioapic", "on"]
  end

  # Network - The host is your computer, guest is the virtual computer
  config.vm.network "forwarded_port", guest: 3000, host: 3000
#   - 3306 is usually used for MySQL
  config.vm.network "forwarded_port", guest: 3306, host: 3306
#   -this is where you change the ip address for the virutal machine
  config.vm.network "private_network", ip: "192.168.10.100"

  # config.vm.network "private_network", type: "dhcp"


  # Shared Folders
#   - the second parameter after the comma is the root folder address of Virtual Machine, the nfs is a file setting parameter used by Mac and Linus Systems to make things more efficient.
  config.vm.synced_folder ".", "/home/vagrant/projects", type: "nfs"

#  ****provisions contain the Specifics of how the development environment and Dependencies are set up.
  config.vm.provision "shell", inline: <<-SHELL
    # update 
    sudo apt-get update
    # upgrade
    sudo apt-get upgrade
    # install git
    sudo apt-get install -y git
    # install extra libraries
    sudo apt-get install -y g++
    sudo apt-get install curl python-software-properties
    apt-get install software-properties-common
    apt-get update
    # install node js 10.x.x stable version
    curl -sL https://deb.nodesource.com/setup_10.x | sudo -E bash -
    apt-get install -y nodejs
    # install gulp-cli globally
    npm install gulp-cli -g
    # Apache
    apt-get install -y apache2
    # Enable Apache Mods
    a2enmod rewrite
    #Add Onrej PPA Repo
    apt-add-repository ppa:ondrej/php
    apt-get update
    # Install PHP
    apt-get install -y php7.2
    # PHP Apache Mod
    apt-get install -y libapache2-mod-php7.2
    # Restart Apache
    service apache2 restart
    # PHP Mods
    apt-get install -y php7.2-common
    apt-get install -y php7.2-mcrypt
    apt-get install -y php7.2-zip
    # Set MySQL Pass
    debconf-set-selections <<< 'mysql-server mysql-server/root_password password root'
    debconf-set-selections <<< 'mysql-server mysql-server/root_password_again password root'
    # Install MySQL
    apt-get install -y mysql-server
    # PHP-MYSQL lib
    apt-get install -y php7.2-mysql
    # Restart Apache
    sudo service apache2 restart
  SHELL
end
