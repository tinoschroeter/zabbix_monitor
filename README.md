## zabbix_monitor

Zabbix alert Monitor

## using Webbrowser

![web](https://github.com/tinoschroeter/zabbix_monitor/blob/master/web.png)

## using cli

![cli](https://github.com/tinoschroeter/zabbix_monitor/blob/master/cli.png)


## Install Apache2
```shell
pip install pyzabbix
apt-get update; apt-get install -y apache2
tee /etc/apache2/sites-enabled/000-default.conf >/dev/null <<EOF
<VirtualHost *:80>
    ServerName example.org
    ServerAdmin webmaster@example.org
    DocumentRoot /var/www/html/

        ScriptAlias "/index.html" "/usr/lib/cgi-bin/zabbix.cgi"
        ScriptAlias "/index" "/usr/lib/cgi-bin/zabbix.cgi"
        ScriptAlias "/zabbix-monitor" "/usr/lib/cgi-bin/zabbix.cgi"
        RedirectMatch 404 .*\.htsh
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
a2enmod cgid
service apache2 restart
```
## Vagrant
```shell
git clone https://github.com/tinoschroeter/zabbix_monitor.git
cd zabbix_monitor

vagrant up

open http://localhost:7700/
