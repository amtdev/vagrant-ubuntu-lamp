# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://vagrantcloud.com/search.
  config.vm.box = "ubuntu/jammy64"
  config.vm.box_download_insecure = true #added by Amos for curl error
  #config.vm.box_version = "20220420"
  config.vm.network :forwarded_port, guest: 3306, host: 3306
  config.vm.network :private_network, ip: '192.168.33.22'
  config.vm.provider "virtualbox" do |vb|
    vb.memory = "4096"
    vb.cpus = 2
  end

  config.vm.provision "shell", inline: <<-SHELL
      # Update package lists
        sudo apt-get update

        # Install Apache
        sudo apt-get install -y apache2

        # Install MySQL and set root password (for simplicity, using "root" as the password)
        sudo debconf-set-selections <<< "mysql-server mysql-server/root_password password root"
        sudo debconf-set-selections <<< "mysql-server mysql-server/root_password_again password root"
        sudo apt-get install -y mysql-server

        # Install PHP and required extensions
        sudo apt-get install -y php libapache2-mod-php php-mysql

        # Enable Apache rewrite module
        sudo a2enmod rewrite
        sudo a2enmod ssl

        # Restart Apache
        sudo service apache2 restart

        # Add the repository 'ppa:ondrej/php'
        sudo apt-get install software-properties-common
        sudo add-apt-repository ppa:ondrej/php
        sudo add-apt-repository ppa:ondrej/apache2
        sudo apt update
        # Install PHP 7.4
        sudo apt install -y php7.4 php7.4-cli libapache2-mod-php7.4
        sudo apt install -y php7.4-imagick php7.4-gettext php7.4-memcache php7.4-apcu php7.4-pear php7.4-xml php7.4-xmlrpc
        sudo apt install -y php7.4-memcached
        sudo apt install -y php7.4-common php7.4-mysql php7.4-cgi
        sudo apt install -y php7.4-curl php7.4-zip php7.4-mbstring php7.4-xmlrpc php7.4-gd php7.4-xml php7.4-xsl
        sudo apt install -y php7.4-dev php7.4-bz2 php7.4-intl php7.4-json php7.4-opcache php7.4-readline
        sudo apt install -y php7.4-imap php7.4-pspell php7.4-recode php7.4-sqlite3 php7.4-tidy php7.4-bcmath php7.4-mcrypt 7.4-xdebug

        # Update the Apache's PHP version
        sudo a2dismod php8.1
        sudo a2enmod php7.4
        sudo service apache2 restart

        # Update the CLI PHP version
        sudo update-alternatives --set php /usr/bin/php7.4


  SHELL
 end