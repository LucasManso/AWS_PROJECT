⚡RAID-5 (RAID / LUKS / LVM)	«I used 3 disks for raid5»
  	
	◽ lsblk  ###To see the disks
	◽ gdisk /dev/xvdf
		◽ o -> y
		◽ n -> ENTER -> ENTER -> ENTER -> fd00
		◽ w -> y
	◽ gdisk /dev/xvdg
		◽ o -> y
		◽ n -> ENTER -> ENTER -> ENTER -> fd00
		◽ w -> y
	◽ gdisk /dev/xvdh
		◽ o -> y
		◽ n -> ENTER -> ENTER -> ENTER -> fd00
		◽ w -> y
 
 	◽ mdadm --create /dev/md0 --level 5 --raid-devices 3 /dev/xvdf1 /dev/xvdg1 /dev/xvdh1
	◽ mdadm --detail /dev/md0  ###Wait until "clean" from line 12 appears

	◽ cryptsetup luksFormat --hash=sha512 --key-size=512 --cipher=aes-xts-plain64 --verify-passphrase /dev/md0
		◽ YES -> (choose a passphrase)
	◽ cryptsetup luksOpen /dev/md0 md0_crypt
		◽ (chosen passphrase)   
	###To add a LUKS KEY:
	◽ cryptsetup luksAddKey /dev/md0
		◽ a passphrase you've chosen -> new passphrase
	◽ cryptsetup luksDump -v /dev/md0  ###To see the keys you have
  
  	◽ pvcreate /dev/mapper/md0_crypt
  	◽ vgcreate vg0 /dev/mapper/md0_crypt
	◽ lvcreate -n vg0lv0 -l +75%FREE vg0  ###Use the space you want by changing the percentage
	◽ lvcreate -n vg0lv1 -l +100%FREE vg0  ###Use the remaining space you want by changing the percentage
	◽ lvdisplay
		
	◽ mkfs.xfs /dev/vg0/vg0lv0  ###You can use "xfs" or "ext4"
	◽ mkfs.xfs /dev/vg0/vg0lv1  ###You can use "xfs" or "ext4"
	◽ df -hT	###Use this command to view xfs or ext4 files
		
	◽ cd /mnt/
	◽ mkdir raid5_1  ###Choose a name for a folder
	◽ mkdir raid5_2  ###Choose a name for another folder
	◽ mount /dev/vg0/vg0lv0 /mnt/raid5_1
	◽ mount /dev/vg0/vg0lv1 /mnt/raid5_2
		
	◽ cat /etc/mtab
		###Copy the two lines where you have the folders you created
		###ex.:	/dev/mapper/vg0-vg0lv0 /mnt/raid5_1 xfs nofail,rw,relatime,attr2,inode64,logbufs=8,logbsize=32k,sunit=1024,swidth=2048,noquota 0 0
			/dev/mapper/vg0-vg0lv1 /mnt/raid5_2 xfs nofail,rw,relatime,attr2,inode64,logbufs=8,logbsize=32k,sunit=1024,swidth=2048,noquota 0 0
	◽ nano /etc/fstab/
		###Paste the two lines you just copied from the "mtab" file and add "no fail" to the two lines as is in the example above
				
	###If you reboot, you have to do the following commands:
		◽ cryptsetup luksOpen /dev/md0 md0_crypt
			◽ one of the passphrase you defined
		◽ mount -a
				
⚡ RAID-6 (RAID / LVM / LUKS)	«I used 4 disks for raid6»
		
	◽ lsblk  ###To see the disks
	◽ gdisk /dev/xvdf
		◽ o -> y
		◽ n -> ENTER -> ENTER -> ENTER -> fd00
		◽ w -> y
	◽ gdisk /dev/xvdg
		◽ o -> y
		◽ n -> ENTER -> ENTER -> ENTER -> fd00
		◽ w -> y
	◽ gdisk /dev/xvdh
		◽ o -> y
		◽ n -> ENTER -> ENTER -> ENTER -> fd00
		◽ w -> y
	◽ gdisk /dev/xvdi
		◽ o -> y
		◽ n -> ENTER -> ENTER -> ENTER -> fd00
		◽ w -> y

	◽ mdadm --create /dev/md0 --level 6 --raid-devices 4 /dev/xvdf1 /dev/xvdg1 /dev/xvdh1 /dev/xvdi1
	◽ mdadm --detail /dev/md0  ####Wait until "clean" from line 12 appears

	◽ pvcreate /dev/md1
	◽ vgcreate vg1 /dev/md1
	◽ lvcreate -n vg1lv0 -l +50%FREE vg1  ###Use the space you want by changing the percentage
	◽ lvcreate -n vg1lv1 -l +100%FREE vg1  ###Use the remaining space you want by changing the percentage
	◽ lvdisplay

	◽ cryptsetup luksFormat --hash=sha512 --key-size=512 --cipher=aes-xts-plain64 --verify-passphrase /dev/vg1/vg1lv0
		◽ YES -> (choose a passphrase)
	◽ cryptsetup luksFormat --hash=sha512 --key-size=512 --cipher=aes-xts-plain64 --verify-passphrase /dev/vg1/vg1lv1
		◽ YES -> (choose another passphrase)
	◽ cryptsetup luksOpen /dev/vg1/vg1lv0 vg1lv0_crypt
		◽ the passphrase you used for this volume group
	◽ cryptsetup luksOpen /dev/vg1/vg1lv1 vg1lv1_crypt
		◽ the passphrase you used for this volume group

	◽ mkfs.xfs /dev/mapper/vg1lv0_crypt  ###You can use "xfs" or "ext4"
	◽ mkfs.xfs /dev/mapper/vg1lv1_crypt  ###You can use "xfs" or "ext4"

	◽ cd /mnt/
	◽ mkdir raid6_1
	◽ mkdir raid6_2
	◽ mount /dev/mapper/vg1lv0_crypt /mnt/raid6_1/
	◽ mount /dev/mapper/vg1lv0_crypt /mnt/raid6_2/

	◽ cat /etc/mtab
		###Copy the two lines where you have the folders you created
		###ex.:	/dev/mapper/vg1lv0_crypt /mnt/raid6_1 xfs nofail,rw,relatime,attr2,inode64,logbufs=8,logbsize=32k,sunit=1024,swidth=2048,noquota 0 0
			/dev/mapper/vg1lv1_crypt /mnt/raid6_2 xfs nofail,rw,relatime,attr2,inode64,logbufs=8,logbsize=32k,sunit=1024,swidth=2048,noquota 0 0
	◽ nano /etc/fstab/
		###Paste the two lines you just copied from the "mtab" file and add "no fail" to the two lines as is in the example above

	###If you reboot, you have to do the following commands:
		◽ cryptsetup luksOpen /dev/vg1/vg1lv0 vg1lv0_crypt
			◽ the passphrase you used for this volume group
		◽ cryptsetup luksOpen /dev/vg1/vg1lv1 vg1lv1_crypt
			◽ the passphrase you used for this volume group
		◽ mount -a
