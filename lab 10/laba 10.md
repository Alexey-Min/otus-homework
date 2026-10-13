### Цель:
Настроить DHCP и синхронизацию времени в офисе Москва, а также NAT в офисах Москва, C.-Перетбруг и Чокурдах.

### Задание:
Настроите NAT(PAT) на R14 и R15. Трансляция должна осуществляться в адрес автономной системы AS1001.
Настроите NAT(PAT) на R18. Трансляция должна осуществляться в пул из 5 адресов автономной системы AS2042.
Настроите статический NAT для R20.
Настроите NAT так, чтобы R19 был доступен с любого узла для удаленного управления.
Настроите статический NAT(PAT) для офиса Чокурдах.
Настроите для IPv4 DHCP сервер в офисе Москва на маршрутизаторах R12 и R13. VPC1 и VPC7 должны получать сетевые настройки по DHCP.
Настроите NTP сервер на R12 и R13. Все устройства в офисе Москва должны синхронизировать время с R12 и R13.
Все офисы в лабораторной работе должны иметь IP связность.

### Общая таблица сетевых адресов на интерфейсах.

| наименование | интерфейс | ip address      | Сеть

| VPC1         |           | ip dhcp         | 172.168.1.0/24

| VPC7         |           | ip dhcp         | 172.168.2.0/24

| R12          | e0/0      | 172.168.1.1/24  | 172.168.1.0/24

| R12          | e0/2      | 172.168.3.2/24  | 172.168.3.0/24

| R12          | e0/3      | 172.168.7.2/24  | 172.168.7.0/24

| R13          | e0/1      | 172.168.2.1/24  | 172.168.1.0/24

| R13          | e0/2      | 172.168.4.2/24  | 172.168.4.0/24

| R13          | e0/3      | 172.168.6.2/24  | 172.168.6.0/24

| R14          | e0/0      | 172.168.3.1/24  | 172.168.3.0/24

| R14          | e0/1      | 172.168.6.1/24  | 172.168.6.0/24

| R14          | e0/2      | 172.168.10.2/24 | 172.168.10.0/24

| R14          | e0/3      | 172.168.8.1/24  | 172.168.8.0/24

| R15          | e0/0      | 172.168.4.1/24  | 172.168.4.0/24

| R15          | e0/1      | 172.168.7.1/24  | 172.168.7.0/24

| R15          | e0/2      | 172.168.9.2/24  | 172.168.9.0/24

| R15          | e0/3      | 172.168.5.1/24  | 172.168.5.0/24

| R19          | e0/0      | 172.168.8.2/24  | 172.168.8.0/24

| R20          | e0/0      | 172.168.5.2/24  | 172.168.5.0/24

| R21          | e0/0      | 192.168.9.1/24  | 192.168.9.0/24

| R21          | e0/1      | 192.168.11.2/24 | 192.168.11.0/24

| R21          | e0/2      | 192.168.12.1/24 | 192.168.12.0/24

| R22          | e0/0      | 192.168.10.1/24 | 192.168.10.0/24

| R22          | e0/1      | 192.168.11.1/24 | 192.168.11.0/24

| R22          | e0/2      | 192.168.13.1/24 | 192.168.13.0/24

| R24          | e0/0      | 192.168.12.2/24 | 192.168.12.0/24

| R24          | e0/1      | 172.168.17.1/24 | 172.168.17.0/24

| R24          | e0/2      | 172.168.14.2/24 | 172.168.14.0/24

| R24          | e0/3      | 192.168.18.1/24 | 192.168.18.0/24

| R26          | e0/0      | 172.168.17.2/24 | 172.168.17.0/24

| R26          | e0/1      | 192.168.26.1/24 | 192.168.26.0/24

| R26          | e0/2      | 172.168.15.2/24 | 172.168.15.0/24

| R26          | e0/3      | 192.168.19.1/24 | 192.168.19.0/24

| R23          | e0/0      | 192.168.13.2/24 | 192.168.13.0/24

| R23          | e0/1      | 172.168.16.1/24 | 172.168.16.0/24

| R23          | e0/2      | 172.168.14.1/24 | 172.168.14.0/24

| R25          | e0/0      | 172.168.16.2/24 | 172.168.16.0/24

| R25          | e0/1      | 192.168.25.1/24 | 192.168.25.0/24

| R25          | e0/2      | 172.168.15.1/24 | 172.168.15.0/24

| R26          | e0/0      | 172.168.21.1/24 | 172.168.21.0/24

| R26          | e0/1      | 172.168.20.1/24 | 172.168.20.0/24

| R26          | e0/2      | 192.168.18.2/24 | 192.168.18.0/24

| R26          | e0/3      | 192.168.19.2/24 | 192.168.19.0/24

| R17          | e0/0      | 172.168.22.1/24 | 172.168.22.0/24

| R17          | e0/1      | 172.168.20.2/24 | 172.168.20.0/24

| SW9          | e0/3      | 172.168.22.2/24 | 172.168.22.0/24

| SW9          | e1/0      | 172.168.24.2/24 | 172.168.24.0/24

| R16          | e0/1      | 172.168.21.2/24 | 172.168.21.0/24

| R16          | e0/2      | 172.168.24.1/24 | 172.168.24.0/24

| R16          | e0/3      | 172.168.23.1/24 | 172.168.23.0/24

| R32          | e0/0      | 172.168.23.2/24 | 172.168.23.0/24

| R2           | e0/0      | 192.168.25.2/24 | 192.168.25.0/24

| R2           | e0/1      | 192.168.26.2/24 | 192.168.26.0/24

| R2           | e0/2      | 172.168.26.1/24 | 172.168.26.0/24

| R2           | e0/0      | 172.168.26.2/24 | 172.168.26.0/24

| R2           | e0/1      | 172.168.27.1/24 | 172.168.27.0/24

| R2           | e0/2      | 172.168.28.1/24 | 172.168.28.0/24

| Node4        |           | 172.168.27.2/24 | 172.168.27.0/24

| Node5        |           | 172.168.28.2/24 | 172.168.28.0/24

### Пример настроек

VPC1

VPC1> ip dhcp
DORA IP 172.168.1.2/24 GW 172.168.1.1                          -------------VPC1 и VPC7 должны получать сетевые настройки по DHCP.

VPC7

VPC7> ip dhcp
DORA IP 172.168.2.2/24 GW 172.168.2.1                          -------------VPC1 и VPC7 должны получать сетевые настройки по DHCP.

R12

ip dhcp pool VPC1_pool                                         --------------Настроите для IPv4 DHCP сервер в офисе Москва на маршрутизаторах R12 и R13.
 network 172.168.1.0 255.255.255.0
 default-router 172.168.1.1
 dns-server 8.8.8.8
 lease 7
--------------------------------------------------------------
ntp master 3
ntp update-calendar                                           --------------Настроите NTP сервер на R12 и R13.


interface Ethernet0/0
 ip address 172.168.1.1 255.255.255.0
 ntp broadcast

interface Ethernet0/2
 ip address 172.168.3.2 255.255.255.0
 ip ospf 1001 area 0
 ntp broadcast
!
interface Ethernet0/3
 ip address 172.168.7.2 255.255.255.0
 ip ospf 1001 area 0
 ntp broadcast

R13

ip dhcp pool VPC7_pool                                        --------------Настроите для IPv4 DHCP сервер в офисе Москва на маршрутизаторах R12 и R13.
 network 172.168.2.0 255.255.255.0
 default-router 172.168.2.1
-------------------------------------------------------------
ntp master 3
ntp update-calendar                                          --------------Настроите NTP сервер на R12 и R13.

interface Ethernet0/1
 ip address 172.168.2.1 255.255.255.0
 ntp broadcast
!
interface Ethernet0/2
 ip address 172.168.4.2 255.255.255.0
 ip ospf 1001 area 0
 ntp broadcast
!
interface Ethernet0/3
 ip address 172.168.6.2 255.255.255.0
 ip ospf 1001 area 0
 ntp broadcast





R14

Router#sh ntp associations                                                   -----Все устройства в офисе Москва должны синхронизировать время с R12 и R13.

  address         ref clock       st   when   poll reach  delay  offset   disp
 ~172.168.3.1     .INIT.          16      -    512     0  0.000   0.000 15937.
* 172.168.3.2     127.127.1.1      3     32     64   376 -1.000  -1.500  2.890
  172.168.3.2     .INIT.          16    616     64     0  0.000   0.000 15937.
 * sys.peer, # selected, + candidate, - outlyer, x falseticker, ~ configured
-------------------------------------------------------------------------------
interface Ethernet0/0
 ip address 172.168.3.1 255.255.255.0
 ip nat inside
 ip virtual-reassembly in
 ip ospf 1001 area 0
 ntp broadcast client
!
interface Ethernet0/1
 ip address 172.168.6.1 255.255.255.0
 ip nat inside
 ip virtual-reassembly in
 ip ospf 1001 area 0
!
interface Ethernet0/2
 ip address 192.168.10.2 255.255.255.0
 ip nat outside
 ip virtual-reassembly in
!
interface Ethernet0/3
 ip address 172.168.8.1 255.255.255.0
 ip nat inside
 ip virtual-reassembly in
 ip ospf 1001 area 0
!
router ospf 1001
 router-id 1.4.0.0
!
router bgp 1001
 bgp router-id 1.4.0.0
 bgp log-neighbor-changes
 network 192.168.40.10 mask 255.255.255.255                                  ----------Настроите NAT так, чтобы R19 был доступен с любого узла для удаленного управления.
 redistribute connected
 redistribute ospf 1001 match internal external 1 external 2
 neighbor 192.168.10.1 remote-as 101

no ip http server
no ip http secure-server
ip nat pool mos14 192.168.10.2 192.168.10.2 netmask 255.255.255.0           ----------Настроите NAT(PAT) на R14 и R15. Трансляция должна осуществляться в адрес автономной системы AS1001.
ip nat inside source list 2 pool mos14 overload                             
ip nat inside source static 172.168.8.2 192.168.40.10                       ----------Настроите NAT так, чтобы R19 был доступен с любого узла для удаленного управления.
ip route 0.0.0.0 0.0.0.0 192.168.10.1
ip route 172.168.1.0 255.255.255.0 172.168.3.2
ip route 172.168.2.0 255.255.255.0 172.168.6.2
!
access-list 3 permit 172.168.4.0 0.0.0.255
access-list 3 permit 172.168.2.0 0.0.0.255
access-list 3 permit 172.168.7.0 0.0.0.255
access-list 3 permit 172.168.6.0 0.0.0.255
access-list 3 permit 172.168.1.0 0.0.0.255
access-list 3 permit 172.168.3.0 0.0.0.255

line con 0
 exec-timeout 0 0
 logging synchronous
line aux 0
line vty 0 4
 login
 transport input none
!
ntp server 172.168.3.1
------------------------------------------------------------------------------
Router#sh ip nat translations
Pro Inside global      Inside local       Outside local      Outside global
icmp 192.168.10.2:1     172.168.7.2:1      192.168.26.2:1     192.168.26.2:1
--- 192.168.40.10      172.168.8.2        ---                ---
-------------------------------------------------------------------------------
Router#sh ip nat statistics
Total active translations: 1 (1 static, 0 dynamic; 0 extended)
Peak translations: 1, occurred 00:07:01 ago
Outside interfaces:
  Ethernet0/2
Inside interfaces:
  Ethernet0/0, Ethernet0/1, Ethernet0/3
Hits: 0  Misses: 0
CEF Translated packets: 0, CEF Punted packets: 0
Expired translations: 0
Dynamic mappings:
-- Inside Source
[Id: 2] access-list 2 pool mos14 refcount 0                                   
 pool mos14: netmask 255.255.255.0
        start 192.168.10.2 end 192.168.10.2
        type generic, total addresses 1, allocated 0 (0%), misses 0

Total doors: 0
Appl doors: 0
Normal doors: 0
Queued Packets: 0


R15

Router#sh ntp associations                                                    --------Все устройства в офисе Москва должны синхронизировать время с R12 и R13.    
  address         ref clock       st   when   poll reach  delay  offset   disp
 ~172.168.4.1     .INIT.          16      -   1024     0  0.000   0.000 15937.
* 172.168.4.2     127.127.1.1      3     39     64   376  0.000  -1.000  2.888
 * sys.peer, # selected, + candidate, - outlyer, x falseticker, ~ configured
-------------------------------------------------------------------------------
interface Ethernet0/0
 ip address 172.168.4.1 255.255.255.0
 ip nat inside
 ip virtual-reassembly in
 ip ospf 1001 area 0
 ntp broadcast client
!
interface Ethernet0/1
 ip address 172.168.7.1 255.255.255.0
 ip nat inside
 ip virtual-reassembly in
 ip ospf 1001 area 0
!
interface Ethernet0/2
 ip address 192.168.9.2 255.255.255.0
 ip nat outside
 ip virtual-reassembly in
!
interface Ethernet0/3
 ip address 172.168.5.1 255.255.255.0
 ip nat inside
 ip virtual-reassembly in
 ip ospf 1001 area 0
!
router ospf 1001
 router-id 1.5.0.0
!
router bgp 1001
 bgp router-id 1.5.0.0
 bgp log-neighbor-changes
 network 192.168.50.10 mask 255.255.255.255
 redistribute connected
 redistribute ospf 1001 match internal external 1 external 2
 neighbor 192.168.9.1 remote-as 301
!
no ip http server
no ip http secure-server
ip nat pool mos 192.168.9.2 192.168.9.2 netmask 255.255.255.0
ip nat inside source list 2 pool mos overload                            -----------Настроите NAT(PAT) на R14 и R15. Трансляция должна осуществляться в адрес автономной системы AS1001.
ip nat inside source static 172.168.5.2 192.168.50.10                    -----------Настроите статический NAT для R20.                  
ip route 0.0.0.0 0.0.0.0 192.168.9.1
ip route 172.168.1.0 255.255.255.0 172.168.7.2
ip route 172.168.2.0 255.255.255.0 172.168.4.2
!
access-list 2 permit 172.168.2.0 0.0.0.255
access-list 2 permit 172.168.4.0 0.0.0.255
access-list 2 permit 172.168.1.0 0.0.0.255
access-list 2 permit 172.168.7.0 0.0.0.255
access-list 2 permit 172.168.6.0 0.0.0.255
access-list 2 permit 172.168.3.0 0.0.0.255
!
line con 0
 exec-timeout 0 0
 logging synchronous
line aux 0
line vty 0 4
 login
 transport input none
!
ntp server 172.168.4.1
----------------------------------------------------------------------------
Router#sh ip nat translations
Pro Inside global      Inside local       Outside local      Outside global
icmp 192.168.9.2:1     172.168.4.2:1      192.168.26.2:1     192.168.26.2:1
--- 192.168.50.10      172.168.5.2        ---                ---
-----------------------------------------------------------------------------
Router#sh ip nat statistics
Total active translations: 1 (1 static, 0 dynamic; 0 extended)
Peak translations: 1, occurred 00:51:45 ago
Outside interfaces:
  Ethernet0/2
Inside interfaces:
  Ethernet0/0, Ethernet0/1, Ethernet0/3
Hits: 0  Misses: 0
CEF Translated packets: 0, CEF Punted packets: 0
Expired translations: 0
Dynamic mappings:
-- Inside Source
[Id: 1] access-list 2 pool mos refcount 0                                  
 pool mos: netmask 255.255.255.0
        start 192.168.9.2 end 192.168.9.2
        type generic, total addresses 1, allocated 0 (0%), misses 0

Total doors: 0
Appl doors: 0
Normal doors: 0
Queued Packets: 0

R21

router bgp 301
 bgp router-id 2.1.0.0
 bgp log-neighbor-changes
 redistribute connected
 redistribute static                             
 neighbor 192.168.9.2 remote-as 1001
 neighbor 192.168.11.1 remote-as 101
 neighbor 192.168.12.2 remote-as 520
 neighbor 192.168.12.2 next-hop-self
-------------------------------------------------
ip route 192.168.50.0 255.255.255.255 192.168.9.2

R22

router bgp 101
 bgp router-id 2.2.0.0
 bgp log-neighbor-changes
 redistribute connected
 redistribute static                              ----------Настроите NAT так, чтобы R19 был доступен с любого узла для удаленного управления.
 neighbor 192.168.10.2 remote-as 1001
 neighbor 192.168.11.2 remote-as 301
 neighbor 192.168.11.2 next-hop-self
 neighbor 192.168.13.2 remote-as 520
------------------------------------------------
ip route 192.168.40.0 255.255.255.255 192.168.10.2----------Настроите NAT так, чтобы R19 был доступен с любого узла для удаленного управления.

R18

interface Ethernet0/0
 ip address 172.168.21.1 255.255.255.0
 ip nat inside
 ip virtual-reassembly in
 ip ospf 2042 area 0
!
interface Ethernet0/1
 ip address 172.168.20.1 255.255.255.0
 ip nat inside
 ip virtual-reassembly in
 ip ospf 2042 area 0
!
interface Ethernet0/2
 ip address 192.168.18.2 255.255.255.0
 ip nat outside
 ip virtual-reassembly in
!
interface Ethernet0/3
 ip address 192.168.19.2 255.255.255.0
 ip nat outside
 ip virtual-reassembly in
!
router ospf 2042
 router-id 1.8.0.0
!
router bgp 2042
 bgp router-id 1.8.0.0
 bgp log-neighbor-changes
 redistribute connected
 redistribute ospf 2042 match internal external 1 external 2
 neighbor 192.168.18.1 remote-as 520
 neighbor 192.168.19.1 remote-as 520
 neighbor 192.168.19.1 next-hop-self
!
no ip http server
no ip http secure-server
ip nat pool sp 192.168.18.2 192.168.18.2 netmask 255.255.255.0   ---------------------Настроите NAT(PAT) на R18. Трансляция должна осуществляться в пул из 5 адресов автономной системы AS2042.
ip nat inside source list 4 pool sp overload
ip route 0.0.0.0 0.0.0.0 192.168.18.1
!
access-list 4 permit 172.168.20.0 0.0.0.255
access-list 4 permit 172.168.22.0 0.0.0.255
access-list 4 permit 172.168.21.0 0.0.0.255
access-list 4 permit 172.168.23.0 0.0.0.255
access-list 4 permit 172.168.24.0 0.0.0.255
!
line con 0
 exec-timeout 0 0
 logging synchronous
line aux 0
line vty 0 4
 login
 transport input none
-----------------------------------------------------------------------------
Router#sh ip nat translations
Pro Inside global      Inside local       Outside local      Outside global
icmp 192.168.18.2:0    172.168.23.2:0     192.168.9.2:0      192.168.9.2:0
------------------------------------------------------------------------------
Router#sh ip nat statistics
Total active translations: 0 (0 static, 0 dynamic; 0 extended)
Peak translations: 1, occurred 00:14:06 ago
Outside interfaces:
  Ethernet0/2, Ethernet0/3
Inside interfaces:
  Ethernet0/0, Ethernet0/1
Hits: 20  Misses: 0
CEF Translated packets: 20, CEF Punted packets: 0
Expired translations: 2
Dynamic mappings:
-- Inside Source
[Id: 1] access-list 4 pool sp refcount 0
 pool sp: netmask 255.255.255.0
        start 192.168.18.2 end 192.168.18.2
        type generic, total addresses 1, allocated 0 (0%), misses 0

Total doors: 0
Appl doors: 0
Normal doors: 0
Queued Packets: 0

R2

interface Ethernet0/0
 ip address 192.168.25.2 255.255.255.0
 ip nat outside
 ip virtual-reassembly in
!
interface Ethernet0/1
 ip address 192.168.26.2 255.255.255.0
 ip nat outside
 ip virtual-reassembly in
!
interface Ethernet0/2
 ip address 172.168.26.1 255.255.255.0
 ip nat inside
 ip virtual-reassembly in
!
router bgp 202
 bgp router-id 2.0.0.0
 bgp log-neighbor-changes
 redistribute connected
 neighbor 192.168.25.1 remote-as 520
 neighbor 192.168.26.1 remote-as 520
!
no ip http server
no ip http secure-server
ip nat pool sch 192.168.25.2 192.168.25.2 netmask 255.255.255.0
ip nat inside source list 1 pool sch overload
ip nat inside source static 172.168.26.2 192.168.25.2          -----------------Настроите статический NAT(PAT) для офиса Чокурдах.
ip route 172.168.27.0 255.255.255.0 172.168.26.2
ip route 172.168.28.0 255.255.255.0 172.168.26.2
!
access-list 1 permit 172.168.26.0 0.0.0.255
access-list 1 permit 172.168.27.0 0.0.0.255
access-list 1 permit 172.168.28.0 0.0.0.255
!
line con 0
 exec-timeout 0 0
 logging synchronous
line aux 0
line vty 0 4
 login
 transport input none
----------------------------------------------------------------------------
Router#sh ip nat translations
Pro Inside global      Inside local       Outside local      Outside global
--- 192.168.25.2       172.168.26.2       ---                ---
-----------------------------------------------------------------------------
Router#sh ip nat statistics
Total active translations: 1 (1 static, 0 dynamic; 0 extended)
Peak translations: 7, occurred 00:02:39 ago
Outside interfaces:
  Ethernet0/0, Ethernet0/1
Inside interfaces:
  Ethernet0/2
Hits: 2002  Misses: 0
CEF Translated packets: 10, CEF Punted packets: 0
Expired translations: 5
Dynamic mappings:
-- Inside Source
[Id: 1] access-list 1 pool sch refcount 0
 pool sch: netmask 255.255.255.0
        start 192.168.25.2 end 192.168.25.2
        type generic, total addresses 1, allocated 0 (0%), misses 0

Total doors: 0
Appl doors: 0
Normal doors: 0
Queued Packets: 0

R32

Router#ping 192.168.9.2                                             ----------Все офисы в лабораторной работе должны иметь IP связность.
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 192.168.9.2, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 2/2/2 ms
Router#ping 192.168.40.10
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 192.168.40.10, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 2/2/4 ms
Router#ping 192.168.26.2
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 192.168.26.2, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 2/2/3 ms

### Выполнение:
Настроил NAT(PAT) на R14 и R15. Трансляция осуществляться в адрес автономной системы AS1001.
Настроил NAT(PAT) на R18. Трансляция осуществляться в пул из 5 адресов автономной системы AS2042.
Настроил статический NAT для R20.
Настроил NAT так, что R19 доступен с любого узла для удаленного управления.
Настроил статический NAT(PAT) для офиса Чокурдах.
Настроил для IPv4 DHCP сервер в офисе Москва на маршрутизаторах R12 и R13. VPC1 и VPC7 получают сетевые настройки по DHCP.
Настроил NTP сервер на R12 и R13. Все устройства в офисе Москва синхронизируют время с R12 и R13.
Все офисы в лабораторной работе имеют IP связность.









