# Configurations – Réseau d'entreprise – Mini projet final

Configuration complète de chaque équipement, exportée depuis le fichier Packet Tracer.
Les mots de passe et numéros de série sont masqués ; les interfaces sans configuration sont omises.
Les fichiers bruts (un par équipement) sont dans [`configs/`](configs/).

## Sommaire

- [CORE-R1](#core-r1) – Router 2911
- [CORE-R2](#core-r2) – Router 2911
- [ISP1](#isp1) – Router 2811
- [ISP2](#isp2) – Router 2811
- [Multilayer1](#multilayer1) – MultiLayerSwitch 3650-24PS
- [Multilayer2](#multilayer2) – MultiLayerSwitch 3650-24PS
- [ADMIN](#admin) – Switch 2960-24TT
- [FINANCE](#finance) – Switch 2960-24TT
- [HR](#hr) – Switch 2960-24TT
- [ICT](#ict) – Switch 2960-24TT
- [SALES](#sales) – Switch 2960-24TT
- [SERVERROOM](#serverroom) – Switch 2960-24TT
- [Postes et serveurs](#postes-et-serveurs)

## Postes et serveurs

| Nom | Type | Adresse IP | Masque | Passerelle | DNS |
|---|---|---|---|---|---|
| Admin-AP | AccessPoint | – | – | – | – |
| Admin-Laptop | Laptop | DHCP | – | 172.16.2.129 | 172.16.3.131 |
| Admin-PC | Pc | – | – | – | – |
| Admin-Printer | Printer | – | – | – | – |
| Admin-Tablet | TabletPC | DHCP | – | 172.16.2.129 | 172.16.3.131 |
| DHCP-SERVER | Server | 172.16.3.130 | 255.255.255.240 | 172.16.3.129 | 172.16.3.131 |
| DNS-SERVER | Server | 172.16.3.131 | 255.255.255.240 | 172.16.3.129 | – |
| EMAIL-SERVER | Server | 172.16.3.133 | 255.255.255.240 | 172.16.3.129 | – |
| Finance-AP | AccessPoint | – | – | – | – |
| Finance-Laptop | Laptop | DHCP | – | 172.16.2.1 | 172.16.3.131 |
| Finance-PC | Pc | – | – | – | – |
| Finance-Printer | Printer | – | – | – | – |
| Finance-Tablet | TabletPC | DHCP | – | 172.16.2.1 | 172.16.3.131 |
| HR-AP | AccessPoint | – | – | – | – |
| HR-Laptop | Laptop | DHCP | – | 172.16.1.129 | 172.16.3.131 |
| HR-PC | Pc | – | – | – | – |
| HR-Printer | Printer | – | – | – | – |
| HR-Tablet | TabletPC | DHCP | – | 172.16.1.129 | 172.16.3.131 |
| ICT-AP | AccessPoint | – | – | – | – |
| ICT-Laptop | Laptop | DHCP | – | 172.16.3.1 | 172.16.3.131 |
| ICT-PC | Pc | – | – | – | – |
| ICT-Printer | Printer | – | – | – | – |
| ICT-Tablet | TabletPC | DHCP | – | 172.16.3.1 | 172.16.3.131 |
| Sales-AP | AccessPoint | – | – | – | – |
| Sales-Laptop | Laptop | DHCP | – | 172.16.1.1 | 172.16.3.131 |
| Sales-PC | Pc | – | – | – | – |
| Sales-Printer | Printer | – | – | – | – |
| Sales-Tablet | TabletPC | DHCP | – | 172.16.1.1 | 172.16.3.131 |
| SysAdmin-PC | Pc | 172.16.3.132 | 255.255.255.240 | 172.16.3.129 | 172.16.3.131 |

*DHCP* : adresse attribuée dynamiquement par le serveur DHCP.

### Services de DHCP-SERVER

- DHCP pool `SalesPool` : réseau 172.16.1.0/255.255.255.128, plage 172.16.1.6 → 172.16.1.125, passerelle 172.16.1.1, DNS 172.16.3.131
- DHCP pool `HRPool` : réseau 172.16.1.128/255.255.255.128, plage 172.16.1.134 → 172.16.1.253, passerelle 172.16.1.129, DNS 172.16.3.131
- DHCP pool `AdminPool` : réseau 172.16.2.128/255.255.255.128, plage 172.16.2.134 → 172.16.2.253, passerelle 172.16.2.129, DNS 172.16.3.131
- DHCP pool `ICTPool` : réseau 172.16.3.0/255.255.255.128, plage 172.16.3.6 → 172.16.3.125, passerelle 172.16.3.1, DNS 172.16.3.131
- DHCP pool `FinancePool` : réseau 172.16.2.0/255.255.255.128, plage 172.16.2.6 → 172.16.2.125, passerelle 172.16.2.1, DNS 172.16.3.131

### Services de DNS-SERVER

- DNS enregistrement A : `www.gtech.com` → 172.16.3.131

## CORE-R1

Router · modèle 2911 · hostname `CORE-R1`

```cisco
!
version 15.1
no service timestamps log datetime msec
no service timestamps debug datetime msec
service password-encryption
!
hostname CORE-R1
!
enable password 7 <redacted>
!
no ip cef
no ipv6 cef
!
username admin password 7 <redacted>
!
license udi pid CISCO2911/K9 sn <redacted>
!
ip ssh version 2
no ip domain-lookup
ip domain-name cisco.net
!
spanning-tree mode pvst
!
interface GigabitEthernet0/0
 ip address 172.16.3.146 255.255.255.252
 ip nat inside
 duplex auto
 speed auto
!
interface GigabitEthernet0/1
 ip address 172.16.3.154 255.255.255.252
 ip nat inside
 duplex auto
 speed auto
!
interface GigabitEthernet0/2
 no ip address
 duplex auto
 speed auto
 shutdown
!
interface Serial0/2/0
 ip address 195.136.17.1 255.255.255.252
 ip nat outside
 clock rate 64000
!
interface Serial0/2/1
 ip address 195.136.17.5 255.255.255.252
 ip nat outside
 clock rate 64000
!
interface Vlan1
 no ip address
 shutdown
!
router ospf 10
 router-id 3.3.3.3
 log-adjacency-changes
 network 172.16.3.144 0.0.0.3 area 0
 network 172.16.3.152 0.0.0.3 area 0
 network 195.136.17.0 0.0.0.3 area 0
 network 195.136.17.4 0.0.0.3 area 0
!
ip nat inside source list 1 interface Serial0/2/1 overload
ip classless
ip route 0.0.0.0 0.0.0.0 Serial0/2/0 
ip route 0.0.0.0 0.0.0.0 Serial0/2/1 70
!
ip flow-export version 9
!
access-list 1 permit 172.16.1.0 0.0.0.127
access-list 1 permit 172.16.1.128 0.0.0.127
access-list 1 permit 172.16.2.0 0.0.0.127
access-list 1 permit 172.16.2.128 0.0.0.127
access-list 1 permit 172.16.3.0 0.0.0.127
access-list 1 permit 172.16.3.128 0.0.0.15
!
banner motd #NO Unauthorised Access!!!#
!
line con 0
 password 7 <redacted>
 login
!
line aux 0
!
line vty 0 4
 login local
 transport input ssh
line vty 5 15
 login local
 transport input ssh
!
end
```

## CORE-R2

Router · modèle 2911 · hostname `CORE-R2`

```cisco
!
version 15.1
no service timestamps log datetime msec
no service timestamps debug datetime msec
service password-encryption
!
hostname CORE-R2
!
enable password 7 <redacted>
!
no ip cef
no ipv6 cef
!
username admin password 7 <redacted>
!
license udi pid CISCO2911/K9 sn <redacted>
!
ip ssh version 2
no ip domain-lookup
ip domain-name cisco.net
!
spanning-tree mode pvst
!
interface GigabitEthernet0/0
 ip address 172.16.3.150 255.255.255.252
 ip nat inside
 duplex auto
 speed auto
!
interface GigabitEthernet0/1
 ip address 172.16.3.158 255.255.255.252
 ip nat inside
 duplex auto
 speed auto
!
interface GigabitEthernet0/2
 no ip address
 duplex auto
 speed auto
 shutdown
!
interface Serial0/2/0
 ip address 195.136.17.9 255.255.255.252
 ip nat outside
 clock rate 64000
!
interface Serial0/2/1
 ip address 195.136.17.13 255.255.255.252
 ip nat outside
 clock rate 64000
!
interface Vlan1
 no ip address
 shutdown
!
router ospf 10
 router-id 4.4.4.4
 log-adjacency-changes
 network 172.16.3.148 0.0.0.3 area 0
 network 172.16.3.156 0.0.0.3 area 0
 network 195.136.17.8 0.0.0.3 area 0
 network 195.136.17.12 0.0.0.3 area 0
!
ip nat inside source list 1 interface Serial0/2/1 overload
ip classless
ip route 0.0.0.0 0.0.0.0 Serial0/2/0 
ip route 0.0.0.0 0.0.0.0 Serial0/2/1 70
!
ip flow-export version 9
!
access-list 1 permit 172.16.1.0 0.0.0.127
access-list 1 permit 172.16.1.128 0.0.0.127
access-list 1 permit 172.16.2.0 0.0.0.127
access-list 1 permit 172.16.2.128 0.0.0.127
access-list 1 permit 172.16.3.0 0.0.0.127
access-list 1 permit 172.16.3.128 0.0.0.15
!
banner motd #NO Unauthorised Access!!!#
!
line con 0
 password 7 <redacted>
 login
!
line aux 0
!
line vty 0 4
 login local
 transport input ssh
line vty 5 15
 login local
 transport input ssh
!
end
```

## ISP1

Router · modèle 2811 · hostname `Router`

```cisco
!
version 15.1
no service timestamps log datetime msec
no service timestamps debug datetime msec
no service password-encryption
!
hostname Router
!
no ip cef
no ipv6 cef
!
license udi pid CISCO2811/K9 sn <redacted>
!
spanning-tree mode pvst
!
interface FastEthernet0/0
 no ip address
 duplex auto
 speed auto
!
interface FastEthernet0/1
 no ip address
 duplex auto
 speed auto
!
interface Serial0/3/0
 ip address 195.136.17.2 255.255.255.252
!
interface Serial0/3/1
 ip address 195.136.17.10 255.255.255.252
!
interface Vlan1
 no ip address
!
router ospf 10
 router-id 5.5.5.5
 log-adjacency-changes
 network 195.136.17.8 0.0.0.3 area 0
 network 195.136.17.0 0.0.0.3 area 0
!
ip classless
!
ip flow-export version 9
!
line con 0
!
line aux 0
!
line vty 0 4
 login
!
end
```

## ISP2

Router · modèle 2811 · hostname `Router`

```cisco
!
version 15.1
no service timestamps log datetime msec
no service timestamps debug datetime msec
no service password-encryption
!
hostname Router
!
no ip cef
no ipv6 cef
!
license udi pid CISCO2811/K9 sn <redacted>
!
spanning-tree mode pvst
!
interface FastEthernet0/0
 no ip address
 duplex auto
 speed auto
!
interface FastEthernet0/1
 no ip address
 duplex auto
 speed auto
!
interface Serial0/3/0
 ip address 195.136.17.6 255.255.255.252
!
interface Serial0/3/1
 ip address 195.136.17.14 255.255.255.252
!
interface Vlan1
 no ip address
!
router ospf 10
 router-id 6.6.6.6
 log-adjacency-changes
 network 195.136.17.4 0.0.0.3 area 0
 network 195.136.17.12 0.0.0.3 area 0
!
ip classless
!
ip flow-export version 9
!
line con 0
!
line aux 0
!
line vty 0 4
 login
!
end
```

## Multilayer1

MultiLayerSwitch · modèle 3650-24PS · hostname `Mlt-SW1`

```cisco
!
version 16.3.2
no service timestamps log datetime msec
no service timestamps debug datetime msec
service password-encryption
!
hostname Mlt-SW1
!
no profinet
enable password 7 <redacted>
!
no ip cef
ip routing
!
no ipv6 cef
!
username admin password 7 <redacted>
!
ip ssh version 2
no ip domain-lookup
ip domain-name cisco.net
!
spanning-tree mode pvst
!
interface GigabitEthernet1/0/1
 no switchport
 ip address 172.16.3.145 255.255.255.252
 duplex auto
 speed auto
!
interface GigabitEthernet1/0/2
 no switchport
 ip address 172.16.3.149 255.255.255.252
 duplex auto
 speed auto
!
interface GigabitEthernet1/0/3
 switchport mode trunk
!
interface GigabitEthernet1/0/4
 switchport mode trunk
!
interface GigabitEthernet1/0/5
 switchport mode trunk
!
interface GigabitEthernet1/0/6
 switchport mode trunk
!
interface GigabitEthernet1/0/7
 switchport mode trunk
!
interface GigabitEthernet1/0/8
 switchport mode trunk
!
interface Vlan1
 no ip address
 shutdown
!
interface Vlan10
 mac-address 00e0.b02c.0b01
 ip address 172.16.1.1 255.255.255.128
 ip helper-address 172.16.3.130
!
interface Vlan20
 mac-address 00e0.b02c.0b02
 ip address 172.16.1.129 255.255.255.128
 ip helper-address 172.16.3.130
!
interface Vlan30
 mac-address 00e0.b02c.0b03
 ip address 172.16.2.1 255.255.255.128
 ip helper-address 172.16.3.130
!
interface Vlan40
 mac-address 00e0.b02c.0b04
 ip address 172.16.2.129 255.255.255.128
 ip helper-address 172.16.3.130
!
interface Vlan50
 mac-address 00e0.b02c.0b05
 ip address 172.16.3.1 255.255.255.128
 ip helper-address 172.16.3.130
!
interface Vlan60
 mac-address 00e0.b02c.0b06
 ip address 172.16.3.129 255.255.255.240
!
router ospf 10
 router-id 2.2.2.2
 log-adjacency-changes
 network 172.16.1.0 0.0.0.127 area 0
 network 172.16.1.128 0.0.0.127 area 0
 network 172.16.2.0 0.0.0.127 area 0
 network 172.16.2.128 0.0.0.127 area 0
 network 172.16.3.0 0.0.0.127 area 0
 network 172.16.3.144 0.0.0.3 area 0
 network 172.16.3.148 0.0.0.3 area 0
 network 172.16.3.128 0.0.0.15 area 0
!
ip classless
ip route 0.0.0.0 0.0.0.0 GigabitEthernet1/0/1 
ip route 0.0.0.0 0.0.0.0 GigabitEthernet1/0/2 70
!
ip flow-export version 9
!
banner motd #NO Unauthorised Access!!!#
!
line con 0
 password 7 <redacted>
 login
!
line aux 0
!
line vty 0 4
 login local
 transport input ssh
line vty 5 15
 login local
 transport input ssh
!
end
```

## Multilayer2

MultiLayerSwitch · modèle 3650-24PS · hostname `Mlt-SW2`

```cisco
!
version 16.3.2
no service timestamps log datetime msec
no service timestamps debug datetime msec
service password-encryption
!
hostname Mlt-SW2
!
no profinet
enable password 7 <redacted>
!
no ip cef
ip routing
!
no ipv6 cef
!
username admin password 7 <redacted>
!
ip ssh version 2
no ip domain-lookup
ip domain-name cisco.net
!
spanning-tree mode pvst
!
interface GigabitEthernet1/0/1
 no switchport
 ip address 172.16.3.153 255.255.255.252
 duplex auto
 speed auto
!
interface GigabitEthernet1/0/2
 no switchport
 ip address 172.16.3.157 255.255.255.252
 duplex auto
 speed auto
!
interface GigabitEthernet1/0/3
 switchport mode trunk
!
interface GigabitEthernet1/0/4
 switchport mode trunk
!
interface GigabitEthernet1/0/5
 switchport mode trunk
!
interface GigabitEthernet1/0/6
 switchport mode trunk
!
interface GigabitEthernet1/0/7
 switchport mode trunk
!
interface GigabitEthernet1/0/8
 switchport mode trunk
!
interface Vlan1
 no ip address
 shutdown
!
interface Vlan10
 mac-address 0060.47a9.1901
 ip address 172.16.1.1 255.255.255.128
 ip helper-address 172.16.3.130
!
interface Vlan20
 mac-address 0060.47a9.1902
 ip address 172.16.1.129 255.255.255.128
 ip helper-address 172.16.3.130
!
interface Vlan30
 mac-address 0060.47a9.1903
 ip address 172.16.2.1 255.255.255.128
 ip helper-address 172.16.3.130
!
interface Vlan40
 mac-address 0060.47a9.1904
 ip address 172.16.2.129 255.255.255.128
 ip helper-address 172.16.3.130
!
interface Vlan50
 mac-address 0060.47a9.1905
 ip address 172.16.3.1 255.255.255.128
 ip helper-address 172.16.3.130
!
interface Vlan60
 mac-address 0060.47a9.1906
 ip address 172.16.3.129 255.255.255.240
!
router ospf 10
 router-id 1.1.1.1
 log-adjacency-changes
 network 172.16.1.0 0.0.0.127 area 0
 network 172.16.1.128 0.0.0.127 area 0
 network 172.16.2.0 0.0.0.127 area 0
 network 172.16.2.128 0.0.0.127 area 0
 network 172.16.3.0 0.0.0.127 area 0
 network 172.16.3.152 0.0.0.3 area 0
 network 172.16.3.156 0.0.0.3 area 0
 network 172.16.3.128 0.0.0.15 area 0
!
ip classless
ip route 0.0.0.0 0.0.0.0 GigabitEthernet1/0/1 
ip route 0.0.0.0 0.0.0.0 GigabitEthernet1/0/2 70
!
ip flow-export version 9
!
banner motd #NO Unauthorised Access!!!#
!
line con 0
 password 7 <redacted>
 login
!
line aux 0
!
line vty 0 4
 login local
 transport input ssh
line vty 5 15
 login local
 transport input ssh
!
end
```

## ADMIN

Switch · modèle 2960-24TT · hostname `Admin-SW`

```cisco
!
version 15.0
no service timestamps log datetime msec
no service timestamps debug datetime msec
service password-encryption
!
hostname Admin-SW
!
enable password 7 <redacted>
!
no ip domain-lookup
!
spanning-tree mode pvst
spanning-tree extend system-id
!
interface FastEthernet0/1
 switchport mode trunk
!
interface FastEthernet0/2
 switchport mode trunk
!
interface FastEthernet0/3
 switchport access vlan 40
 switchport mode access
!
interface FastEthernet0/4
 switchport access vlan 40
 switchport mode access
!
interface FastEthernet0/5
 switchport access vlan 40
 switchport mode access
!
interface FastEthernet0/6
 switchport access vlan 40
 switchport mode access
!
interface FastEthernet0/7
 switchport access vlan 40
 switchport mode access
!
interface FastEthernet0/8
 switchport access vlan 40
 switchport mode access
!
interface FastEthernet0/9
 switchport access vlan 40
 switchport mode access
!
interface FastEthernet0/10
 switchport access vlan 40
 switchport mode access
!
interface FastEthernet0/11
 switchport access vlan 40
 switchport mode access
!
interface FastEthernet0/12
 switchport access vlan 40
 switchport mode access
!
interface FastEthernet0/13
 switchport access vlan 40
 switchport mode access
!
interface FastEthernet0/14
 switchport access vlan 40
 switchport mode access
!
interface FastEthernet0/15
 switchport access vlan 40
 switchport mode access
!
interface FastEthernet0/16
 switchport access vlan 40
 switchport mode access
!
interface FastEthernet0/17
 switchport access vlan 40
 switchport mode access
!
interface FastEthernet0/18
 switchport access vlan 40
 switchport mode access
!
interface FastEthernet0/19
 switchport access vlan 40
 switchport mode access
!
interface FastEthernet0/20
 switchport access vlan 40
 switchport mode access
!
interface FastEthernet0/21
 switchport access vlan 40
 switchport mode access
!
interface FastEthernet0/22
 switchport access vlan 40
 switchport mode access
!
interface FastEthernet0/23
 switchport access vlan 40
 switchport mode access
!
interface FastEthernet0/24
 switchport access vlan 40
 switchport mode access
!
interface GigabitEthernet0/1
 switchport access vlan 99
 switchport mode access
!
interface GigabitEthernet0/2
 switchport access vlan 99
 switchport mode access
!
interface Vlan1
 no ip address
 shutdown
!
banner motd #NO Unauthorised Access!!!#
!
line con 0
 password 7 <redacted>
 login
!
line vty 0 4
 login
line vty 5 15
 login
!
end
```

## FINANCE

Switch · modèle 2960-24TT · hostname `Finance-SW`

```cisco
!
version 15.0
no service timestamps log datetime msec
no service timestamps debug datetime msec
service password-encryption
!
hostname Finance-SW
!
enable password 7 <redacted>
!
no ip domain-lookup
!
spanning-tree mode pvst
spanning-tree extend system-id
!
interface FastEthernet0/1
 switchport mode trunk
!
interface FastEthernet0/2
 switchport mode trunk
!
interface FastEthernet0/3
 switchport access vlan 30
 switchport mode access
 switchport port-security
 switchport port-security mac-address sticky 
 switchport port-security mac-address sticky 000C.8580.A6AB
!
interface FastEthernet0/4
 switchport access vlan 30
 switchport mode access
 switchport port-security
 switchport port-security mac-address sticky 
 switchport port-security mac-address sticky 0002.4AAB.9191
!
interface FastEthernet0/5
 switchport access vlan 30
 switchport mode access
!
interface FastEthernet0/6
 switchport access vlan 30
 switchport mode access
 switchport port-security
 switchport port-security mac-address sticky 
!
interface FastEthernet0/7
 switchport access vlan 30
 switchport mode access
 switchport port-security
 switchport port-security mac-address sticky 
!
interface FastEthernet0/8
 switchport access vlan 30
 switchport mode access
 switchport port-security
 switchport port-security mac-address sticky 
!
interface FastEthernet0/9
 switchport access vlan 30
 switchport mode access
 switchport port-security
 switchport port-security mac-address sticky 
!
interface FastEthernet0/10
 switchport access vlan 30
 switchport mode access
 switchport port-security
 switchport port-security mac-address sticky 
!
interface FastEthernet0/11
 switchport access vlan 30
 switchport mode access
 switchport port-security
 switchport port-security mac-address sticky 
!
interface FastEthernet0/12
 switchport access vlan 30
 switchport mode access
 switchport port-security
 switchport port-security mac-address sticky 
!
interface FastEthernet0/13
 switchport access vlan 30
 switchport mode access
 switchport port-security
 switchport port-security mac-address sticky 
!
interface FastEthernet0/14
 switchport access vlan 30
 switchport mode access
 switchport port-security
 switchport port-security mac-address sticky 
!
interface FastEthernet0/15
 switchport access vlan 30
 switchport mode access
 switchport port-security
 switchport port-security mac-address sticky 
!
interface FastEthernet0/16
 switchport access vlan 30
 switchport mode access
 switchport port-security
 switchport port-security mac-address sticky 
!
interface FastEthernet0/17
 switchport access vlan 30
 switchport mode access
 switchport port-security
 switchport port-security mac-address sticky 
!
interface FastEthernet0/18
 switchport access vlan 30
 switchport mode access
 switchport port-security
 switchport port-security mac-address sticky 
!
interface FastEthernet0/19
 switchport access vlan 30
 switchport mode access
 switchport port-security
 switchport port-security mac-address sticky 
!
interface FastEthernet0/20
 switchport access vlan 30
 switchport mode access
 switchport port-security
 switchport port-security mac-address sticky 
!
interface FastEthernet0/21
 switchport access vlan 30
 switchport mode access
 switchport port-security
 switchport port-security mac-address sticky 
!
interface FastEthernet0/22
 switchport access vlan 30
 switchport mode access
 switchport port-security
 switchport port-security mac-address sticky 
!
interface FastEthernet0/23
 switchport access vlan 30
 switchport mode access
 switchport port-security
 switchport port-security mac-address sticky 
!
interface FastEthernet0/24
 switchport access vlan 30
 switchport mode access
 switchport port-security
 switchport port-security mac-address sticky 
!
interface GigabitEthernet0/1
 switchport access vlan 99
 switchport mode access
!
interface GigabitEthernet0/2
 switchport access vlan 99
 switchport mode access
!
interface Vlan1
 no ip address
 shutdown
!
banner motd #NO Unauthorised Access!!!#
!
line con 0
 password 7 <redacted>
 login
!
line vty 0 4
 login
line vty 5 15
 login
!
end
```

## HR

Switch · modèle 2960-24TT · hostname `HR-SW`

```cisco
!
version 15.0
no service timestamps log datetime msec
no service timestamps debug datetime msec
service password-encryption
!
hostname HR-SW
!
enable password 7 <redacted>
!
no ip domain-lookup
!
spanning-tree mode pvst
spanning-tree extend system-id
!
interface FastEthernet0/1
 switchport mode trunk
!
interface FastEthernet0/2
 switchport mode trunk
!
interface FastEthernet0/3
 switchport access vlan 20
 switchport mode access
!
interface FastEthernet0/4
 switchport access vlan 20
 switchport mode access
!
interface FastEthernet0/5
 switchport access vlan 20
 switchport mode access
!
interface FastEthernet0/6
 switchport access vlan 20
 switchport mode access
!
interface FastEthernet0/7
 switchport access vlan 20
 switchport mode access
!
interface FastEthernet0/8
 switchport access vlan 20
 switchport mode access
!
interface FastEthernet0/9
 switchport access vlan 20
 switchport mode access
!
interface FastEthernet0/10
 switchport access vlan 20
 switchport mode access
!
interface FastEthernet0/11
 switchport access vlan 20
 switchport mode access
!
interface FastEthernet0/12
 switchport access vlan 20
 switchport mode access
!
interface FastEthernet0/13
 switchport access vlan 20
 switchport mode access
!
interface FastEthernet0/14
 switchport access vlan 20
 switchport mode access
!
interface FastEthernet0/15
 switchport access vlan 20
 switchport mode access
!
interface FastEthernet0/16
 switchport access vlan 20
 switchport mode access
!
interface FastEthernet0/17
 switchport access vlan 20
 switchport mode access
!
interface FastEthernet0/18
 switchport access vlan 20
 switchport mode access
!
interface FastEthernet0/19
 switchport access vlan 20
 switchport mode access
!
interface FastEthernet0/20
 switchport access vlan 20
 switchport mode access
!
interface FastEthernet0/21
 switchport access vlan 20
 switchport mode access
!
interface FastEthernet0/22
 switchport access vlan 20
 switchport mode access
!
interface FastEthernet0/23
 switchport access vlan 20
 switchport mode access
!
interface FastEthernet0/24
 switchport access vlan 20
 switchport mode access
!
interface GigabitEthernet0/1
 switchport access vlan 99
 switchport mode access
!
interface GigabitEthernet0/2
 switchport access vlan 99
 switchport mode access
!
interface Vlan1
 no ip address
 shutdown
!
banner motd #NO Unauthorised Access!!!#
!
line con 0
 password 7 <redacted>
 login
!
line vty 0 4
 login
line vty 5 15
 login
!
end
```

## ICT

Switch · modèle 2960-24TT · hostname `ICT-SW`

```cisco
!
version 15.0
no service timestamps log datetime msec
no service timestamps debug datetime msec
service password-encryption
!
hostname ICT-SW
!
enable password 7 <redacted>
!
no ip domain-lookup
!
spanning-tree mode pvst
spanning-tree extend system-id
!
interface FastEthernet0/1
 switchport mode trunk
!
interface FastEthernet0/2
 switchport mode trunk
!
interface FastEthernet0/3
 switchport access vlan 50
 switchport mode access
!
interface FastEthernet0/4
 switchport access vlan 50
 switchport mode access
!
interface FastEthernet0/5
 switchport access vlan 50
 switchport mode access
!
interface FastEthernet0/6
 switchport access vlan 50
 switchport mode access
!
interface FastEthernet0/7
 switchport access vlan 50
 switchport mode access
!
interface FastEthernet0/8
 switchport access vlan 50
 switchport mode access
!
interface FastEthernet0/9
 switchport access vlan 50
 switchport mode access
!
interface FastEthernet0/10
 switchport access vlan 50
 switchport mode access
!
interface FastEthernet0/11
 switchport access vlan 50
 switchport mode access
!
interface FastEthernet0/12
 switchport access vlan 50
 switchport mode access
!
interface FastEthernet0/13
 switchport access vlan 50
 switchport mode access
!
interface FastEthernet0/14
 switchport access vlan 50
 switchport mode access
!
interface FastEthernet0/15
 switchport access vlan 50
 switchport mode access
!
interface FastEthernet0/16
 switchport access vlan 50
 switchport mode access
!
interface FastEthernet0/17
 switchport access vlan 50
 switchport mode access
!
interface FastEthernet0/18
 switchport access vlan 50
 switchport mode access
!
interface FastEthernet0/19
 switchport access vlan 50
 switchport mode access
!
interface FastEthernet0/20
 switchport access vlan 50
 switchport mode access
!
interface FastEthernet0/21
 switchport access vlan 50
 switchport mode access
!
interface FastEthernet0/22
 switchport access vlan 50
 switchport mode access
!
interface FastEthernet0/23
 switchport access vlan 50
 switchport mode access
!
interface FastEthernet0/24
 switchport access vlan 50
 switchport mode access
!
interface GigabitEthernet0/1
 switchport access vlan 99
 switchport mode access
!
interface GigabitEthernet0/2
 switchport access vlan 99
 switchport mode access
!
interface Vlan1
 no ip address
 shutdown
!
banner motd #NO Unauthorised Access!!!#
!
line con 0
 password 7 <redacted>
 login
!
line vty 0 4
 login
line vty 5 15
 login
!
end
```

## SALES

Switch · modèle 2960-24TT · hostname `Sales-SW`

```cisco
!
version 15.0
no service timestamps log datetime msec
no service timestamps debug datetime msec
service password-encryption
!
hostname Sales-SW
!
enable password 7 <redacted>
!
no ip domain-lookup
!
spanning-tree mode pvst
spanning-tree extend system-id
!
interface FastEthernet0/1
 switchport mode trunk
!
interface FastEthernet0/2
 switchport mode trunk
!
interface FastEthernet0/3
 switchport access vlan 10
 switchport mode access
!
interface FastEthernet0/4
 switchport access vlan 10
 switchport mode access
!
interface FastEthernet0/5
 switchport access vlan 10
 switchport mode access
!
interface FastEthernet0/6
 switchport access vlan 10
 switchport mode access
!
interface FastEthernet0/7
 switchport access vlan 10
 switchport mode access
!
interface FastEthernet0/8
 switchport access vlan 10
 switchport mode access
!
interface FastEthernet0/9
 switchport access vlan 10
 switchport mode access
!
interface FastEthernet0/10
 switchport access vlan 10
 switchport mode access
!
interface FastEthernet0/11
 switchport access vlan 10
 switchport mode access
!
interface FastEthernet0/12
 switchport access vlan 10
 switchport mode access
!
interface FastEthernet0/13
 switchport access vlan 10
 switchport mode access
!
interface FastEthernet0/14
 switchport access vlan 10
 switchport mode access
!
interface FastEthernet0/15
 switchport access vlan 10
 switchport mode access
!
interface FastEthernet0/16
 switchport access vlan 10
 switchport mode access
!
interface FastEthernet0/17
 switchport access vlan 10
 switchport mode access
!
interface FastEthernet0/18
 switchport access vlan 10
 switchport mode access
!
interface FastEthernet0/19
 switchport access vlan 10
 switchport mode access
!
interface FastEthernet0/20
 switchport access vlan 10
 switchport mode access
!
interface FastEthernet0/21
 switchport access vlan 10
 switchport mode access
!
interface FastEthernet0/22
 switchport access vlan 10
 switchport mode access
!
interface FastEthernet0/23
 switchport access vlan 10
 switchport mode access
!
interface FastEthernet0/24
 switchport access vlan 10
 switchport mode access
!
interface GigabitEthernet0/1
 switchport access vlan 99
 switchport mode access
 shutdown
!
interface GigabitEthernet0/2
 switchport access vlan 99
 switchport mode access
 shutdown
!
interface Vlan1
 no ip address
 shutdown
!
banner motd #NO Unauthorised Access!!!#
!
line con 0
 password 7 <redacted>
 login
!
line vty 0 4
 login
line vty 5 15
 login
!
end
```

## SERVERROOM

Switch · modèle 2960-24TT · hostname `ServerRoom-SW`

```cisco
!
version 15.0
no service timestamps log datetime msec
no service timestamps debug datetime msec
service password-encryption
!
hostname ServerRoom-SW
!
enable password 7 <redacted>
!
no ip domain-lookup
!
spanning-tree mode pvst
spanning-tree extend system-id
!
interface FastEthernet0/1
 switchport mode trunk
!
interface FastEthernet0/2
 switchport mode trunk
!
interface FastEthernet0/3
 switchport access vlan 60
 switchport mode access
!
interface FastEthernet0/4
 switchport access vlan 60
 switchport mode access
!
interface FastEthernet0/5
 switchport access vlan 60
 switchport mode access
!
interface FastEthernet0/6
 switchport access vlan 60
 switchport mode access
!
interface FastEthernet0/7
 switchport access vlan 60
 switchport mode access
!
interface FastEthernet0/8
 switchport access vlan 60
 switchport mode access
!
interface FastEthernet0/9
 switchport access vlan 60
 switchport mode access
!
interface FastEthernet0/10
 switchport access vlan 60
 switchport mode access
!
interface FastEthernet0/11
 switchport access vlan 60
 switchport mode access
!
interface FastEthernet0/12
 switchport access vlan 60
 switchport mode access
!
interface FastEthernet0/13
 switchport access vlan 60
 switchport mode access
!
interface FastEthernet0/14
 switchport access vlan 60
 switchport mode access
!
interface FastEthernet0/15
 switchport access vlan 60
 switchport mode access
!
interface FastEthernet0/16
 switchport access vlan 60
 switchport mode access
!
interface FastEthernet0/17
 switchport access vlan 60
 switchport mode access
!
interface FastEthernet0/18
 switchport access vlan 60
 switchport mode access
!
interface FastEthernet0/19
 switchport access vlan 60
 switchport mode access
!
interface FastEthernet0/20
 switchport access vlan 60
 switchport mode access
!
interface FastEthernet0/21
 switchport access vlan 60
 switchport mode access
!
interface FastEthernet0/22
 switchport access vlan 60
 switchport mode access
!
interface FastEthernet0/23
 switchport access vlan 60
 switchport mode access
!
interface FastEthernet0/24
 switchport access vlan 60
 switchport mode access
!
interface GigabitEthernet0/1
 switchport access vlan 99
 switchport mode access
!
interface GigabitEthernet0/2
 switchport access vlan 99
 switchport mode access
!
interface Vlan1
 no ip address
 shutdown
!
banner motd #NO Unauthorised Access!!!#
!
line con 0
 password 7 <redacted>
 login
!
line vty 0 4
 login
line vty 5 15
 login
!
end
```
