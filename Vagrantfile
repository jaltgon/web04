# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "debian/bullseye64"
  config.vm.provider "virtualbox" do |vb|
    vb.gui = false
    vb.linked_clone = true
  end

  ####################################################################
  # dns.izv.ies
  ####################################################################
  
  config.vm.define "dns" do |dns|

    dns.vm.provider "virtualbox" do |vb|
      vb.name = "http-lab-4-dns"
      vb.memory = "256"
    end

    dns.vm.hostname = "dns.izv.ies"    
    dns.vm.network :private_network, ip: "192.168.69.100", hostname: true

    dns.vm.provision "shell", inline: <<-SHELL

      # instalar paquetes
      apt-get update
      apt-get install -y bind9 bind9-utils bind9-doc

      # copiar ficheros
     sudo  cp -v /vagrant/config/dns/named /etc/default/
     sudo cp -v /vagrant/config/dns/named.conf.options /etc/bind
     sudo cp -v /vagrant/config/dns/named.conf.local /etc/bind
     sudo cp -v /vagrant/config/dns/db.192.168.1 /var/lib/bind
     sudo cp -v /vagrant/config/dns/db.izv.ies /var/lib/bind

      # reiniciar servicio
      systemctl restart bind9
      systemctl status bind9
    SHELL
    
  end

  ####################################################################
  # www.izv.ies
  ####################################################################

  config.vm.define "www" do |www|
    www.vm.provider "virtualbox" do |vb|
      vb.name = "http-lab-4-www"
      vb.memory = "256"
    end

    www.vm.hostname = "www.izv.ies"
    www.vm.network :private_network, ip: "192.168.69.101", hostname: true
    
    www.vm.provision "shell", inline: <<-SHELL
      apt-get update
      apt-get install -y apache2
      apt-get install openssl
      apt-get install ca-certificates

       cp -v /vagrant/config/www/resolv.conf /etc
      cp -v /vagrant/config/www/hosts /etc

      cp -v /vagrant/config/www/ports.conf /etc/apache2
      cp -v /vagrant/config/www/www.izv.ies.conf /etc/apache2/sites-available

      sudo mkdir /var/www/izv.ies
      cp -v /vagrant/config/www/index.html /var/www/izv.ies
   
      sudo a2dissite 000-default.conf
      sudo a2ensite www.izv.ies
      sudo a2enmod ssl

     sudo systemctl restart apache2
     sudo systemctl status apache2
    SHELL
  end
end
