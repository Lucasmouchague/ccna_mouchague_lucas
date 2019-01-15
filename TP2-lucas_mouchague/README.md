# TP2 lucas_mouchague
## Affichage d'informations sur la pile TCP/IP locale:
### CMD 
ipconfig/all

### CARTE WIFI 
Nom = Intel(R) Dual Band Wireless-AC 8265
Adresse MAC = BC-A8-A6-FD-DA-7C
Adresse IP = 10.33.3.84
Masque de sous réseaux = 255.255.252.0 (/22)
Adresse de réseaux = 10.33.0.0
Adresse de broadcast = 10.33.3.255

### CARTE ETHERNET 
Nom : Realtek PCIe GBE Family Controller
Adresse MAC = B0-6E-BF-51-49-0C
Adresse IP = Non renseigné car non connecté

### GATEWAY 
ipconfig/all > Passerelle par défaut : 10.33.3.253

### GUI (Windows) 
Clique droit sur le l'icone wifi > ouvrir les paramètres réseau et internet > afficher les propriétés réseau (et voila)
### CARTE WIFI 	
Adresse IP = 10.33.3.84 (/22)
Adresse MAC = BC-A8-A6-FD-DA-7C
GATEWAY  = 10.33.3.253

__A quoi set la gateway dans le réseau d'Ingésup__
Elle sert a acceder au réseau internet extérieur a l'école (ou dumoins exterieur au routeur de sortie).

# Modifications des informations 
## A 
Première adresse IP du réseau : 10.33.0.1
Dernière adresse IP du réseau : 10.33.3.252 
### Changement adresse IP 
Panneau de configuration > Centre réseau et partage > Cliquer sur sa connexion internet > Propriétés > Choisi 'Protocole Internet Version 4 (TCP/IPv4)' > Propriétés > cochez 'Utiliser l adresse IP suivante'
Adresse utilisé  = 10.33.3.45 
Masque de sous réseau = 255.255.252.0
GATEWAY = 10.33.3.253

Connexion internet OK!

## NMAP
### B
Découverte de nmap

### C
Modification de l'adresse IP sur : 10.33.2.246

Connexion Internet OK!

Modifications de la GATEWAY sur 10.33.3.247

Connexion Internet KO!

#EXPLORATION LOCALE EN DUO

## 3
### Modification adresse IP
Panneau de configuration > Centre réseau et partage > Cliquer sur sa connexion ethernet > Propriétés > Choisi 'Protocole Internet Version 4 (TCP/IPv4)' > Propriétés > cochez 'Utiliser l adresse IP suivante'

Adresse utilisé : 192.168.1.1 /24

Ping 192.168.1.3 > Connexion OK!

Plus petit masque possible : 255.255.255.252

Ping 192.168.1.2 > Connexion OK!

## 4
Le pont réseau a fonctionner et le pc client a pu aller sur internet

## 5
### Partie serveur (Sous linux)
Dans un terminal: nc -l -p 8888
J'envoie un message il est reçu par l'autre poste et inversement.

### Partie client
Dans un terminal: nc 192.168.1.2
J'envoie un message il est reçu par l'autre poste et inversement.

## 6 
### Ping de l'ip 192.168.1.2 (Mon ip: 192.168.1.1)
Les messages "Echo (ping) request id=0x003a, ..." et "Echo (ping) reply id=0x003a, ..." sont visibles.
![alt text](https://github.com/Lucasmouchague/TP2-lucas_mouchague/blob/master/Trame%20NC.PNG)


## 7 
### Firewall
#### Autorsation des pings (Windaube)
Panneau de configuration > Système et sécurité > Pare feu windows defender > Paramètre avancé > Nouvelle règle > Personnalisée > Protocole ICMPv4 > Toute adresse IP > autoriser la connexion.

#### Partie serveur (sous linux)
dans un terminal: nc -l -p 2000
J'envoie un message il est reçu par l'autre poste et inversement.


### Partie client
Dans un terminal: nc 192.168.1.2 -p 2000
J'envoie un message il est reçu par l'autre poste et inversement.

# Manipulations d'autres outils/protocoles côté clients
## DHCP (sous windows)
Dans un invite de commande: ipconfig /all 
--> Serveur DHCP : 10.33.3.254       Bail expirant : lundi 7 janvier 2019 14:56:39

### Pour renouveler l'adresse ip:
Dans un invite de commande: ipconfig /renew (Windaube a refusé de le faire)

## DNS
### Dans un invite de commande: ipconfig /all 
--> Serveur DNS: 10.33.10.20

### Dans un invite de commande: nslookup google.com 
--> Addresses:  2a00:1450:4007:810::200e 216.58.208.238
Ici on nous montre que pour le nom de domaine google.com il y a deux adresse IP (une IPV4 l'autre IPV6)
### Dans un invite de commande: nslookup ynov.com 
--> Address:  217.70.184.38
Ici on nous montre que pour le nom de domaine ynov.com il y a une adresse IP (IPV4)

### Dans un invite de commande: nslookup 78.78.21.21
--> Nom : host-78-78-21-21.mobileonline.telia.com
Ici on nous montre le nom de domaine pour l'IP 78.78.21.21

### Dans un invite de commande: nslookup 92.16.54.88
--> Nom : host-92-16-54-88.as13285.net
Ici on nous montre le nom de domaine pour l'IP 92.16.54.88