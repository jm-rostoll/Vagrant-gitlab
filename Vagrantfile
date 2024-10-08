# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
 
  # Utiliser l'image Ubuntu 22.04,trouvable sur https://vagrantcloud.com/search.
    config.vm.box = "generic/ubuntu2204"


    # Configurer le réseau pour accéder à GitLab sur le port 8080
    config.vm.network "forwarded_port", guest: 80, host: 8080

    # Configurer les ressources de la VM
    config.vm.provider "virtualbox" do |vb|
      vb.memory = "4096"
      vb.cpus = "4"
    end

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

   # Configurer GitLab pour utiliser une URL locale
    sudo sed -i "s|# external_url 'http://gitlab.example.com'|external_url 'https://localhost'|" /etc/gitlab/gitlab.rb

    # Désactiver la redirection HTTP vers HTTPS 
    sudo sed -i "s/# nginx\\['redirect_http_to_https'\\] = false/nginx['redirect_http_to_https'] = false/" /etc/gitlab/gitlab.rb

    # Générer un certificat auto-signé pour localhost
    sudo mkdir -p /etc/gitlab/ssl
    sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 \
      -keyout /etc/gitlab/ssl/localhost.key \
      -out /etc/gitlab/ssl/localhost.crt \
      -subj "/C=US/ST=State/L=City/O=Organization/OU=OrgUnit/CN=localhost"

    # Définir les permissions appropriées pour le certificat
    sudo chmod 600 /etc/gitlab/ssl/localhost.key
    sudo chmod 644 /etc/gitlab/ssl/localhost.crt

    # Configurer GitLab pour utiliser le certificat SSL généré pour localhost
    sudo sed -i "s|# nginx\\['ssl_certificate'\\] = \"/etc/gitlab/ssl/gitlab.example.com.crt\"|nginx['ssl_certificate'] = '/etc/gitlab/ssl/localhost.crt'|" /etc/gitlab/gitlab.rb
    sudo sed -i "s|# nginx\\['ssl_certificate_key'\\] = \"/etc/gitlab/ssl/gitlab.example.com.key\"|nginx['ssl_certificate_key'] = '/etc/gitlab/ssl/localhost.key'|" /etc/gitlab/gitlab.rb

  
   # Reconfigurer GitLab pour appliquer les modifications
    sudo gitlab-ctl reconfigure
SHELL
    end