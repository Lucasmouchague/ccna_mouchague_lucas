# TP4-Lucas_mouchague
## checklist:
### Définition des IPs statiques:
#### Client1.tp4:
```
NAME=enp0s3
DEVICE=enp0s3

BOOTPROTO=static
ONBOOT=yes

IPADDR=10.1.0.10
NETMASK=255.255.255.0
```
#### Server1.tp4:
```
NAME=enp0s3
DEVICE=enp0s3

BOOTPROTO=static
ONBOOT=yes

IPADDR=10.2.0.10
NETMASK=255.255.255.0
```
#### Router1.tp4:
```
BOOTPROTO=static
NAME=enp0s3
DEVICE=enp0s3
ONBOOT=yes
IPADDR=10.1.0.254
NETMASK=255.255.255.0
```
### Remplissage du fichier /etc/hosts:
#### Client1.tp4:
```
127.0.0.1   localhost localhost.localdomain localhost4 localhost4.localdomain4
::1         localhost localhost.localdomain localhost6 localhost6.localdomain6
10.1.0.10   client1 client1.tp4
10.1.0.254  router1 router1.tp4
10.2.0.10   server1 server1.tp4
```
#### Server1.tp4:
```
127.0.0.1   localhost localhost.localdomain localhost4 localhost4.localdomain4
::1         localhost localhost.localdomain localhost6 localhost6.localdomain6
10.2.0.10   server1 server1.tp4
10.2.0.254  router1 router1.tp4
10.1.0.10   client1 client1.tp4
```
#### Router1.tp4
```
127.0.0.1   localhost localhost.localdomain localhost4 localhost4.localdomain4
::1         localhost localhost.localdomain localhost6 localhost6.localdomain6
10.1.0.254  router1 router1.tp4
10.2.0.254  router1 router1.tp4
10.1.0.10   client1 client1.tp4
10.2.0.10   server1 server1.tp4
```
### Ping
#### Ping client1 --> router1.tp4
```
[nawak@client1 ~]$ ping router1.tp4
PING router1 (10.1.0.254) 56(84) bytes of data.
64 bytes from router1 (10.1.0.254): icmp_seq=1 ttl=64 time=0.438 ms
64 bytes from router1 (10.1.0.254): icmp_seq=2 ttl=64 time=0.403 ms
^C
--- router1 ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1001ms
rtt min/avg/max/mdev = 0.403/0.420/0.438/0.026 ms
```
#### Ping server1 --> router1.tp4
```
[nawak@server1 ~]$ ping router1.tp4
PING router1 (10.2.0.254) 56(84) bytes of data.
64 bytes from router1 (10.2.0.254): icmp_seq=1 ttl=64 time=0.228 ms
64 bytes from router1 (10.2.0.254): icmp_seq=2 ttl=64 time=0.350 ms
^X64 bytes from router1 (10.2.0.254): icmp_seq=3 ttl=64 time=0.358 ms
^C
--- router1 ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 1999ms
rtt min/avg/max/mdev = 0.228/0.312/0.358/0.059 ms
```
## Mise en place du routage statique
### Client1:
```
[nawak@client1 ~]$ ip route show
10.1.0.0/24 dev enp0s3 proto kernel scope link src 10.1.0.10 metric 100
10.2.0.0/24 via 10.1.0.254 dev enp0s3 proto static metric 100
```
### Server1:
```
[nawak@server1 ~]$ ip route show
10.1.0.0/24 via 10.2.0.254 dev enp0s3 proto static metric 100
10.2.0.0/24 dev enp0s3 proto kernel scope link src 10.2.0.10 metric 100
```
### Ping client1 --> server1:
```
[nawak@client1 ~]$ ping server1.tp4
PING server1 (10.2.0.10) 56(84) bytes of data.
64 bytes from server1 (10.2.0.10): icmp_seq=1 ttl=63 time=0.835 ms
64 bytes from server1 (10.2.0.10): icmp_seq=2 ttl=63 time=0.748 ms
^C
--- server1 ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1000ms
rtt min/avg/max/mdev = 0.748/0.791/0.835/0.051 ms
```
### Ping server1 --> client1:
```
[nawak@server1 ~]$ ping client1.tp4
PING client1 (10.1.0.10) 56(84) bytes of data.
64 bytes from client1 (10.1.0.10): icmp_seq=1 ttl=63 time=0.568 ms
64 bytes from client1 (10.1.0.10): icmp_seq=2 ttl=63 time=0.702 ms
^C
--- client1 ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1000ms
rtt min/avg/max/mdev = 0.568/0.635/0.702/0.067 ms
```
# II. Spéléologie réseau
## ARP:
### A. Manip 1
#### 2. Client1
```
[nawak@client1 ~]$ ip neigh show
10.1.0.1 dev enp0s3 lladdr 0a:00:27:00:00:0e REACHABLE
```
On vois que y'a eu une communication avec 10.1.0.1 via enp0s3
#### 3. Server1
```
[nawak@server1 ~]$ ip neigh show
10.2.0.1 dev enp0s3 lladdr 0a:00:27:00:00:17 DELAY
```
On vois que y'a eu une communication avec l'adresse IP via l'interface enp0s3 mais que la table va être bientôt mise a jour.

#### 4. Client1
```
[nawak@client1 ~]$ ping server1.tp4
PING server1 (10.2.0.10) 56(84) bytes of data.
64 bytes from server1 (10.2.0.10): icmp_seq=1 ttl=63 time=1.11 ms
64 bytes from server1 (10.2.0.10): icmp_seq=2 ttl=63 time=0.661 ms
^C
--- server1 ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1001ms
rtt min/avg/max/mdev = 0.661/0.886/1.112/0.227 ms
[nawak@client1 ~]$ ip neigh show
10.1.0.1 dev enp0s3 lladdr 0a:00:27:00:00:0e REACHABLE
10.1.0.254 dev enp0s3 lladdr 08:00:27:bb:9a:30 REACHABLE
```
Une seconde adresse est visible l'adresse de gateway qui a servi a ping le server dans l'autre réseau
#### 5. Server1
```
[nawak@server1 ~]$ ping client1.tp4
PING client1 (10.1.0.10) 56(84) bytes of data.
64 bytes from client1 (10.1.0.10): icmp_seq=1 ttl=63 time=0.828 ms
64 bytes from client1 (10.1.0.10): icmp_seq=2 ttl=63 time=0.656 ms
^C
--- client1 ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1001ms
rtt min/avg/max/mdev = 0.656/0.742/0.828/0.086 ms
[nawak@server1 ~]$ ip neigh show
10.2.0.1 dev enp0s3 lladdr 0a:00:27:00:00:17 REACHABLE
10.2.0.254 dev enp0s3 lladdr 08:00:27:c7:a9:68 REACHABLE
```
Une seconde adresse est visible l'adresse de gateway qui a servi a ping le client dans l'autre réseau
### B. Manip 2
#### 2. Router1 
```
[nawak@router ~]$ ip neigh show
10.1.0.1 dev enp0s3 lladdr 0a:00:27:00:00:0e REACHABLE
```
Il a enregistrer dans sa table ARP une comunication dec 10.1.0.1 via dev enp0s3
#### 4. router1
```
[nawak@router ~]$ ip neigh show
10.1.0.10 dev enp0s3 lladdr 08:00:27:d3:57:10 REACHABLE
10.2.0.10 dev enp0s8 lladdr 08:00:27:8c:a8:a9 REACHABLE
10.1.0.1 dev enp0s3 lladdr 0a:00:27:00:00:0e DELAY
```
Le router a enregistrer dans sa table ARP l'adresse ip du server et du client qui ont communiquer par lui
### C. Manip 3
#### Un peu
```
C:\Windows\system32>arp -a

Interface : 169.254.118.29 --- 0xa
  Adresse Internet      Adresse physique      Type
  224.0.0.22            01-00-5e-00-00-16     statique

Interface : 10.1.0.1 --- 0xe
  Adresse Internet      Adresse physique      Type
  224.0.0.22            01-00-5e-00-00-16     statique

Interface : 10.33.0.54 --- 0xf
  Adresse Internet      Adresse physique      Type
  10.33.3.90            ac-b5-7d-66-3e-f5     dynamique
  10.33.3.253           00-12-00-40-4c-bf     dynamique
  224.0.0.22            01-00-5e-00-00-16     statique

Interface : 192.168.145.1 --- 0x15
  Adresse Internet      Adresse physique      Type
  224.0.0.22            01-00-5e-00-00-16     statique

Interface : 10.2.0.1 --- 0x17
  Adresse Internet      Adresse physique      Type
  224.0.0.22            01-00-5e-00-00-16     statique

Interface : 192.168.207.1 --- 0x18
  Adresse Internet      Adresse physique      Type
  224.0.0.22            01-00-5e-00-00-16     statique
```
#### Encore un peu
```
C:\Windows\system32>arp -a

Interface : 169.254.118.29 --- 0xa
  Adresse Internet      Adresse physique      Type
  224.0.0.22            01-00-5e-00-00-16     statique

Interface : 10.1.0.1 --- 0xe
  Adresse Internet      Adresse physique      Type
  224.0.0.22            01-00-5e-00-00-16     statique

Interface : 10.33.0.54 --- 0xf
  Adresse Internet      Adresse physique      Type
  10.33.0.4             b4-86-55-31-f1-a9     dynamique
  10.33.1.140           f0-18-98-5c-0b-78     dynamique
  10.33.3.90            ac-b5-7d-66-3e-f5     dynamique
  10.33.3.253           00-12-00-40-4c-bf     dynamique
  224.0.0.22            01-00-5e-00-00-16     statique

Interface : 192.168.145.1 --- 0x15
  Adresse Internet      Adresse physique      Type
  224.0.0.22            01-00-5e-00-00-16     statique

Interface : 10.2.0.1 --- 0x17
  Adresse Internet      Adresse physique      Type
  224.0.0.22            01-00-5e-00-00-16     statique

Interface : 192.168.207.1 --- 0x18
  Adresse Internet      Adresse physique      Type
  224.0.0.22            01-00-5e-00-00-16     statique
```
Pas de changement en rapport avec la gateway. Déso pas déso. Signé Windaube.
### D. Manip 4
Après flush:
```
[nawak@client1 ~]$ ip n s
10.1.0.1 dev enp0s3 lladdr 0a:00:27:00:00:0e REACHABLE
```
Après curl:
```
[nawak@client1 ~]$ ip n s
10.0.3.2 dev enp0s8 lladdr 52:54:00:12:35:02 REACHABLE
10.1.0.1 dev enp0s3 lladdr 0a:00:27:00:00:0e REACHABLE
```
Le réseau NAT est présent qui nous a permis de récuperer la page www.google.com.
# 2. Wireshark
## A. Interception d'ARP et ping
(https://github.com/Lucasmouchague/ccna_mouchague_lucas/blob/master/TP4-lucas_mouchague/ping.pcap)
On vois au niveau de la requete 2 et 3 que on demande au broadcast qui est le destinataire et au niveau de la requete 4 on nous renvoie l'adresse mac correspondante a notre cible.

## B. Interception d'une communication netcat
(https://github.com/Lucasmouchague/ccna_mouchague_lucas/blob/master/TP4-lucas_mouchague/nc.pcap)
La première requete envoi un paquet de synchronisation(SYN) et la cible répond par un paquet de synchronisation(SYN) et d'acquittement(ACK) pour synchroniser la communication via netcat.

## HEY j'ai une idée:
(https://github.com/Lucasmouchague/ccna_mouchague_lucas/blob/master/TP4-lucas_mouchague/nc_ko.pcap)
La premiere requete demande une synchronisation(SYN) mais au niveau de la seconde requete nous renvois que la cible n'est pas accessible donc on recommence la demande de synchronisation.

## C. Inteception d'un trafic HTTP (bonus)
### Install et config du serveur Web
#### Sur serveur1
```
[nawak@server1 ~]$ curl localhost:80
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.1//EN" "http://www.w3.org/TR/xhtml11/DTD/xhtml11.dtd">

<html xmlns="http://www.w3.org/1999/xhtml" xml:lang="en">
    <head>
        <title>Test Page for the Nginx HTTP Server on Fedora</title>
        <meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />
        <style type="text/css">
            /*<![CDATA[*/

            etc etc...
```
#### Sur router1
```
[nawak@server1 ~]$ curl localhost:80
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.1//EN" "http://www.w3.org/TR/xhtml11/DTD/xhtml11.dtd">

<html xmlns="http://www.w3.org/1999/xhtml" xml:lang="en">
    <head>
        <title>Test Page for the Nginx HTTP Server on Fedora</title>
        <meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />
        <style type="text/css">
            /*<![CDATA[*/

            etc etc...
```
#### Sur PC
Page d'acceuil par défaut de NGINX

### Interception du trafic
(https://github.com/Lucasmouchague/ccna_mouchague_lucas/blob/master/TP4-lucas_mouchague/http.pcap)
