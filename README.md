# cluster-debian-heartbeat

https://www.bfnetworks.com.br/configurando-heartbeat/

https://pt.slideshare.net/fred_m/alta-disponibilidade-em-linux-com-heartbeat-e-drbd

https://www.youtube.com/watch?v=9RIsZu0KKfY


nano /etc/network/interfaces

iface enp0s3 inet static
address 192.168.1.101
//15.12 example
netmask 255.255.255.0
gateway 192.168.1.1
//15.1
iface enp0s3 inet static
address 192.168.1.102
netmask 255.255.255.0
gateway 192.168.1.1


iface enp0s3 inet static
address 192.168.1.103
netmask 255.255.255.0
gateway 192.168.1.1

nano /etc/hosts/

apt-get install heatbeat apache2

nano /etc/ha.d/ha.cf 

# primeiro node
node master
# segundo node      
node slave1
# terceiro node
node slave2  
# interface vai ser usada para comunicação   
udp enp0s3 
# ativando log    (recomendado, opcional)         
use_logd on   
# freqüência, em segundos, da verificação das máquinas      
keepalive 1
# tempo mín para declarar a outra máquina como morta
deadtime 5 
# caso master volte, ele assume novamente        
auto_failback on  

nano /etc/ha.d/haresources

master IPaddr::192.168.1.100/24/enp0s3 apache2

nano /etc/ha.d/authkeys

auth 3
3 md5 123

chmod 600 /etc/ha.d/authkeys

systemctl start heartbeat

cl_status hbstatus

cl_status nodestatus slave1
cl_status nodestatus slave2


logfile /var/log/ha-log

logfacility local0

keepalive 5

deadtime 30

initdead 120

warntime 5

udpport 694

bcast enp0s3


trocar nas VMs: 
address/gateway arquivo /etc/network/interfaces
arquivo hosts (endereço master)
/etc/ha.d/haresources  (endereço master)




systemctl start heartbeat
systemctl status heartbeat
cl_status hbstatus
cl_status listnodes

