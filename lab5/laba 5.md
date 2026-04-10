### Цель
Настроить EIGRP в С.-Петербург;
Использовать named EIGRP

### Задание
1 В офисе С.-Петербург настроить EIGRP.
2 R32 получает только маршрут по умолчанию.
3 R16-17 анонсируют только суммарные префиксы.
4 Использовать EIGRP named-mode для настройки сети.

### Общая таблица сетевых адресов на интерфейсах.
| наименование | интерфейс | ip address      | Сеть

| R18          | e0/0      | 192.168.1.10/24 | 192.168.1.0/24

| R18          | e0/1      | 172.168.1.10/24 | 172.168.1.0/24

| R17          | e0/0      | 172.168.3.10/24 | 172.168.3.0/24

| R17          | e0/1      | 172.168.1.1/24  | 172.168.1.0/24

| R17          | e0/2      | 172.168.2.10/24 | 172.168.2.0/24

| R16          | e0/0      | 192.168.3.10/24 | 192.168.3.0/24

| R16          | e0/1      | 192.168.1.1/24  | 192.168.1.0/24

| R16          | e0/2      | 192.168.2.10/24 | 192.168.2.0/24

| R16          | e0/3      | 192.168.4.10/24 | 192.168.4.0/24

| R32          | e0/0      | 192.168.4.1/24  | 192.168.4.0/24

| SW9          | e0/0      | 40.40.40.1/24   | 40.40.40.0/24

| SW9          | e0/1      | 30.30.30.1/24   | 30.30.30.0/24

| SW9          | e0/2      | 10.10.10.10/24  | 10.10.10.0/24

| SW9          | e0/3      | 172.168.3.1/24  | 172.168.3.0/24

| SW9          | e1/0      | 192.168.2.1/24  | 192.168.2.0/24

| SW10         | e0/0      | 40.40.40.2/24   | 40.40.40.0/24

| SW10         | e0/1      | 30.30.30.2/24   | 30.30.30.0/24

| SW10         | e0/2      | 20.20.20.10/24  | 20.20.20.0/24

| SW10         | e0/3      | 192.168.3.1/24  | 192.168.3.0/24

| SW10         | e1/0      | 172.168.2.1/24  | 172.168.2.0/24

| VPC1         | e0/0      | 10.10.10.1/24   | 10.10.10.0/24

| VPC2         | e0/0      | 20.20.20.1/24   | 20.20.20.0/24


### Пример настроек
VPC 1

set pcname VPC 1

ip add ress 10.10.10.1 255.255.255.0  gateway 10.10.10.10
-----------------------------------------------
VPC1> ping 20.20.20.1

84 bytes from 20.20.20.1 icmp_seq=1 ttl=62 time=1.574 ms
84 bytes from 20.20.20.1 icmp_seq=2 ttl=62 time=3.514 ms
84 bytes from 20.20.20.1 icmp_seq=3 ttl=62 time=2.545 ms
84 bytes from 20.20.20.1 icmp_seq=4 ttl=62 time=1.277 ms
84 bytes from 20.20.20.1 icmp_seq=5 ttl=62 time=1.213 ms

SW9

Switch#sh run | s ei
router eigrp name
 !
 address-family ipv4 unicast autonomous-system 2042
  !
  topology base
  exit-af-topology
  network 10.0.0.0
  network 30.0.0.0
  network 40.0.0.0
  network 172.168.0.0
  network 192.168.2.0
  eigrp router-id 0.0.0.9
 exit-address-family
---------------------------------------------------
Switch#sh ip ei nei
EIGRP-IPv4 VR(name) Address-Family Neighbors for AS(2042)
H   Address                 Interface              Hold Uptime   SRTT   RTO  Q  Seq
                                                   (sec)         (ms)       Cnt Num
3   40.40.40.2              Et0/0                    14 00:50:42   10   100  0  10
2   30.30.30.2              Et0/1                    10 00:50:42   10   100  0  9
1   172.168.3.10            Et0/3                    11 00:50:42    9   100  0  12
0   192.168.2.10            Et1/0                    13 00:50:42   15   100  0  16
---------------------------------------------------
Switch#sh ip ei top
EIGRP-IPv4 VR(name) Topology Table for AS(2042)/ID(0.0.0.9)
Codes: P - Passive, A - Active, U - Update, Q - Query, R - Reply,
       r - reply Status, s - sia Status

P 192.168.3.0/24, 3 successors, FD is 196608000
        via 30.30.30.2 (196608000/131072000), Ethernet0/1
        via 40.40.40.2 (196608000/131072000), Ethernet0/0
        via 192.168.2.10 (196608000/131072000), Ethernet1/0
P 192.168.2.0/24, 1 successors, FD is 131072000
        via Connected, Ethernet1/0
P 40.40.40.0/24, 1 successors, FD is 131072000
        via Connected, Ethernet0/0
P 192.168.0.0/22, 1 successors, FD is 327680000
        via 172.168.3.10 (327680000/262144000), Ethernet0/3
P 192.168.1.0/24, 1 successors, FD is 196608000
        via 192.168.2.10 (196608000/131072000), Ethernet1/0
P 30.30.30.0/24, 1 successors, FD is 131072000
        via Connected, Ethernet0/1
P 172.168.2.0/24, 3 successors, FD is 196608000
        via 30.30.30.2 (196608000/131072000), Ethernet0/1
        via 40.40.40.2 (196608000/131072000), Ethernet0/0
        via 172.168.3.10 (196608000/131072000), Ethernet0/3
P 20.20.20.0/24, 2 successors, FD is 196608000
        via 30.30.30.2 (196608000/131072000), Ethernet0/1
        via 40.40.40.2 (196608000/131072000), Ethernet0/0
P 172.168.3.0/24, 1 successors, FD is 131072000
        via Connected, Ethernet0/3
P 172.168.1.0/24, 1 successors, FD is 196608000
        via 172.168.3.10 (196608000/131072000), Ethernet0/3
P 172.168.0.0/22, 1 successors, FD is 327680000
        via 192.168.2.10 (327680000/262144000), Ethernet1/0
P 10.10.10.0/24, 1 successors, FD is 131072000
        via Connected, Ethernet0/2

R17
Router#sh ip ei nei
EIGRP-IPv4 VR(name) Address-Family Neighbors for AS(2042)
H   Address                 Interface              Hold Uptime   SRTT   RTO  Q  Seq
                                                   (sec)         (ms)       Cnt Num
2   172.168.3.1             Et0/0                    13 00:44:25    4   100  0  9
1   172.168.2.1             Et0/2                    14 00:44:28    1   100  0  12
0   172.168.1.10            Et0/1                    12 00:44:36 1015  5000  0  14
----------------------------------------------------------
Router#sh run | s ei
router eigrp name
 !
 address-family ipv4 unicast autonomous-system 2042
  !
  af-interface Ethernet0/1
   summary-address 172.168.0.0 255.255.252.0
  exit-af-interface
  !
  topology base
  exit-af-topology
  network 172.168.0.0
  eigrp router-id 0.0.0.17
 exit-address-family
--------------------------------------------------------
Router#sh ip ei top
EIGRP-IPv4 VR(name) Topology Table for AS(2042)/ID(0.0.0.17)
Codes: P - Passive, A - Active, U - Update, Q - Query, R - Reply,
       r - reply Status, s - sia Status

P 192.168.3.0/24, 1 successors, FD is 196608000
        via 172.168.2.1 (196608000/131072000), Ethernet0/2
P 192.168.2.0/24, 1 successors, FD is 196608000
        via 172.168.3.1 (196608000/131072000), Ethernet0/0
P 40.40.40.0/24, 2 successors, FD is 196608000
        via 172.168.2.1 (196608000/131072000), Ethernet0/2
        via 172.168.3.1 (196608000/131072000), Ethernet0/0
P 192.168.0.0/22, 1 successors, FD is 262144000
        via 172.168.1.10 (262144000/196608000), Ethernet0/1
P 192.168.1.0/24, 1 successors, FD is 196608000
        via 172.168.1.10 (196608000/131072000), Ethernet0/1
P 30.30.30.0/24, 2 successors, FD is 196608000
        via 172.168.2.1 (196608000/131072000), Ethernet0/2
        via 172.168.3.1 (196608000/131072000), Ethernet0/0
P 172.168.2.0/24, 1 successors, FD is 131072000
        via Connected, Ethernet0/2
P 20.20.20.0/24, 1 successors, FD is 196608000
        via 172.168.2.1 (196608000/131072000), Ethernet0/2
P 172.168.3.0/24, 1 successors, FD is 131072000
        via Connected, Ethernet0/0
P 172.168.1.0/24, 1 successors, FD is 131072000
        via Connected, Ethernet0/1
### P 172.168.0.0/22, 1 successors, FD is 131072000
###        via Summary (131072000/0), Null0
P 10.10.10.0/24, 1 successors, FD is 196608000
        via 172.168.3.1 (196608000/131072000), Ethernet0/0

R16
Router#sh run | s ei
router eigrp name
 !
 address-family ipv4 unicast autonomous-system 2042
  !
  af-interface Ethernet0/1
   summary-address 192.168.0.0 255.255.252.0
  exit-af-interface
  !
  topology base
  exit-af-topology
  network 192.168.1.0
  network 192.168.2.0
  network 192.168.3.0
  eigrp router-id 0.0.0.16
 exit-address-family
-------------------------------------------------------
Router#sh ip ei nei
EIGRP-IPv4 VR(name) Address-Family Neighbors for AS(2042)
H   Address                 Interface              Hold Uptime   SRTT   RTO  Q  Seq
                                                   (sec)         (ms)       Cnt Num
2   192.168.2.1             Et0/2                    14 00:59:17   16   100  0  12
1   192.168.3.1             Et0/0                    11 00:59:20    6   100  0  11
0   192.168.1.10            Et0/1                    13 00:59:27  658  3948  0  15
-------------------------------------------------------
Router#sh ip ei top
EIGRP-IPv4 VR(name) Topology Table for AS(2042)/ID(0.0.0.16)
Codes: P - Passive, A - Active, U - Update, Q - Query, R - Reply,
       r - reply Status, s - sia Status

P 192.168.3.0/24, 1 successors, FD is 131072000
        via Connected, Ethernet0/0
P 192.168.2.0/24, 1 successors, FD is 131072000
        via Connected, Ethernet0/2
P 40.40.40.0/24, 2 successors, FD is 196608000
        via 192.168.2.1 (196608000/131072000), Ethernet0/2
        via 192.168.3.1 (196608000/131072000), Ethernet0/0
### P 192.168.0.0/22, 1 successors, FD is 131072000
###        via Summary (131072000/0), Null0
P 192.168.1.0/24, 1 successors, FD is 131072000
        via Connected, Ethernet0/1
P 30.30.30.0/24, 2 successors, FD is 196608000
        via 192.168.2.1 (196608000/131072000), Ethernet0/2
        via 192.168.3.1 (196608000/131072000), Ethernet0/0
P 172.168.2.0/24, 1 successors, FD is 196608000
        via 192.168.3.1 (196608000/131072000), Ethernet0/0
P 20.20.20.0/24, 1 successors, FD is 196608000
        via 192.168.3.1 (196608000/131072000), Ethernet0/0
P 172.168.3.0/24, 1 successors, FD is 196608000
        via 192.168.2.1 (196608000/131072000), Ethernet0/2
P 172.168.1.0/24, 1 successors, FD is 196608000
        via 192.168.1.10 (196608000/131072000), Ethernet0/1
P 172.168.0.0/22, 1 successors, FD is 262144000
        via 192.168.1.10 (262144000/196608000), Ethernet0/1
P 10.10.10.0/24, 1 successors, FD is 196608000
        via 192.168.2.1 (196608000/131072000), Ethernet0/2

R18
Router#sh run | s ei
router eigrp name
 !
 address-family ipv4 unicast autonomous-system 2042
  !
  topology base
  exit-af-topology
  network 172.168.0.0
  network 192.168.1.0
  eigrp router-id 0.0.0.18
 exit-address-family
--------------------------------------------------------
Router#sh ip ei nei
EIGRP-IPv4 VR(name) Address-Family Neighbors for AS(2042)
H   Address                 Interface              Hold Uptime   SRTT   RTO  Q  Seq
                                                   (sec)         (ms)       Cnt Num
1   192.168.1.1             Et0/0                    14 01:02:19    4   100  0  15
0   172.168.1.1             Et0/1                    13 01:02:20    9   100  0  11

R32
Router#sh ip route
Gateway of last resort is 192.168.4.10 to network 0.0.0.0

S*    0.0.0.0/0 [1/0] via 192.168.4.10
      192.168.4.0/24 is variably subnetted, 2 subnets, 2 masks
C        192.168.4.0/24 is directly connected, Ethernet0/0
L        192.168.4.1/32 is directly connected, Ethernet0/0
----------------------------------------------------------
Router#ping 10.10.10.1
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 10.10.10.1, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 1/1/2 ms

### Выполнение:
1 В офисе С.-Петербург настроено EIGRP.
2 R32 получает только маршрут по умолчанию.
3 R16-17 анонсируют только суммарные префиксы.
4 Используется EIGRP named-mode для настройки сети.
