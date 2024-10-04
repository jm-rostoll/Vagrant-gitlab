# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://vagrantcloud.com/search.
  config.vm.box = "generic/ubuntu2204"

  # Disable automatic box update checking. If you disable this, then
  # boxes will only be checked for updates when the user runs
  # `vagrant box outdated`. This is not recommended.
  # config.vm.box_check_update = false

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  # NOTE: This will enable public access to the opened port
  config.vm.network "forwarded_port", guest: 80, host: 8080

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine and only allow access
  # via 127.0.0.1 to disable public access
  # config.vm.network "forwarded_port", guest: 80, host: 8080, host_ip: "127.0.0.1"

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

  # Disable the default share of the current code directory. Doing this
  # provides improved isolation between the vagrant box and your host
  # by making sure your Vagrantfile isn't accessible to the vagrant box.
  # If you use this you may want to enable additional shared subfolders as
  # shown above.
  # config.vm.synced_folder ".", "/vagrant", disabled: true

  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  # Example for VirtualBox:
  #
  # config.vm.provider "virtualbox" do |vb|
  #   # Display the VirtualBox GUI when booting the machine
  #vb.gui = true
  #
  #   # Customize the amount of memory on the VM:
  #vb.memory = "4096"
  # end
  #
  # View the documentation for the provider you are using for more
  # information on available options.


  config.vm.provision "shell", inline: <<-SHELL
  # Mettre à jour le système
  sudo apt-get update -y
  sudo apt-get upgrade -y

  # Installer les dépendances nécessaires
  sudo apt-get install -y curl openssh-server ca-certificates tzdata perl

  # Ajouter le dépôt officiel de GitLab
  curl https://packages.gitlab.com/install/repositories/gitlab/gitlab-ce/script.deb.sh | sudo bash

  # Installer GitLab CE (Community Edition)
  sudo apt-get install -y gitlab-ce
  
 # Créer un certificat auto-signé pour GitLab
    sudo mkdir -p /etc/gitlab/ssl
    sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/gitlab/ssl/gitlab.example.com.key -out /etc/gitlab/ssl/gitlab.example.com.crt -subj "/C=US/ST=State/L=City/O=Organization/OU=OrgUnit/CN=gitlab.example.com"

    # Définir les permissions appropriées pour le certificat
    sudo chmod 600 /etc/gitlab/ssl/gitlab.example.com.key
    sudo chmod 644 /etc/gitlab/ssl/gitlab.example.com.crt

    # Configurer GitLab pour utiliser le certificat SSL
    sudo sed -i "s/# external_url 'http:\/\/gitlab.example.com'/external_url 'https:\/\/gitlab.example.com'/" /etc/gitlab/gitlab.rb
    sudo sed -i "s/# nginx\['redirect_http_to_https'\] = false/nginx['redirect_http_to_https'] = true/" /etc/gitlab/gitlab.rb
    sudo sed -i "s/# nginx\['ssl_certificate'\] = \"\/etc\/gitlab\/ssl\/gitlab.example.com.crt\"/nginx['ssl_certificate'] = '\/etc\/gitlab\/ssl\/gitlab.example.com.crt'/" /etc/gitlab/gitlab.rb
    sudo sed -i "s/# nginx\['ssl_certificate_key'\] = \"\/etc\/gitlab\/ssl\/gitlab.example.com.key\"/nginx['ssl_certificate_key'] = '\/etc\/gitlab\/ssl\/gitlab.example.com.key'/" /etc/gitlab/gitlab.rb

  # Configurer GitLab (l'URL est définie par défaut à http://localhost)
  sudo gitlab-ctl reconfigure
SHELL

end
