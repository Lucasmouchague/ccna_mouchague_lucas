# TP5-Lucas_Mouchague

## Checklist IP VMs
- Définition des IPs statiques
Sur chaque VM :
* client1
* client2
* server1

`sudo nano /etc/sysconfig/network-scripts/ifcfg-enp0s3` et dans le fichier :
```
NAME=enp0s3
DEVICE=enp0s3

BOOTPROTO=static
ONBOOT=yes

IPADDR=10.5.2.10 OU 10.5.2.11 OU 10.5.1.10 (client1, client2, server1)
NETMASK=255.255.255.0
```
puis
```
sudo ifdown enp0s3
sudo ifup enp0s3
```

- La connexion SSH doit être fonctionnelle (voir tableau plus bas avec récap, adresse IP SSH pour reminder)
- une fois fait, vous avez vos trois fenêtres SSH ouvertes, une dans chaque machine
- Définition du nom de domaine (pour les trois VMs, exemple seulement pour la VM client1)
`echo 'client1.tp5.b1' | sudo tee /etc/hostname`

`sudo nano /etc/hosts` :
```
10.5.2.11 client2 client2.tp5.b1
10.5.1.10 server1 server1.tp5.b1
```
## Checklist IP Routeurs
Le réseau sur lequel les router sont est en /30 car on n'a pas besoin de plus de 2 machines sur le réseau.
- Définition des IPs statiques
router1 :
```
show ip int br
conf t
interface ethernet 0/0
ip address 10.5.1.254 255.255.255.0
no shut
exit
interface ethernet 0/3
ip address 10.5.3.1 255.255.255.252
no shut
exit
exit
```
router2 :
```
show ip int br
conf t
interface ethernet 0/0
ip address 10.5.2.254 255.255.255.0
no shut
exit
interface ethernet 0/3
ip address 10.5.3.2 255.255.255.252
no shut
exit
exit
```
- Définition du nom de domaine
router1 :
```
conf t
hostname router1.tp5.b1
exit
```
router2 :
```
conf t
hostname router2.tp5.b1
exit
```

## Checklist routes
Fichiers hosts fait en même temps que le hostname

- router1.tp5.b1
```
conf t
ip route 10.5.2.0 255.255.255.0 10.5.3.2
```
- router2.tp5.b1
```
conf t
ip route 10.5.1.0 255.255.255.0 10.5.3.2
```
- server1.tp5.b1
```
sudo nano /etc/sysconfig/network-script/route-enp0s3
(dans le fichier) 10.5.2.0/24 via 10.5.1.254 dev enp0s3
sudo systemctl restart network
```
- client1.tp5.b1
```
sudo nano /etc/sysconfig/network-script/route-enp0s3
(dans le fichier) 10.5.1.0/24 via 10.5.2.254 dev enp0s3
sudo systemctl restart network
```
- client2.tp5.b1
```
sudo nano /etc/sysconfig/network-script/route-enp0s3
(dans le fichier) 10.5.1.0/24 via 10.5.2.254 dev enp0s3
sudo systemctl restart network
```

**Depuis client1**
```
ping server1
PING server1 (10.5.1.10) 56(84) bytes of data.
64 bytes from server1 (10.5.1.10): icmp_seq=2 ttl=62 time=29.5 ms
64 bytes from server1 (10.5.1.10): icmp_seq=3 ttl=62 time=26.6 ms
```
**Depuis client2**
```
ping server1
PING server1 (10.5.1.10) 56(84) bytes of data.
64 bytes from server1 (10.5.1.10): icmp_seq=1 ttl=62 time=35.2 ms
64 bytes from server1 (10.5.1.10): icmp_seq=2 ttl=62 time=28.6 ms
```

# DHCP
client2 renommée dhcp-net2

Ajout de la carte NAT via virtual box, démarrage de la machine, `sudo yum install -y dhcp`, shutdown de la VM, rallumage dans GNS3.

Dans le fichier `sudo nano /etc/dhcp/dhcpd.conf` de dhcp-net2 :

```
# dhcpd.conf

# option definitions common to all supported networks
option domain-name "tp5.b1";

default-lease-time 600; //durée du bail par défaut
max-lease-time 7200; //durée maximum du bail

# If this DHCP server is the official DHCP server for the local
# network, the authoritative directive should be uncommented.
authoritative;

# Use this to send dhcp log messages to a different log file (you also
# have to hack syslog.conf to complete the redirection).
log-facility local7;

subnet 10.5.2.0 netmask 255.255.255.0 { //sous réseau et masque
  range 10.5.2.50 10.5.2.70; //plage d'adressage
  option domain-name "tp5.b1"; //nom du domain
  option routers 10.5.2.254; //IP du router
  option broadcast-address 10.5.2.255; //IP broadcast
}
```
puis `sudo systemctl enable dhcpd`

Sur client1 :

* IP dynamique
`sudo nano /etc/sysconfig/network-scripts/ifcfg-enp0s3`

```
NAME=enp0s3
DEVICE=enp0s3

BOOTPROTO=dhcp
ONBOOT=yes
```
`sudo ifdown enp0s3 `et` sudo ifup enp0s3`
Résultat : `ip a`
```
enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    link/ether 08:00:27:35:66:94 brd ff:ff:ff:ff:ff:ff
    inet 10.5.2.50/24 brd 10.5.2.255 scope global noprefixroute dynamic enp0s3
       valid_lft 598sec preferred_lft 598sec
    inet6 fe80::a00:27ff:fe35:6694/64 scope link
       valid_lft forever preferred_lft forever
```
* dhclient
```
sudo dhclient -v -r //release du bail
sudo dhclient -v

ip a
enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    link/ether 08:00:27:35:66:94 brd ff:ff:ff:ff:ff:ff
    inet 10.5.2.51/24 brd 10.5.2.255 scope global dynamic enp0s3
       valid_lft 598sec preferred_lft 598sec
    inet6 fe80::a00:27ff:fe35:6694/64 scope link
```

* capture [lease.pcap](lease.pcap)
On voit bien le DORA
