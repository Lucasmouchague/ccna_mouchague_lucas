# TP3-Lucas_mouchague
# Creation et utilisation simple d'une VM CentOS
## 4 Configuration réseau d'une machine CentOS
### a
``` 
[nawak@localhost ~]$ wget www.google.com
--2019-01-08 16:22:54--  http://www.google.com/
Resolving www.google.com (www.google.com)... 216.58.213.132, 2a00:1450:4007:815::2004
Connecting to www.google.com (www.google.com)|216.58.213.132|:80... connected.
HTTP request sent, awaiting response... 200 OK
Length: unspecified [text/html]
Saving to: ‘index.html’
    [ <=>                                                                        ] 11,333      --.-K/s   in 0.005s
2019-01-08 16:22:55 (2.05 MB/s) - ‘index.html’ saved [11333] 
```

### b
``` C:\Users\lukea>ping 192.168.127.10

Envoi d’une requête 'Ping'  192.168.127.10 avec 32 octets de données :
Réponse de 192.168.127.10 : octets=32 temps<1ms TTL=64
Réponse de 192.168.127.10 : octets=32 temps<1ms TTL=64
Réponse de 192.168.127.10 : octets=32 temps<1ms TTL=64
Réponse de 192.168.127.10 : octets=32 temps<1ms TTL=64

Statistiques Ping pour 192.168.127.10:
    Paquets : envoyés = 4, reçus = 4, perdus = 0 (perte 0%),
Durée approximative des boucles en millisecondes :
    Minimum = 0ms, Maximum = 0ms, Moyenne = 0ms
```

### c
```
[nawak@localhost ~]$ ip route
default via 10.0.2.2 dev enp0s3 proto dhcp metric 100
10.0.2.0/24 dev enp0s3 proto kernel scope link src 10.0.2.15 metric 100
192.168.127.0/24 dev enp0s8 proto kernel scope link src 192.168.127.10 metric 101
```
Ligne 1: Connexion au réseau avec une IP publique via ``` 10.0.0.2 ``` et la carte enp0s3 par DHCP

Ligne 2: La carte enp0s3 se connecte au réseau ```10.0.2.0/24``` avec l'adresse IP ```10.0.2.15```

Ligne 3: La carte enp0s8 se connect au réseau ```192.168.127.0/24``` avec l'adresse IP ```192.168.127.10```
## 5 Faire joujou avec quelques commandes
### ping hôte -> VM
```C:\Users\lukea>ping 192.168.127.10

Envoi d’une requête 'Ping'  192.168.127.10 avec 32 octets de données :
Réponse de 192.168.127.10 : octets=32 temps<1ms TTL=64
Réponse de 192.168.127.10 : octets=32 temps<1ms TTL=64
Réponse de 192.168.127.10 : octets=32 temps<1ms TTL=64
Réponse de 192.168.127.10 : octets=32 temps<1ms TTL=64

Statistiques Ping pour 192.168.127.10:
    Paquets : envoyés = 4, reçus = 4, perdus = 0 (perte 0%),
Durée approximative des boucles en millisecondes :
    Minimum = 0ms, Maximum = 0ms, Moyenne = 0ms
```
### ping VM -> hôte
```
[nawak@localhost ~]$ ping 192.168.127.1
PING 192.168.127.1 (192.168.127.1) 56(84) bytes of data.
64 bytes from 192.168.127.1: icmp_seq=1 ttl=128 time=0.341 ms
64 bytes from 192.168.127.1: icmp_seq=2 ttl=128 time=0.325 ms
64 bytes from 192.168.127.1: icmp_seq=3 ttl=128 time=0.296 ms
64 bytes from 192.168.127.1: icmp_seq=4 ttl=128 time=0.307 ms
^C
--- 192.168.127.1 ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 3000ms
rtt min/avg/max/mdev = 0.296/0.317/0.341/0.021 ms
```
### Table de routage hôte
```
IPv4 Table de routage
===========================================================================
Itinéraires actifs :
Destination réseau    Masque réseau  Adr. passerelle   Adr. interface Métrique
          0.0.0.0          0.0.0.0      10.33.3.253       10.33.2.88     35
        10.33.0.0    255.255.252.0         On-link        10.33.2.88    291
       10.33.2.88  255.255.255.255         On-link        10.33.2.88    291
      10.33.3.255  255.255.255.255         On-link        10.33.2.88    291
        127.0.0.0        255.0.0.0         On-link         127.0.0.1    331
        127.0.0.1  255.255.255.255         On-link         127.0.0.1    331
  127.255.255.255  255.255.255.255         On-link         127.0.0.1    331
      169.254.0.0      255.255.0.0         On-link    169.254.118.29    281
   169.254.118.29  255.255.255.255         On-link    169.254.118.29    281
  169.254.255.255  255.255.255.255         On-link    169.254.118.29    281
    192.168.127.0    255.255.255.0         On-link     192.168.127.1    330
    192.168.127.1  255.255.255.255         On-link     192.168.127.1    330
  192.168.127.255  255.255.255.255         On-link     192.168.127.1    330
        224.0.0.0        240.0.0.0         On-link         127.0.0.1    331
        224.0.0.0        240.0.0.0         On-link        10.33.2.88    291
        224.0.0.0        240.0.0.0         On-link     192.168.127.1    330
        224.0.0.0        240.0.0.0         On-link    169.254.118.29    281
  255.255.255.255  255.255.255.255         On-link         127.0.0.1    331
  255.255.255.255  255.255.255.255         On-link        10.33.2.88    291
  255.255.255.255  255.255.255.255         On-link     192.168.127.1    330
  255.255.255.255  255.255.255.255         On-link    169.254.118.29    281
===========================================================================
```
Les lignes suivante permettent de discuter.
```    
192.168.127.0    255.255.255.0         On-link     192.168.127.1    330
192.168.127.1  255.255.255.255         On-link     192.168.127.1    330
```
### Table de routage VM
```
default via 10.0.2.2 dev enp0s3 proto dhcp metric 100
10.0.2.0/24 dev enp0s3 proto kernel scope link src 10.0.2.15 metric 100
192.168.127.0/24 dev enp0s8 proto kernel scope link src 192.168.127.10 metric 101
```
La ligne suivante permet de discuter.
```
192.168.127.0/24 dev enp0s8 proto kernel scope link src 192.168.127.10 metric 101
```
### Telecharger un fichier depuis internet
```
[nawak@localhost ~]$ wget www.google.com
--2019-01-08 16:22:54--  http://www.google.com/
Resolving www.google.com (www.google.com)... 216.58.213.132, 2a00:1450:4007:815::2004
Connecting to www.google.com (www.google.com)|216.58.213.132|:80... connected.
HTTP request sent, awaiting response... 200 OK
Length: unspecified [text/html]
Saving to: ‘index.html’
    [ <=>                                                                        ] 11,333      --.-K/s   in 0.005s
2019-01-08 16:22:55 (2.05 MB/s) - ‘index.html’ saved [11333] 
```
### dig ynov.com
```
;; ANSWER SECTION:
ynov.com.               2814    IN      A       217.70.184.38
```
L'IP de ynov.com est ```217.70.184.38```
### dig google.com
```
;; ANSWER SECTION:
google.com.             55      IN      A       216.58.213.142
```
L'IP de google.com est ```216.58.213.142```
# Notion de ports et SSH
## 1. Exploration des ports locaux
```
[nawak@localhost ~]$ sudo ss -l -t -n -p -4
State       Recv-Q Send-Q             Local Address:Port                            Peer Address:Port
LISTEN      0      128                            *:22                                         *:*                   users:(("sshd",pid=3139,fd=3))
LISTEN      0      100                    127.0.0.1:25                                         *:*                   users:(("master",pid=3229,fd=13))
```
On vois sur le port 22 une application "sshd" qui écoute
## 2. SSH
```
C:\Users\lukea>ssh nawak@192.168.127.10
nawak@192.168.127.10's password:
Last login: Tue Jan  8 16:10:29 2019 from 192.168.127.1
[nawak@localhost ~]$
```
## 3. Firewall
### A
Après modification de mon port ssh je ne peux pas me connecter car le firewall bloque le port ```2222``` donc la solution est d'autoriser le port 2222 sur le firewall. 

### B
#### VM
```[nawak@localhost ~]$ nc -l 5454```

#### PC
``` nawak@DESKTOP-L3EGT19:~$ nc 192.168.127.10 5454 ```

#### VM
```
[nawak@localhost ~]$ sudo ss -p -n -4
Netid  State      Recv-Q Send-Q            Local Address:Port                           Peer Address:Port
tcp    ESTAB      0      0                192.168.127.10:2222                          192.168.127.1:29962               users:(("sshd",pid=3580,fd=3),("sshd",pid=3576,fd=3))
tcp    ESTAB      0      0                192.168.127.10:5454                          192.168.127.1:30027               users:(("nc",pid=3770,fd=5))
tcp    ESTAB      0      36               192.168.127.10:2222                          192.168.127.1:30030               users:(("sshd",pid=3775,fd=3),("sshd",pid=3771,fd=3))
```
# III Routage Statique
## 1.Préparation des hôtes
### Préparation virtualbox (PC1)
Réseau host-only --> 192.168.101.0/24

VM1 --> 192.168.101.10

### Check
#### PC1 --> PC2
```
PS C:\Users\lukea> ping 192.168.112.2

Envoi d’une requête 'Ping'  192.168.112.2 avec 32 octets de données :
Réponse de 192.168.112.2 : octets=32 temps=7 ms TTL=128
Réponse de 192.168.112.2 : octets=32 temps=4 ms TTL=128
Réponse de 192.168.112.2 : octets=32 temps=4 ms TTL=128
Réponse de 192.168.112.2 : octets=32 temps=4 ms TTL=128

Statistiques Ping pour 192.168.112.2:
    Paquets : envoyés = 4, reçus = 4, perdus = 0 (perte 0%),
Durée approximative des boucles en millisecondes :
    Minimum = 4ms, Maximum = 7ms, Moyenne = 4ms
```
#### VM1 --> PC1
```
[nawak@localhost ~]$ ping 192.168.101.1
PING 192.168.101.1 (192.168.101.1) 56(84) bytes of data.
64 bytes from 192.168.101.1: icmp_seq=1 ttl=128 time=0.284 ms
64 bytes from 192.168.101.1: icmp_seq=2 ttl=128 time=0.308 ms
64 bytes from 192.168.101.1: icmp_seq=3 ttl=128 time=0.300 ms
64 bytes from 192.168.101.1: icmp_seq=4 ttl=128 time=0.295 ms
^C
--- 192.168.101.1 ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 3000ms
rtt min/avg/max/mdev = 0.284/0.296/0.308/0.022 ms
```
## 2. Configuration du routage
### Bilan des IP
PC1 (ethernet) --> 192.168.112.1
PC2 (ethernet) --> 192.168.112.2
VM1 --> 192.168.101.10
VM2 --> 192.168.102.10

### Configuration des routes (PC1)
```
route add 192.168.102.0/24 mask 255.255.255.0 192.168.112.2
```

```
PS C:\Windows\system32> ping 192.168.102.1

Envoi d’une requête 'Ping'  192.168.102.1 avec 32 octets de données :
Réponse de 192.168.102.1 : octets=32 temps=4 ms TTL=128
Réponse de 192.168.102.1 : octets=32 temps=4 ms TTL=128
Réponse de 192.168.102.1 : octets=32 temps=4 ms TTL=128
Réponse de 192.168.102.1 : octets=32 temps=6 ms TTL=128
```


### Configuration des routes (VM1)
```
sudo ip route add 192.168.102.0/24 via 192.168.101.1 dev enp0s8
```
Ping 1:
```
[nawak@localhost ~]$ ping 192.168.112.2
PING 192.168.112.2 (192.168.112.2) 56(84) bytes of data.
64 bytes from 192.168.112.2: icmp_seq=1 ttl=127 time=5.05 ms
64 bytes from 192.168.112.2: icmp_seq=2 ttl=127 time=5.03 ms
^C
--- 192.168.112.2 ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1001ms
rtt min/avg/max/mdev = 5.034/5.043/5.052/0.009 ms
```
Ping 2:
```
[nawak@localhost ~]$ ping 192.168.101.1
PING 192.168.101.1 (192.168.101.1) 56(84) bytes of data.
64 bytes from 192.168.101.1: icmp_seq=1 ttl=128 time=0.250 ms
64 bytes from 192.168.101.1: icmp_seq=2 ttl=128 time=0.238 ms
^C
--- 192.168.101.1 ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 999ms
rtt min/avg/max/mdev = 0.238/0.244/0.250/0.006 ms
```

## 3. Configuration des noms de domaine
### Fichier de configuration hôte (PC1)
```
127.0.0.1 pc1.tp3.b1
192.168.112.2 pc2.tp3.b1
192.168.102.10 vm2.tp3.b1
192.168.101.10 vm1.tp3.b1
```
### Fichier de configuraion serveur (VM1)
```
127.0.0.1   localhost localhost.localdomain localhost4 localhost4.localdomain4
::1         localhost localhost.localdomain localhost6 localhost6.localdomain6
192.168.101.10 vm1 vm1.tp3.b1
192.168.102.10 vm2 vm2.tp3.b1
192.168.112.2 pc2 pc2.tp3.b1
192.168.112.1 pc1 pc1.tp3.b1
```
### Ping VM1 --> PC1
```
[nawak@localhost ~]$ ping pc1
PING pc1 (192.168.112.1) 56(84) bytes of data.
64 bytes from pc1 (192.168.112.1): icmp_seq=1 ttl=127 time=0.208 ms
64 bytes from pc1 (192.168.112.1): icmp_seq=2 ttl=127 time=0.288 ms
^C
--- pc1 ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 999ms
rtt min/avg/max/mdev = 0.208/0.248/0.288/0.040 ms
```
### Ping VM1 --> PC2
```
[nawak@localhost ~]$ ping pc2
PING pc2 (192.168.112.2) 56(84) bytes of data.
64 bytes from pc2 (192.168.112.2): icmp_seq=1 ttl=127 time=4.81 ms
64 bytes from pc2 (192.168.112.2): icmp_seq=2 ttl=127 time=7.19 ms
^C
--- pc2 ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1002ms
rtt min/avg/max/mdev = 4.814/6.004/7.195/1.193 ms
```
### Ping VM1 --> VM2
```
[nawak@localhost ~]$ ping vm2
PING vm2 (192.168.102.10) 56(84) bytes of data.
64 bytes from vm2 (192.168.102.10): icmp_seq=1 ttl=62 time=5.21 ms
64 bytes from vm2 (192.168.102.10): icmp_seq=2 ttl=62 time=4.99 ms
^C
--- vm2 ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1001ms
rtt min/avg/max/mdev = 4.990/5.103/5.217/0.134 ms
```

### Netcat VM1 --> VM2
```
[nawak@localhost ~]$ nc vm2  5454
miaou
netcat
```

### Netcat VM2 --> VM1
```
[nawak@localhost ~]$ nc -l 5454
Voici un joli netcat
miaou
```