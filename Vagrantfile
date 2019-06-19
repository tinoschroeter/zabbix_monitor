# -*- mode: ruby -*-
# vi: set ft=ruby :

$script = <<SCRIPT
apt-get update; apt-get install -y apache2 
#python-pip 
#pip install -y pyzabbix
rm /var/www/html/index.html
tee /etc/apache2/sites-enabled/000-default.conf >/dev/null <<EOF
<VirtualHost *:80>
	ServerName example.org
	ServerAdmin tino@example.org
	DocumentRoot /var/www/html/
 
        ScriptAlias "/index.html" "/usr/lib/cgi-bin/zabbix.cgi"
        ScriptAlias "/index" "/usr/lib/cgi-bin/zabbix.cgi"
        ScriptAlias "/zabbix-monitor" "/usr/lib/cgi-bin/zabbix.cgi"
        RedirectMatch 404 index.htsh

        <Directory /var/www/html/>
          AllowOverride none
          Options -Indexes
          Require all granted
        </Directory>

	ErrorLog /var/log/apache2/error.log
	CustomLog /var/log/apache2/access.log combined

	Include conf-available/serve-cgi-bin.conf
</VirtualHost>
EOF
sed -i 's/www-data/vagrant/g' /etc/apache2/envvars
a2enmod cgid
service apache2 restart

chmod +x /var/www/html/build
cd /var/www/html/build
SCRIPT

Vagrant.configure("2") do |config|
config.vm.define "zabbix" do |zabbix|
  zabbix.vm.hostname = "zabbix"
  zabbix.vm.box = "ubuntu/bionic64"
  zabbix.vm.provision "shell", inline: $script
  zabbix.vm.network "forwarded_port", guest: 80, host: 7700
  zabbix.vm.synced_folder ".", "/var/www/html"
  end
config.vm.provider "virtualbox" do |v|
  v.memory = 1024
  v.cpus = 2
  end
end
