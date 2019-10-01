## TP6-lucas_mouchague
### Mise en place du lab :
#### Checklist VMs :
#####Config des interfaces 
Router 1 :
```
r1.tp6#show ip int br
Interface                  IP-Address      OK? Method Status                Protocol
Ethernet0/0                10.6.100.1      YES manual up                    up
Ethernet0/1                10.6.100.5      YES manual up                    up
Ethernet0/2                10.6.202.254    YES manual up                    up
Ethernet0/3                unassigned      YES unset  administratively down down
```
Router 2 :
```
r2.tp6#show ip int br
Interface                  IP-Address      OK? Method Status                Protocol
Ethernet0/0                10.6.100.2      YES manual up                    up
Ethernet0/1                10.3.100.9      YES manual up                    up
Ethernet0/2                unassigned      YES unset  administratively down down
Ethernet0/3                unassigned      YES unset  administratively down down
```
Router 3 :
```
r3.tp6#show ip int br
Interface                  IP-Address      OK? Method Status                Protocol
Ethernet0/0                10.6.100.14     YES manual up                    up
Ethernet0/1                10.6.100.10     YES manual up                    up
Ethernet0/2                10.6.101.1      YES manual up                    up
Ethernet0/3                unassigned      YES unset  administratively down down
```
Router 4 :
```
r4.tp6#show ip int br
Interface                  IP-Address      OK? Method Status                Protocol
Ethernet0/0                10.6.100.6      YES manual up                    up
Ethernet0/1                10.6.100.13     YES manual up                    up
Ethernet0/2                unassigned      YES unset  administratively down down
Ethernet0/3                unassigned      YES unset  administratively down down
```
Router 5 :
```
r5.tp6#show ip int br
Interface                  IP-Address      OK? Method Status                Protocol
Ethernet0/0                10.6.101.2      YES manual up                    up
Ethernet0/1                10.6.251.254    YES manual up                    up
Ethernet0/2                unassigned      YES unset  administratively down down
Ethernet0/3                unassigned      YES unset  administratively down down
```
Client 1 :
```
sudo nano /etc/sysconfig/network-scripts/ifcfg-enp0s3
```
```
NAME=enp0s3
DEVICE=enp0s3

BOOTPROTO=static
ONBOOT=yes

IPADDR=10.6.201.10
NETMASK=255.255.255.0
GATEWAY=10.6.201.254
```
Client 2 :
```
sudo nano /etc/sysconfig/network-scripts/ifcfg-enp0s3
```
```
NAME=enp0s3
DEVICE=enp0s3

BOOTPROTO=static
ONBOOT=yes

IPADDR=10.6.201.11
NETMASK=255.255.255.0
GATEWAY=10.6.201.254
```
Server 1 :
```
sudo nano /etc/sysconfig/network-scripts/ifcfg-enp0s3
```
```
NAME=enp0s3
DEVICE=enp0s3

BOOTPROTO=static
ONBOOT=yes

IPADDR=10.6.202.10
NETMASK=255.255.255.0
GATEWAY=10.6.202.254
```
##### Config des fichiers host
Server1 :
```
127.0.0.1   localhost localhost.localdomain localhost4 localhost4.localdomain4
::1         localhost localhost.localdomain localhost6 localhost6.localdomain6
10.6.201.11 client2
10.6.201.10 client1
```
Client1 :
```
127.0.0.1   localhost localhost.localdomain localhost4 localhost4.localdomain4
::1         localhost localhost.localdomain localhost6 localhost6.localdomain6
10.6.202.10 server1
10.6.201.11 client2
```
Client2 :
```
127.0.0.1   localhost localhost.localdomain localhost4 localhost4.localdomain4
::1         localhost localhost.localdomain localhost6 localhost6.localdomain6
10.6.202.10 server1
10.6.201.10 client1
``` 
#### Configuration de OSPF
##### Activation de OSPF
Router 1 :
```
r1.tp6#show ip protocols
Routing Protocol is "ospf 1"
  Outgoing update filter list for all interfaces is not set
  Incoming update filter list for all interfaces is not set
  Router ID 1.1.1.1
  It is an area border router
  Number of areas in this router is 2. 2 normal 0 stub 0 nssa
  Maximum path: 4
  Routing for Networks:
    10.6.100.0 0.0.0.3 area 0
    10.6.100.4 0.0.0.3 area 0
    10.6.202.0 0.0.0.255 area 2
 Reference bandwidth unit is 100 mbps
  Routing Information Sources:
    Gateway         Distance      Last Update
    3.3.3.3              110      00:03:12
    5.5.5.5              110      00:05:56
    4.4.4.4              110      00:03:12
  Distance: (default is 110)
```
Router 2 : 
```
r2.tp6#show ip protocols
Routing Protocol is "ospf 1"
  Outgoing update filter list for all interfaces is not set
  Incoming update filter list for all interfaces is not set
  Router ID 2.2.2.2
  Number of areas in this router is 1. 1 normal 0 stub 0 nssa
  Maximum path: 4
  Routing for Networks:
    10.6.100.0 0.0.0.3 area 0
    10.6.100.8 0.0.0.3 area 0
 Reference bandwidth unit is 100 mbps
  Routing Information Sources:
    Gateway         Distance      Last Update
    3.3.3.3              110      00:04:04
    5.5.5.5              110      00:06:47
    1.1.1.1              110      00:04:04
    4.4.4.4              110      00:04:04
  Distance: (default is 110)
```
Router 3 :
```
r3.tp6#show ip protocols
Routing Protocol is "ospf 1"
  Outgoing update filter list for all interfaces is not set
  Incoming update filter list for all interfaces is not set
  Router ID 3.3.3.3
  It is an area border router
  Number of areas in this router is 2. 2 normal 0 stub 0 nssa
  Maximum path: 4
  Routing for Networks:
    10.6.100.8 0.0.0.3 area 0
    10.6.100.12 0.0.0.3 area 0
    10.6.101.0 0.0.0.3 area 1
 Reference bandwidth unit is 100 mbps
  Routing Information Sources:
    Gateway         Distance      Last Update
    1.1.1.1              110      00:00:45
    2.2.2.2              110      00:02:52
    4.4.4.4              110      00:02:52
  Distance: (default is 110)
```
Router 4 :
```
r4.tp6#show ip protocols
Routing Protocol is "ospf 1"
  Outgoing update filter list for all interfaces is not set
  Incoming update filter list for all interfaces is not set
  Router ID 4.4.4.4
  Number of areas in this router is 1. 1 normal 0 stub 0 nssa
  Maximum path: 4
  Routing for Networks:
    10.6.100.4 0.0.0.3 area 0
    10.6.100.12 0.0.0.3 area 0
 Reference bandwidth unit is 100 mbps
  Routing Information Sources:
    Gateway         Distance      Last Update
    1.1.1.1              110      00:04:57
    3.3.3.3              110      00:04:57
    5.5.5.5              110      00:07:40
    2.2.2.2              110      00:04:57
  Distance: (default is 110)

```
Router 5 :
```
r5.tp6#show ip protocols
Routing Protocol is "ospf 1"
  Outgoing update filter list for all interfaces is not set
  Incoming update filter list for all interfaces is not set
  Router ID 5.5.5.5
  Number of areas in this router is 1. 1 normal 0 stub 0 nssa
  Maximum path: 4
  Routing for Networks:
    10.6.101.0 0.0.0.3 area 1
    10.6.201.0 0.0.0.255 area 1
 Reference bandwidth unit is 100 mbps
  Routing Information Sources:
    Gateway         Distance      Last Update
    1.1.1.1              110      00:08:05
    3.3.3.3              110      00:02:45
    2.2.2.2              110      00:08:05
    4.4.4.4              110      00:08:05
  Distance: (default is 110)
```