### Цель:
Настроить iBGP в офисе Москва
Настроить iBGP в сети провайдера Триада
Организовать полную IP связанность всех сетей

### Задание
Настроить iBGP в офисом Москва между маршрутизаторами R14 и R15.
Настроить iBGP в провайдере Триада, с использованием RR.
Настройть офиса Москва так, чтобы приоритетным провайдером стал Ламас.
Настройть офиса С.-Петербург так, чтобы трафик до любого офиса распределялся по двум линкам одновременно.
Все сети в лабораторной работе должны иметь IP связность.

### Общая таблица сетевых адресов на интерфейсах.
| наименование | интерфейс | ip address      | Сеть

| R12          | e0/2      | 172.168.7.1/24  | 172.168.7.0/24

| R12          | e0/3      | 172.168.10.1/24 | 172.168.10.0/24

| R13          | e0/2      | 172.168.8.1/24  | 172.168.8.0/24

| R13          | e0/3      | 172.168.9.1/24  | 172.168.9.0/24

| R14          | e0/0      | 172.168.7.10/24 | 172.168.7.0/24

| R14          | e0/1      | 172.168.9.10/24 | 172.168.9.0/24

| R14          | e0/2      | 172.168.11.1/24 | 172.168.11.0/24

| R14          | e0/3      | 172.168.6.1/24  | 172.168.6.0/24

| R15          | e0/0      | 172.168.8.10/24 | 172.168.8.0/24

| R15          | e0/1      | 172.168.10.10/24| 172.168.10.0/24

| R15          | e0/2      | 172.168.15.1/24 | 172.168.15.0/24

| R15          | e0/3      | 172.168.6.2/24  | 172.168.6.0/24

| R22          | e0/0      | 172.168.11.10/24| 172.168.11.0/24

| R22          | e0/1      | 172.168.12.10/24| 172.168.12.0/24

| R22          | e0/2      | 172.168.13.1/24 | 172.168.13.0/24

| R21          | e0/0      | 172.168.15.10/24| 172.168.15.0/24

| R21          | e0/1      | 172.168.12.1/24 | 172.168.12.0/24

| R21          | e0/2      | 172.168.14.1/24 | 172.168.14.0/24

| R24          | e0/0      | 172.168.14.2/24 | 172.168.14.0/24

| R24          | e0/1      | 40.40.40.1/24   | 40.40.40.0/24

| R24          | e0/2      | 10.10.10.1/24   | 10.10.10.0/24

| R24          | e0/3      | 192.168.12.10/24| 192.168.12.0/24

| R26          | e0/0      | 40.40.40.2/24   | 40.40.40.0/24

| R26          | e0/2      | 30.30.30.1/24   | 30.30.30.0/24 

| R26          | e0/3      | 192.168.13.10/24| 192.168.13.0/24

| R23          | e0/0      | 172.168.13.2/24 | 172.168.13.0/24

| R23          | e0/1      | 20.20.20.1/24   | 20.20.20.0/24

| R23          | e0/2      | 10.10.10.10/24  | 10.10.10.0/24

| R25          | e0/0      | 20.20.20.2/24   | 20.20.20.0/24

| R25          | e0/2      | 30.30.30.10/24  | 30.30.30.0/24

| R18          | e0/0      | 192.168.11.10/24| 192.168.11.0/24

| R18          | e0/1      | 192.168.10.10/24| 192.168.10.0/24

| R18          | e0/2      | 192.168.12.1/24 | 192.168.12.0/24

| R18          | e0/3      | 192.168.13.1/24 | 192.168.13.0/24

| R17          | e0/1      | 192.168.10.1/24 | 192.168.10.0/24

| R17          | e0/3      | 192.168.7.1/24  | 192.168.7.0/24

| R16          | e0/1      | 192.168.11.1/24 | 192.168.11.0/24

| R16          | e0/3      | 172.168.7.2/24  | 172.168.7.0/24


### Пример настроек

R12

Router#sh run | s bgp
router bgp 1001
 bgp log-neighbor-changes
 network 172.168.7.0 mask 255.255.255.0
 network 172.168.10.0 mask 255.255.255.0
 neighbor 172.168.7.10 remote-as 1001
 neighbor 172.168.10.10 remote-as 1001

R13

Router#sh run | s bgp
router bgp 1001
 bgp log-neighbor-changes
 network 172.168.8.0 mask 255.255.255.0
 network 172.168.9.0 mask 255.255.255.0
 neighbor 172.168.8.10 remote-as 1001
 neighbor 172.168.9.10 remote-as 1001

R14

Router#sh run | s bgp
router bgp 1001
 bgp log-neighbor-changes
 network 172.168.6.0 mask 255.255.255.0
 network 172.168.9.0 mask 255.255.255.0
 neighbor 172.168.6.2 remote-as 1001
 neighbor 172.168.6.2 route-map TUDA out
 neighbor 172.168.7.1 remote-as 1001
 neighbor 172.168.9.1 remote-as 1001
 neighbor 172.168.11.10 remote-as 101
 ----------------------------------------
 Router#sh ip route bgp
      10.0.0.0/24 is subnetted, 1 subnets                          Трафик в Москве идет через Ламас
B        10.10.10.0 [200/0] via 172.168.15.10, 00:36:24
      20.0.0.0/24 is subnetted, 1 subnets
B        20.20.20.0 [200/0] via 172.168.15.10, 00:36:24
      30.0.0.0/24 is subnetted, 1 subnets
B        30.30.30.0 [200/0] via 172.168.15.10, 00:36:24
      40.0.0.0/24 is subnetted, 1 subnets
B        40.40.40.0 [200/0] via 172.168.15.10, 00:36:24
      172.168.0.0/16 is variably subnetted, 14 subnets, 2 masks
B        172.168.8.0/24 [200/0] via 172.168.9.1, 00:48:33
B        172.168.10.0/24 [200/0] via 172.168.7.1, 00:48:33
B        172.168.12.0/24 [200/0] via 172.168.15.10, 00:36:24
B        172.168.14.0/24 [200/0] via 172.168.15.10, 00:36:24
B        172.168.15.0/24 [200/0] via 172.168.6.2, 00:36:24
B     192.168.5.0/24 [200/0] via 172.168.15.10, 00:36:24
B     192.168.7.0/24 [200/0] via 172.168.15.10, 00:36:24
B     192.168.10.0/24 [200/0] via 172.168.15.10, 00:36:24
B     192.168.11.0/24 [200/0] via 172.168.15.10, 00:36:24
B     192.168.12.0/24 [200/0] via 172.168.15.10, 00:36:24
B     192.168.13.0/24 [200/0] via 172.168.15.10, 00:36:24
--------------------------------------------------------
Router#sh run | s route-map
 neighbor 172.168.6.2 route-map TUDA out
route-map TUDA permit 10
 set local-preference 999
--------------------------------------------------------
Router#sh ip bgp summary
Neighbor        V           AS MsgRcvd MsgSent   TblVer  InQ OutQ Up/Down  State/PfxRcd
172.168.6.2     4         1001      56      63       34    0    0 00:37:20       17
172.168.7.1     4         1001      63      71       34    0    0 00:50:26        2
172.168.9.1     4         1001      63      71       34    0    0 00:50:38        2
172.168.11.10   4          101      74      73       34    0    0 00:50:20       14


 R15
 Router#sh run | s bgp
router bgp 1001
 bgp log-neighbor-changes
 network 172.168.6.0 mask 255.255.255.0
 network 172.168.8.0 mask 255.255.255.0
 network 172.168.10.0 mask 255.255.255.0
 network 172.168.15.0 mask 255.255.255.0
 neighbor 172.168.6.1 remote-as 1001
 neighbor 172.168.8.1 remote-as 1001
 neighbor 172.168.10.1 remote-as 1001
 neighbor 172.168.15.10 remote-as 301
 neighbor 172.168.15.10 route-map PREF in
-------------------------------------------
Router#sh ip route bgp
      10.0.0.0/24 is subnetted, 1 subnets                           Трафик в Москве идет через Ламас
B        10.10.10.0 [20/0] via 172.168.15.10, 00:55:37
      20.0.0.0/24 is subnetted, 1 subnets
B        20.20.20.0 [20/0] via 172.168.15.10, 00:55:37
      30.0.0.0/24 is subnetted, 1 subnets
B        30.30.30.0 [20/0] via 172.168.15.10, 00:55:37
      40.0.0.0/24 is subnetted, 1 subnets
B        40.40.40.0 [20/0] via 172.168.15.10, 00:55:37
      172.168.0.0/16 is variably subnetted, 14 subnets, 2 masks
B        172.168.7.0/24 [200/0] via 172.168.10.1, 00:55:37
B        172.168.9.0/24 [200/0] via 172.168.6.1, 00:40:15
B        172.168.11.0/24 [20/0] via 172.168.15.10, 00:55:37
B        172.168.12.0/24 [20/0] via 172.168.15.10, 00:55:37
B        172.168.14.0/24 [20/0] via 172.168.15.10, 00:55:37
B     192.168.5.0/24 [20/0] via 172.168.15.10, 00:55:37
B     192.168.7.0/24 [20/0] via 172.168.15.10, 00:55:37
B     192.168.10.0/24 [20/0] via 172.168.15.10, 00:55:37
B     192.168.11.0/24 [20/0] via 172.168.15.10, 00:55:37
B     192.168.12.0/24 [20/0] via 172.168.15.10, 00:55:37
B     192.168.13.0/24 [20/0] via 172.168.15.10, 00:55:37
-------------------------------------------
Router#sh run | s route-map
 neighbor 172.168.15.10 route-map PREF in
route-map PREF permit 10
 set local-preference 999
-------------------------------------------
Router#sh ip bgp summary
Neighbor        V           AS MsgRcvd MsgSent   TblVer  InQ OutQ Up/Down  State/PfxRcd
172.168.6.1     4         1001      74      67       21    0    0 00:47:35        2
172.168.8.1     4         1001     101      75       21    0    0 01:00:47        2
172.168.10.1    4         1001      73      75       21    0    0 01:00:38        2
172.168.15.10   4          301      78      72       21    0    0 01:00:32       14

R22

Router#sh run | s bgp
router bgp 101
 bgp log-neighbor-changes
 network 172.168.11.0 mask 255.255.255.0
 network 172.168.12.0 mask 255.255.255.0
 neighbor 172.168.11.1 remote-as 1001
 neighbor 172.168.12.1 remote-as 301

R21

Router#sh run | s bgp
router bgp 301
 bgp log-neighbor-changes
 network 172.168.12.0 mask 255.255.255.0
 network 172.168.14.0 mask 255.255.255.0
 network 172.168.15.0 mask 255.255.255.0
 neighbor 172.168.12.10 remote-as 101
 neighbor 172.168.14.2 remote-as 520
 neighbor 172.168.15.1 remote-as 1001

R24

Router#sh run | s bgp
router bgp 520
 bgp log-neighbor-changes
 network 10.10.10.0 mask 255.255.255.0
 network 40.40.40.0 mask 255.255.255.0
 network 172.168.14.0 mask 255.255.255.0
 network 192.168.12.0
 neighbor 10.10.10.10 remote-as 520
 neighbor 10.10.10.10 route-reflector-client       RR настроил
 neighbor 40.40.40.2 remote-as 520
 neighbor 172.168.14.1 remote-as 301
 neighbor 192.168.12.1 remote-as 2042

R23

Router#sh run | s bgp
router bgp 520
 bgp log-neighbor-changes
 network 10.10.10.0 mask 255.255.255.0
 network 20.20.20.0 mask 255.255.255.0
 neighbor 10.10.10.1 remote-as 520
 neighbor 20.20.20.2 remote-as 520

R25

Router#sh run | s bgp
router bgp 520
 bgp log-neighbor-changes
 network 20.20.20.0 mask 255.255.255.0
 network 30.30.30.0 mask 255.255.255.0
 neighbor 20.20.20.1 remote-as 520
 neighbor 30.30.30.1 remote-as 520

R26

Router#sh run | s bgp
router bgp 520
 bgp log-neighbor-changes
 network 30.30.30.0 mask 255.255.255.0
 network 40.40.40.0 mask 255.255.255.0
 network 192.168.13.0
 neighbor 30.30.30.10 remote-as 520
 neighbor 30.30.30.10 route-reflector-client      RR настроил
 neighbor 40.40.40.1 remote-as 520
 neighbor 192.168.13.1 remote-as 2042

R18

Router#sh run | s bgp
router bgp 2042
 bgp log-neighbor-changes
 network 192.168.10.0
 network 192.168.11.0
 network 192.168.12.0
 network 192.168.13.0
 neighbor 192.168.10.1 remote-as 2042
 neighbor 192.168.11.1 remote-as 2042
 neighbor 192.168.12.10 remote-as 520
 neighbor 192.168.13.10 remote-as 520
 maximum-paths 2                                 Трафик распределяется по 2 линкам

 R17

 Router#sh run | s bgp
router bgp 2042
 bgp log-neighbor-changes
 network 192.168.5.0
 network 192.168.7.0
 network 192.168.10.0
 neighbor 192.168.5.1 remote-as 2042
 neighbor 192.168.7.2 remote-as 2042
 neighbor 192.168.10.10 remote-as 2042
 maximum-paths 2                                Трафик распределяется по 2 линкам

R16

Router#sh run | s bgp
router bgp 2042
 bgp log-neighbor-changes
 network 192.168.7.0
 network 192.168.11.0
 neighbor 192.168.7.1 remote-as 2042
 neighbor 192.168.11.10 remote-as 2042
 maximum-paths 2                               Трафик распределяется по 2 линкам
----------------------------------------
Router#ping 172.168.7.1                                     IP связность
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 172.168.7.1, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 2/2/3 ms
----------------------------------------
Router#ping 20.20.20.1
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 20.20.20.1, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 2/2/2 ms
----------------------------------------
Router#ping 172.168.11.10
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 172.168.11.10, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 1/1/2 ms

### Выполнение:
Настроил iBGP в офисом Москва между маршрутизаторами R14 и R15.
Настроил iBGP в провайдере Триада, с использованием RR.
Настроил офиса Москвы так, чтобы приоритетным провайдером стал Ламас.
Настрол офиса С.-Петербурга так, чтобы трафик до любого офиса распределялся по двум линкам одновременно.
Все сети в лабораторной работе должны иметь IP связность.
