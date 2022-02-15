⚡ DNS - UBUNTU

        ◽ apt install bind9

        ◽ cd /etc/bind/
        ◽ nano named.conf.options  ###Uncomment forwarders 0.0.0.0

        ◽ cp db.local db.inova.pt  ###Forward Zone
        ◽ cp db.127 db.172  ###Reverse Zone

        ◽ mv db.inova.pt /var/lib/bind/  ###Move to a directory of your choice
        ◽ mv db.172 /var/lib/bind/  ###Move to a directory of your choice

        ◽ cd /var/lib/bind/
        ◽ nano db.inova.pt  ###Set up forward zone (CTRL+/ -> replace "localhost" with server domain)
        ◽ nano db.172  ###Set up reverse zone

        ◽ cd /etc/bind/

        ◽ nano named.conf.default-zones  ###Copy the forward zone and reverse zone that is defaulted
        ◽ nano named.conf.local  ###Paste and edit as needed

        ◽ systemctl restart bind9
        ◽ systemctl status bind9

        ◽ dig SITE @IP_SERVER_DNS
