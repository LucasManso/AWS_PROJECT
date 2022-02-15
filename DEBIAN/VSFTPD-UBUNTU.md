⚡ VSFTPD-UBUNTU

        ◽ apt install vsftpd
        ◽ passwd ubuntu
	        ◽ choose a password
        ◽ systemctl start vsftpd
        ◽ ss -tulnp  ###Check if have port 21

        ◽ nano /etc/vsftpd.conf

        🏷️	write_enable=YES  ###Uncomment
        🏷️	change for your own certificates and the folders where they are located
        🏷️     ssl_enable=YES

        ###Add at the end of the page

        🏷️	pasv_enable=YES
        🏷️	pasv_min_port=10000
        🏷️	pasv_max_port=10100

        ###FTP METHODS:
        ###Add this at the end for "Implicit Mode"

        🏷️	allow_anon_ssl=NO
        🏷️	force_local_data_ssl=YES
        🏷️	force_local_logins_ssl=YES
        🏷️	ssl_tlsv1=YES
        🏷️	ssl_sslv2=NO
        🏷️	ssl_sslv3=NO
        🏷️	require_ssl_reuse=NO
        🏷️	ssl_ciphers=HIGH

        🏷️	implicit_ssl=YES
        🏷️	listen_port=990
        
        ###For the #Explicit Mode", removes the last two lines
        ###Save the file

        ◽ systemctl restart vsftpd
        ◽ systemctl status vsftpd

        ###Now, you have to try your FTP
