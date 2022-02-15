⚡ EMAIL_SERVER (POSTFIX/DOVECOT) - UBUNTU

        ◽ apt -y install postfix postfix-doc dovecot-imapd dovecot-pop3d sasl2-bin
	
        🏷️	Internet Site
        🏷️	inova.pt		    # Your Domain

        ◽ dpkg-reconfigure postfix

        🏷️	Internet Site
        🏷️	inova.pt  ###Your Domain
        🏷️	ubuntu
        🏷️	next...
        🏷️	NO
        🏷️	Clear all line
        🏷️	next x3...

        ◽ systemctl start saslauthd postfix dovecot

        ◽ cd /etc/dovecot/conf.d/
        ◽ nano 10-mail.conf

        🏷️	Comment "#mail_location = mbox:~/mail:INBOX=/var/mail/%u"
        🏷️	UnComment "mail_location = maildir:~/Maildir"

        ◽ nano 10-auth.conf

        🏷️	disable_plaintext_auth = no

        ###If you want to use your own certificate (CA), edit the '/etc/dovecot/conf.d/10-ssl.conf' document and the respective 'SSL' directories
        ◽ systemctl restart dovecot
        ◽ systemctl status dovecot

        ###When a user is created, it automatically creates the 'Maildir' folder

        ◽ cd /etc/skel/			
        ◽ maildirmake.dovecot Maildir
        ◽ adduser USER

        #or...

        ###If the user is already created
	
	◽ login USER
	◽ maildirmake.dovecot Maildir

        ◽ cd /etc/postfix/
        ◽ nano master.cf

        ###Uncomment first group

        🏷️ 	submission inet n       -       y       -       -       smtpd
        🏷️	-o syslog_name=postfix/submission
        🏷️	-o smtpd_tls_security_level=encrypt
        🏷️	-o smtpd_sasl_auth_enable=yes
        ###ADD	-o smtpd_client_restrictions=permit_sasl_authenticated,reject

        ###Uncomment second group

        🏷️	smtps     inet  n       -       y       -       -       smtpd
        🏷️	-o syslog_name=postfix/smtps
        🏷️	-o smtpd_tls_wrappermode=yes
        🏷️	-o smtpd_sasl_auth_enable=yes
        ###ADD	-o smtpd_client_restrictions=permit_sasl_authenticated,reject


        ◽ nano /etc/postfix/sasl/smtpd.conf

        🏷️	pwcheck_method: saslauthd
        🏷️	mech_list: PLAIN LOGIN

        ◽ postconf -e 'home_mailbox = Maildir/'  ###This command will add 'home_mailbox' automatically into the end of the 'main.cf' document

        ◽ adduser postfix sasl
        ◽ adduser dovecot sasl

        ◽ nano /etc/default/saslauthd

        🏷️	START=yes
        🏷️	OPTIONS="-c -m /var/spool/postfix/var/run/saslauthd"  ###Add to the last line of the file

        ◽ systemctl restart postfix dovecot saslauthd

⚡ EMAIL_CLIENT (ThunderBird) - UBUNTU

        ###If you don't have a user created

        ◽ adduser USER
        ◽ cp /home/ubuntu/.xsession /home/USER/.xsession
        ◽ chown USER:USER /home/USER/.xsession 

        ###Open your Email Client (ex: ThunderBird)
        ###Create a new email user based on the created user data
        ###Insert the Server IP ( Private IP Email Server )
        ###Select the type of security and mailing you want to use
        ###Send an email to your self...
        ###Create another email user account and try to send to each other.
