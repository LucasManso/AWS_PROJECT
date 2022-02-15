⚡ NIS - UBUNTU

SERVER:

        ◽ apt -y install nis  ###During the intalation put your domain
        ◽ nano /etc/default/nis 🔻 add ahead of "NISSERVER="🔻

        🏷️ master

        ◽ nano /etc/ypserv.securenets  ###Comment 0.0.0.0 0.0.0.0 🔻 add at the end of the file 🔻

        🏷️ 255.255.0.0	172.31.0.0      ###depending on the mask you want and your IP

        ◽ nano /var/yp/Makefile  🔻 ###Change line 52 and line 56, respectively 🔻

        🏷️ MERGE_PASSWD=true
        🏷️ MERGE_GROUP=true

        ◽ /usr/lib/yp/ypinit -m  ###Ctrl + D -> y
        ◽ nano /etc/yp.conf  🔻 add at the end of the file 🔻

        🏷️ ypserver IP_PRIV_SERVER

        ◽ systemctl restart nis
                
CLIENT:

        ◽ apt -y install nis
        ◽ nano /etc/yp.conf

        🏷️ ypserver IP_PRIV_SERVER
        🏷️ hostname domain IP_PRIV_SERVER hostname.domain
				
        ◽nano /etc/nsswitch.conf  🔻 leave as it is below 🔻

        🏷️ passwd:	compat nis
        🏷️ group:	compat nis
        🏷️ shadow:	compat nis
        🏷️ gshadow:	files
        🏷️
        🏷️ hosts:	files dns nis
        🏷️ networks:	files
        🏷️ 
        🏷️ protocols:	db files
        🏷️ services:	db files
        🏷️ ethers:	db files
        🏷️ rpc:	db files
        🏷️
        🏷️ netgroup:	nis

        ◽ nano /etc/pam.d/common-session  🔻 add at the end of the file 🔻

        🏷️ session optional	pam_mkhomedir.so skel=/etc/skel umask=077

        ◽ systemctl restart nis

        ###Check if you can sign in to users through the client
        ◽ login ...
