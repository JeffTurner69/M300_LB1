# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. / Setup VM
Vagrant.configure(2) do |config|
  config.vm.define "dhcp" do |dhcp|
    dhcp.vm.box = "debian/jessie64"
    dhcp.vm.hostname = "dhcp"
    dhcp.vm.network "private_network", ip:"192.168.50.4"
        dhcp.vm.provider "virtualbox" do |vb|
          vb.memory = "2048"
        end
        dhcp.vm.provision "shell", inline: <<-SHELL
                sudo apt-get update
        # DHCP Server installieren
        sudo apt-get -y install isc-dhcp-server
        # DHCP Server konfigurieren
        sudo sed -i 's/example.org/labor.local/g' /etc/dhcp/dhcpd.conf
        sudo sed -i 's/ns2.labor.local/8.8.8.8/g' /etc/dhcp/dhcpd.conf
        sudo sed -i 's/#authoritative/authoritative/g' /etc/dhcp/dhcpd.conf
        sudo sed -i '$asubnet 192.168.50.0 netmask 255.255.255.0 {' /etc/dhcp/dhcpd.conf
        sudo sed -i '$arange 192.168.50.50 192.168.50.80;' /etc/dhcp/dhcpd.conf
        sudo sed -i '$aoption routers 192.168.50.1;' /etc/dhcp/dhcpd.conf
        sudo sed -i '$a}' /etc/dhcp/dhcpd.conf
        # Firewall installieren und aktivieren
        sudo service isc-dhcp-server restart
        sudo apt-get install ufw -y
        sudo ufw allow from 10.0.2.2 to any port 22
        sudo ufw --force enable
        # Eingabemethode⁄ ändern
        sudo sed -i 's/XKBLAYOUT="us"/XKBLAYOUT="ch"/g' /etc/default/locale
SHELL
        end
 end
