β‘ NIS - UBUNTU

SERVER:

        β½ apt -y install nis  ###During the intalation put your domain
        β½ nano /etc/default/nis π» add ahead of "NISSERVER="π»

        π·οΈ master

        β½ nano /etc/ypserv.securenets  ###Comment 0.0.0.0 0.0.0.0 π» add at the end of the file π»

        π·οΈ 255.255.0.0	172.31.0.0      ###depending on the mask you want and your IP

        β½ nano /var/yp/Makefile  π» ###Change line 52 and line 56, respectively π»

        π·οΈ MERGE_PASSWD=true
        π·οΈ MERGE_GROUP=true

        β½ /usr/lib/yp/ypinit -m  ###Ctrl + D -> y
        β½ nano /etc/yp.conf  π» add at the end of the file π»

        π·οΈ ypserver IP_PRIV_SERVER

        β½ systemctl restart nis
                
CLIENT:

        β½ apt -y install nis
        β½ nano /etc/yp.conf

        π·οΈ ypserver IP_PRIV_SERVER
        π·οΈ hostname domain IP_PRIV_SERVER hostname.domain
				
        β½nano /etc/nsswitch.conf  π» leave as it is below π»

        π·οΈ passwd:	compat nis
        π·οΈ group:	compat nis
        π·οΈ shadow:	compat nis
        π·οΈ gshadow:	files
        π·οΈ
        π·οΈ hosts:	files dns nis
        π·οΈ networks:	files
        π·οΈ 
        π·οΈ protocols:	db files
        π·οΈ services:	db files
        π·οΈ ethers:	db files
        π·οΈ rpc:	db files
        π·οΈ
        π·οΈ netgroup:	nis

        β½ nano /etc/pam.d/common-session  π» add at the end of the file π»

        π·οΈ session optional	pam_mkhomedir.so skel=/etc/skel umask=077

        β½ systemctl restart nis

        ###Check if you can sign in to users through the client
        β½ login ...
