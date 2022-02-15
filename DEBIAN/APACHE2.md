⚡ APACHE2 - UBUNTU
        
Apache2 - Installation and configuration

        ◽ apt install apache2
        ◽ a2enmod ssl
        ◽ a2ensite default-ssl.conf
        ◽ systemctl restart apache2
        ◽ nano /etc/apache2/sites-available/default-ssl.conf
	        ◽ change DocumentRoot > htmls
	        ◽ change the names of the certificates and put the folder where you put them  ###You need to create the certificates through easy-rsa
        ◽ cd /var/www/
        ◽ cp -r html/ htmls
        ◽ cd html/
        ◽ > index.html
        ◽ nano index.html (you are changing the :80)
        ◽ cd htmls/
        ◽ > index.html
        ◽ nano index.html (you are changing the :443)
        ◽ systemctl restart apache2

Verification Apache2

        In your browser:
                - search http://ELASTIC IP
                - search https://ELASTIC IP
