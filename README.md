# M300_LB1 - DHCP Server

***Voraussetzungen***

Diese Vagrant Konfiguration wurde mit folgender lokaler Konfiguration getestet:
* Vagrant
* VirtualBox 5.2.6
* GitBash


***Virtual Box - VM Konfiguration***

Als Betriebssystem für den DHCP Server wird Debian verwendet. In den ersten Zeilen des Vagrantfiles
können einige Einstellungen vorgenommen werden:
```
Vagrant.configure(2) do |config|  
  config.vm.define "dhcp" do |dhcp|	
    dhcp.vm.box = "debian/jessie64" 	
    dhcp.vm.hostname = "dhcp"		
    dhcp.vm.network "private_network", ip:"192.168.50.4" 	
	dhcp.vm.provider "virtualbox" do |vb|			
vb.memory = "2048"
```

Damit der DHCP Server später funktioniert muss zuerst das Paketverzeichnis aktualisiert werden.
Danach wird der DHCP Server installiert mit dem Paket "ISC-DHCP-SERVER".
```
sudo apt-get update
sudo apt-get -y install isc-dhcp-server
```


***DHCP Server Konfiguration***
Als nächstes wird das DHCP Server Configfile angepasst, dabei werden folgende Attribute verändertd:
*DNS
```
sudo sed -i 's/example.org/labor.local/g' /etc/dhcp/dhcpd.conf
```
*Domainname
```
sudo sed -i 's/ns2.labor.local/8.8.8.8/g' /etc/dhcp/dhcpd.conf
```
*DHCP Scope
```
sudo sed -i 's/example.org/labor.local/g' /etc/dhcp/dhcpd.conf
sudo sed -i 's/ns2.labor.local/8.8.8.8/g' /etc/dhcp/dhcpd.conf
sudo sed -i 's/#authoritative/authoritative/g' /etc/dhcp/dhcpd.conf
sudo sed -i '$asubnet 192.168.50.0 netmask 255.255.255.0 {' /etc/dhcp/dhcpd.conf
sudo sed -i '$arange 192.168.50.50 192.168.50.80;' /etc/dhcp/dhcpd.conf
sudo sed -i '$aoption routers 192.168.50.1;' /etc/dhcp/dhcpd.conf
sudo sed -i '$a}' /etc/dhcp/dhcpd.conf
```

Nach der Konfiguration wird der DHCP Service neu gestartet.
```
sudo service isc-dhcp-server restart
```
In einem letzten Schritt wird das Tastaturlayout auf Deutsch gestellt.
```
sudo sed -i 's/XKBLAYOUT="us"/XKBLAYOUT="ch"/g' /etc/default/locale
```
