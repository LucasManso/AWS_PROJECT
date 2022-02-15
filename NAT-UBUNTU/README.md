⚡NAT - UBUNTU

SERVER:
	
	◽ ip a	###Copy the macadress from the created network interface (this if another network interface was created)
	◽ nano /etc/netplan/50-cloud-init.yaml	###Create one more network block 'eth2' and paste the mac address
	◽ netplan try
	◽ netplan apply
	◽ apt update && apt upgrade
	◽ apt install netfilter-persistent iptables-persistent
	◽ iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE
	◽ netfilter-persistent save
	◽ nano /etc/sysctl.conf  ###uncomment "net.ipv4.ip_forward = 1"
	◽ nano chave.pem  ###Paste clientes key
	◽ chmod 400 chave.pem

	#To enter the client do the following command on the server:
	◽ ssh -i chave.pem ubuntu@IP_PRIV_CLIENTE
	
CLIENT:
	
	◽ nano /etc/netplan/50-cloud-init.yaml
	🔻 copy and paste into the file between "dhcp6" and "match" 🔻

	🏷️ dhcp4-overrides:
	🏷️         use-dns: false
	🏷️          use-routes: false
	🏷️ nameservers:
	🏷️          addresses: [ 8.8.8.8 ]     ###Once you have DNS we can change to the ip of the server
	🏷️ routes:
	🏷️        - to: 0.0.0.0/0
	🏷️          via: IP_PRIV_SERVIDOR (server to which we want to connect to get network)
	🏷️          on-link: true

	◽ netplan try
	◽ netplan apply
	◽ ping 1.1.1.1
	◽ apt update && apt upgrade  ###On all clients for later installations
