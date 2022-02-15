⚡ NFS - UBUNTU

SERVER:

        ◽ apt install nfs-kernel-server
        ◽ nano /etc/exports 🔻 add at the end of the file 🔻

        🏷️ /var/homes *(rw,sync,no_subtree_check,no_root_squash)

        ◽ mkdir /var/homes	
        ◽ exportfs -a
        ◽ systemctl enable --now nfs-kernel-server
        ◽ adduser user --home /var/homes/user
        ◽ echo xfce4-session > /var/homes/user/.xsession
        ◽ chown user:user /var/homes/user/.xsession
        
        ###Always you create a user, do:
        ◽ cd /var/yp
        ◽ make
        
CLIENT:

        ◽ apt install nfs-common
        ◽ mkdir /var/homes
        ◽ mount -t nfs IP_PRIV_SRV:/var/homes /var/homes/
        ◽ cat /etc/mtab 
        ###Copy the line where you have the PRIVATE IP to /etc/fstab and add {,nofail} in any "between commas" of that line
