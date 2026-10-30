### Цель:
Настроить BGP free core в офисах Москвы и Санкт-Петербурга.

### Задание:
Настроите BGP free core в офисе Москвы.
Настроите BGP free core в офисе Санкт-Петербурга.

### Общая таблица сетевых адресов на интерфейсах.

| наименование | интерфейс | ip address      | Сеть

| R19          | e0/0      | 172.168.3.1/24  | 172.168.3.0/24

| R19          | lo0       | 1.9.0.0/32      | 1.9.0.0/32 

| R13          | e0/2      | 172.168.2.1/24  | 172.168.2.0/24

| R13          | e0/3      | 172.168.5.1/24  | 172.168.5.0/24

| R13          | lo0       | 1.3.0.0/32      | 1.3.0.0/32 

| R12          | e0/2      | 172.168.6.1/24  | 172.168.6.0/24

| R12          | e0/3      | 172.168.1.2/24  | 172.168.1.0/24

| R12          | lo0       | 1.2.0.0/32      | 1.2.0.0/32 

| R20          | e0/0      | 172.168.4.1/24  | 172.168.4.0/24

| R20          | lo0       | 2.0.0.0/32      | 2.0.0.0/32 

| R15          | e0/0      | 172.168.2.2/24  | 172.168.2.0/24

| R15          | e0/1      | 172.168.1.1/24  | 172.168.1.0/24

| R15          | e0/2      | 192.168.1.1/24  | 192.168.1.0/24

| R15          | e0/3      | 172.168.4.2/24  | 172.168.4.0/24

| R15          | lo0       | 1.5.0.0/32      | 1.5.0.0/32 

| R14          | e0/0      | 172.168.6.2/24  | 172.168.6.0/24

| R14          | e0/1      | 172.168.5.2/24  | 172.168.5.0/24

| R14          | e0/2      | 192.168.4.1/24  | 192.168.4.0/24

| R14          | e0/3      | 172.168.3.2/24  | 172.168.3.0/24

| R14          | lo0       | 1.4.0.0/32      | 1.4.0.0/32 

| R18          | e0/0      | 172.168.7.1/24  | 172.168.7.0/24

| R18          | e0/1      | 172.168.8.1/24  | 172.168.8.0/24

| R18          | e0/2      | 192.168.3.2/24  | 192.168.3.0/24

| R18          | lo0       | 1.8.0.0/32      | 1.8.0.0/32 

| R17          | e0/0      | 172.168.10.1/24 | 172.168.10.0/24

| R17          | e0/1      | 172.168.8.2/24  | 172.168.8.0/24

| R17          | lo0       | 1.7.0.0/32      | 1.7.0.0/32 

| sw9          | e0/3      | 172.168.10.2/24 | 172.168.10.0/24

| sw9          | e1/0      | 172.168.11.1/24 | 172.168.11.0/24

| sw9          | lo0       | 9.0.0.0/32      | 9.0.0.0/32 

| R16          | e0/0      | 172.168.7.2/24  | 172.168.7.0/24

| R16          | e0/1      | 172.168.11.2/24  | 172.168.11.0/24

| R16          | e0/2      | 192.168.9.1/24  | 192.168.9.0/24

| R16          | lo0       | 1.6.0.0/32      | 1.6.0.0/32 

| R32          | e0/0      | 172.168.9.2/24  | 172.168.9.0/24

| R32          | lo0       | 3.2.0.0/32      | 3.2.0.0/32 


### Пример настроек:

R15

mpls label protocol ldp                            ---------------------Настроите BGP free core в офисе Москвы.

interface Loopback0
 ip address 1.5.0.0 255.255.255.255
 ip ospf network point-to-point
!
interface Ethernet0/0
 ip address 172.168.2.2 255.255.255.0
 mpls ip
!
interface Ethernet0/1
 ip address 172.168.1.1 255.255.255.0
 mpls ip
!
interface Ethernet0/2
 ip address 192.168.1.1 255.255.255.0
!
interface Ethernet0/3
 ip address 172.168.4.2 255.255.255.0
 mpls ip
!
router ospf 1
 router-id 1.5.0.0
 network 1.5.0.0 0.0.0.0 area 0
 network 172.168.1.0 0.0.0.3 area 0
 network 172.168.2.0 0.0.0.3 area 0
 network 172.168.4.0 0.0.0.3 area 0
!
router bgp 1001
 bgp router-id 1.5.0.0
 bgp log-neighbor-changes
 neighbor 1.2.0.0 remote-as 1001
 neighbor 1.2.0.0 update-source Loopback0
 neighbor 1.3.0.0 remote-as 1001
 neighbor 1.3.0.0 update-source Loopback0
 neighbor 2.0.0.0 remote-as 1001
 neighbor 2.0.0.0 update-source Loopback0
 neighbor 192.168.1.2 remote-as 301
!
mpls ldp router-id Loopback0

line con 0
 exec-timeout 0 0
 logging synchronous
line aux 0
line vty 0 4
 login
 transport input none
-----------------------------------------------
Router#sh mpls ldp neighbor
    Peer LDP Ident: 2.0.0.0:0; Local LDP Ident 1.5.0.0:0
        TCP connection: 2.0.0.0.25095 - 1.5.0.0.646
        State: Oper; Msgs sent/rcvd: 46/45; Downstream
        Up time: 00:26:29
        LDP discovery sources:
          Ethernet0/3, Src IP addr: 172.168.4.1
        Addresses bound to peer LDP Ident:
          172.168.4.1     2.0.0.0
    Peer LDP Ident: 1.2.0.0:0; Local LDP Ident 1.5.0.0:0
        TCP connection: 1.2.0.0.646 - 1.5.0.0.60430
        State: Oper; Msgs sent/rcvd: 44/45; Downstream
        Up time: 00:26:17
        LDP discovery sources:
          Ethernet0/1, Src IP addr: 172.168.1.2
        Addresses bound to peer LDP Ident:
          172.168.6.1     1.2.0.0         172.168.1.2
    Peer LDP Ident: 1.3.0.0:0; Local LDP Ident 1.5.0.0:0
        TCP connection: 1.3.0.0.646 - 1.5.0.0.18828
        State: Oper; Msgs sent/rcvd: 45/45; Downstream
        Up time: 00:26:17
        LDP discovery sources:
          Ethernet0/0, Src IP addr: 172.168.2.1
        Addresses bound to peer LDP Ident:
          172.168.2.1     1.3.0.0         172.168.5.1
--------------------------------------------------------------
Router#sh mpls forwarding-table                                     ---------------------Настроите BGP free core в офисе Москвы.
Local      Outgoing   Prefix           Bytes Label   Outgoing   Next Hop
Label      Label      or Tunnel Id     Switched      interface
20         Pop Label  2.0.0.0/32       0             Et0/3      172.168.4.1
21         19         1.9.0.0/32       0             Et0/1      172.168.1.2
           22         1.9.0.0/32       0             Et0/0      172.168.2.1
22         21         1.4.0.0/32       0             Et0/1      172.168.1.2
           23         1.4.0.0/32       0             Et0/0      172.168.2.1
23         Pop Label  1.3.0.0/32       0             Et0/0      172.168.2.1
24         Pop Label  1.2.0.0/32       0             Et0/1      172.168.1.2
25         24         172.168.3.0/24   0             Et0/1      172.168.1.2
           26         172.168.3.0/24   0             Et0/0      172.168.2.1
26         Pop Label  172.168.5.0/24   0             Et0/0      172.168.2.1
27         Pop Label  172.168.6.0/24   0             Et0/1      172.168.1.2

R14

mpls label protocol ldp                          ---------------------Настроите BGP free core в офисе Москвы.

interface Loopback0
 ip address 1.4.0.0 255.255.255.255
 ip ospf network point-to-point
!
interface Ethernet0/0
 ip address 172.168.6.2 255.255.255.0
 mpls ip
!
interface Ethernet0/1
 ip address 172.168.5.2 255.255.255.0
 mpls ip
!
interface Ethernet0/2
 ip address 192.168.4.1 255.255.255.0
!
interface Ethernet0/3
 ip address 172.168.3.2 255.255.255.0
 mpls ip
!
router ospf 1
 router-id 1.4.0.0
 network 1.4.0.0 0.0.0.0 area 0
 network 172.168.3.0 0.0.0.3 area 0
 network 172.168.5.0 0.0.0.3 area 0
 network 172.168.6.0 0.0.0.3 area 0
!
router bgp 1001
 bgp router-id 1.4.0.0
 bgp log-neighbor-changes
 neighbor 1.2.0.0 remote-as 1001
 neighbor 1.2.0.0 update-source Loopback0
 neighbor 1.3.0.0 remote-as 1001
 neighbor 1.3.0.0 update-source Loopback0
 neighbor 1.9.0.0 remote-as 1001
 neighbor 1.9.0.0 update-source Loopback0
 neighbor 192.168.4.2 remote-as 101

mpls ldp router-id Loopback0

line con 0
 exec-timeout 0 0
 logging synchronous
line aux 0
line vty 0 4
 login
 transport input none
----------------------------------------------------------
Router#sh mpls ldp neighbor
    Peer LDP Ident: 1.9.0.0:0; Local LDP Ident 1.4.0.0:0
        TCP connection: 1.9.0.0.27571 - 1.4.0.0.646
        State: Oper; Msgs sent/rcvd: 51/49; Downstream
        Up time: 00:30:52
        LDP discovery sources:
          Ethernet0/3, Src IP addr: 172.168.3.1
        Addresses bound to peer LDP Ident:
          172.168.3.1     1.9.0.0
    Peer LDP Ident: 1.2.0.0:0; Local LDP Ident 1.4.0.0:0
        TCP connection: 1.2.0.0.646 - 1.4.0.0.13188
        State: Oper; Msgs sent/rcvd: 51/49; Downstream
        Up time: 00:30:42
        LDP discovery sources:
          Ethernet0/0, Src IP addr: 172.168.6.1
        Addresses bound to peer LDP Ident:
          172.168.6.1     1.2.0.0         172.168.1.2
    Peer LDP Ident: 1.3.0.0:0; Local LDP Ident 1.4.0.0:0
        TCP connection: 1.3.0.0.646 - 1.4.0.0.60062
        State: Oper; Msgs sent/rcvd: 49/50; Downstream
        Up time: 00:30:36
        LDP discovery sources:
          Ethernet0/1, Src IP addr: 172.168.5.1
        Addresses bound to peer LDP Ident:
          172.168.2.1     1.3.0.0         172.168.5.1
-----------------------------------------------------------
Router#sh mpls forwarding-table                                             ---------------------Настроите BGP free core в офисе Москвы.
Local      Outgoing   Prefix           Bytes Label   Outgoing   Next Hop
Label      Label      or Tunnel Id     Switched      interface
20         Pop Label  1.9.0.0/32       0             Et0/3      172.168.3.1
21         18         2.0.0.0/32       0             Et0/1      172.168.5.1
           18         2.0.0.0/32       0             Et0/0      172.168.6.1
22         19         1.5.0.0/32       0             Et0/1      172.168.5.1
           20         1.5.0.0/32       0             Et0/0      172.168.6.1
23         Pop Label  1.3.0.0/32       0             Et0/1      172.168.5.1
24         Pop Label  1.2.0.0/32       0             Et0/0      172.168.6.1
25         21         172.168.4.0/24   0             Et0/1      172.168.5.1
           26         172.168.4.0/24   0             Et0/0      172.168.6.1
26         Pop Label  172.168.1.0/24   0             Et0/0      172.168.6.1
27         Pop Label  172.168.2.0/24   0             Et0/1      172.168.5.1

R19

mpls label protocol ldp                             ---------------------Настроите BGP free core в офисе Москвы.

interface Loopback0
 ip address 1.9.0.0 255.255.255.255
 ip ospf network point-to-point
!
interface Ethernet0/0
 ip address 172.168.3.1 255.255.255.0
 mpls ip
!
interface Ethernet0/1
 no ip address
 shutdown
!
interface Ethernet0/2
 no ip address
 shutdown
!
interface Ethernet0/3
 no ip address
 shutdown
!
router ospf 1
 router-id 1.9.0.0
 network 1.9.0.0 0.0.0.0 area 0
 network 172.168.3.0 0.0.0.3 area 0

mpls ldp router-id Loopback0

line con 0
 exec-timeout 0 0
 logging synchronous
line aux 0
line vty 0 4
 login
 transport input none
----------------------------------------------------------
Router#sh mpls ldp neighbor
    Peer LDP Ident: 1.4.0.0:0; Local LDP Ident 1.9.0.0:0
        TCP connection: 1.4.0.0.646 - 1.9.0.0.27571
        State: Oper; Msgs sent/rcvd: 53/55; Downstream
        Up time: 00:34:03
        LDP discovery sources:
          Ethernet0/0, Src IP addr: 172.168.3.2
        Addresses bound to peer LDP Ident:
          172.168.6.2     1.4.0.0         172.168.5.2     192.168.4.1
          172.168.3.2
--------------------------------------------------------------
Router#sh mpls forwarding-table                                            ---------------------Настроите BGP free core в офисе Москвы.
Local      Outgoing   Prefix           Bytes Label   Outgoing   Next Hop
Label      Label      or Tunnel Id     Switched      interface
17         Pop Label  1.4.0.0/32       0             Et0/0      172.168.3.2
18         Pop Label  172.168.6.0/24   0             Et0/0      172.168.3.2
19         Pop Label  172.168.5.0/24   0             Et0/0      172.168.3.2
20         21         2.0.0.0/32       0             Et0/0      172.168.3.2
21         22         1.5.0.0/32       0             Et0/0      172.168.3.2
22         23         1.3.0.0/32       0             Et0/0      172.168.3.2
23         24         1.2.0.0/32       0             Et0/0      172.168.3.2
24         25         172.168.4.0/24   0             Et0/0      172.168.3.2
25         26         172.168.1.0/24   0             Et0/0      172.168.3.2
26         27         172.168.2.0/24   0             Et0/0      172.168.3.2

R18

mpls label protocol ldp                                    ------------Настроите BGP free core в офисе Санкт-Петербурга.

interface Loopback0
 ip address 1.8.0.0 255.255.255.255
 ip ospf network point-to-point
!
interface Ethernet0/0
 ip address 172.168.7.1 255.255.255.0
 mpls ip
!
interface Ethernet0/1
 ip address 172.168.8.1 255.255.255.0
 mpls ip
!
interface Ethernet0/2
 ip address 192.168.3.2 255.255.255.0
!
interface Ethernet0/3
 no ip address
 shutdown
!
router ospf 1
 router-id 1.8.0.0
 network 1.8.0.0 0.0.0.0 area 0
 network 172.168.7.0 0.0.0.3 area 0
 network 172.168.8.0 0.0.0.3 area 0
!
router bgp 2042
 bgp router-id 1.8.0.0
 bgp log-neighbor-changes
 neighbor 1.6.0.0 remote-as 2042
 neighbor 1.6.0.0 update-source Loopback0
 neighbor 1.7.0.0 remote-as 2042
 neighbor 1.7.0.0 update-source Loopback0
 neighbor 192.168.3.1 remote-as 520

mpls ldp router-id Loopback0

line con 0
 exec-timeout 0 0
 logging synchronous
line aux 0
line vty 0 4
 login
 transport input none
---------------------------------------------------------------------
Router#sh mpls ldp neighbor
    Peer LDP Ident: 1.7.0.0:0; Local LDP Ident 1.8.0.0:0
        TCP connection: 1.7.0.0.646 - 1.8.0.0.62142
        State: Oper; Msgs sent/rcvd: 56/55; Downstream
        Up time: 00:37:22
        LDP discovery sources:
          Ethernet0/1, Src IP addr: 172.168.8.2
        Addresses bound to peer LDP Ident:
          172.168.10.1    1.7.0.0         172.168.8.2
    Peer LDP Ident: 1.6.0.0:0; Local LDP Ident 1.8.0.0:0
        TCP connection: 1.6.0.0.646 - 1.8.0.0.48271
        State: Oper; Msgs sent/rcvd: 56/55; Downstream
        Up time: 00:37:07
        LDP discovery sources:
          Ethernet0/0, Src IP addr: 172.168.7.2
        Addresses bound to peer LDP Ident:
          172.168.7.2     1.6.0.0         172.168.11.2    172.168.9.1
--------------------------------------------------------------------------
Router#sh mpls forwarding-table                                            ------------Настроите BGP free core в офисе Санкт-Петербурга.
Local      Outgoing   Prefix           Bytes Label   Outgoing   Next Hop
Label      Label      or Tunnel Id     Switched      interface
19         Pop Label  1.7.0.0/32       0             Et0/1      172.168.8.2
20         Pop Label  172.168.10.0/24  0             Et0/1      172.168.8.2
21         Pop Label  1.6.0.0/32       0             Et0/0      172.168.7.2
22         Pop Label  172.168.11.0/24  0             Et0/0      172.168.7.2
23         Pop Label  172.168.9.0/24   0             Et0/0      172.168.7.2
24         23         3.2.0.0/32       0             Et0/0      172.168.7.2
25         24         9.0.0.0/32       0             Et0/0      172.168.7.2
           24         9.0.0.0/32       0             Et0/1      172.168.8.2

R17

mpls label protocol ldp                              ------------Настроите BGP free core в офисе Санкт-Петербурга.

interface Loopback0
 ip address 1.7.0.0 255.255.255.255
 ip ospf network point-to-point
!
interface Ethernet0/0
 ip address 172.168.10.1 255.255.255.0
 mpls ip
!
interface Ethernet0/1
 ip address 172.168.8.2 255.255.255.0
 mpls ip
!
interface Ethernet0/2
 no ip address
 shutdown
!
interface Ethernet0/3
 no ip address
 shutdown
!
router ospf 1
 router-id 1.7.0.0
 network 1.7.0.0 0.0.0.0 area 0
 network 172.168.8.0 0.0.0.3 area 0
 network 172.168.10.0 0.0.0.3 area 0

mpls ldp router-id Loopback0

line con 0
 exec-timeout 0 0
 logging synchronous
line aux 0
line vty 0 4
 login
 transport input none
--------------------------------------------------------
Router#sh mpls ldp neighbor
    Peer LDP Ident: 1.8.0.0:0; Local LDP Ident 1.7.0.0:0
        TCP connection: 1.8.0.0.62142 - 1.7.0.0.646
        State: Oper; Msgs sent/rcvd: 58/59; Downstream
        Up time: 00:40:02
        LDP discovery sources:
          Ethernet0/1, Src IP addr: 172.168.8.1
        Addresses bound to peer LDP Ident:
          172.168.7.1     1.8.0.0         172.168.8.1     192.168.3.2
    Peer LDP Ident: 9.0.0.0:0; Local LDP Ident 1.7.0.0:0
        TCP connection: 9.0.0.0.34049 - 1.7.0.0.646
        State: Oper; Msgs sent/rcvd: 57/57; Downstream
        Up time: 00:39:27
        LDP discovery sources:
          Ethernet0/0, Src IP addr: 172.168.10.2
        Addresses bound to peer LDP Ident:
          172.168.10.2    9.0.0.0         172.168.11.1
----------------------------------------------------------
Router#sh mpls forwarding-table                                            ------------Настроите BGP free core в офисе Санкт-Петербурга.
Local      Outgoing   Prefix           Bytes Label   Outgoing   Next Hop
Label      Label      or Tunnel Id     Switched      interface
18         Pop Label  1.8.0.0/32       0             Et0/1      172.168.8.1
19         Pop Label  172.168.7.0/24   0             Et0/1      172.168.8.1
20         24         3.2.0.0/32       0             Et0/1      172.168.8.1
           18         3.2.0.0/32       0             Et0/0      172.168.10.2
21         21         1.6.0.0/32       0             Et0/1      172.168.8.1
           21         1.6.0.0/32       0             Et0/0      172.168.10.2
22         Pop Label  172.168.11.0/24  0             Et0/0      172.168.10.2
23         23         172.168.9.0/24   0             Et0/1      172.168.8.1
           23         172.168.9.0/24   0             Et0/0      172.168.10.2
24         Pop Label  9.0.0.0/32       0             Et0/0      172.168.10.2

### Выполнение:

Настроил BGP free core в офисе Москвы.
Настроил BGP free core в офисе Санкт-Петербурга.





