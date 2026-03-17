### Цель:
Настроить OSPF офисе Москва

Разделить сеть на зоны

Настроить фильтрацию между зонами

### Задание 

Маршрутизаторы R14-R15 находятся в зоне 0 - backbone.

Маршрутизаторы R12-R13 находятся в зоне 10. Дополнительно к маршрутам должны получать маршрут по умолчанию.

Маршрутизатор R19 находится в зоне 101 и получает только маршрут по умолчанию.

Маршрутизатор R20 находится в зоне 102 и получает все маршруты, кроме маршрутов до сетей зоны 101.

План работы и изменения зафиксированы в документации .

### Общая таблица сетевых адресов нв интерфейсах:
| наименование | интерфейс | ip address      | Сеть

| R19          | e0/0      | 172.168.6.1/24  | 172.168.6.0/24

| R14          | e0/0      | 172.168.5.10/24 | 172.168.5.10/24

| R14          | e0/1      | 172.168.4.10/24 | 172.168.4.0/24

| R14          | e0/3      | 172.168.6.10/24 | 172.168.6.0/24

| R12          | e0/0      | 172.168.1.10/24 | 172.168.1.0/24

| R12          | e0/1      | 172.168.2.10/24 | 172.168.2.0/24

| R12          | e0/2      | 172.168.5.1/24  | 172.168.5.0/24

| R12          | e0/3      | 172.168.3.1/24  | 172.168.3.0/24

| R13          | e0/0      | 172.168.1.11/24 | 172.168.1.0/24

| R13          | e0/1      | 172.168.2.11/24 | 172.168.2.0/24

| R13          | e0/2      | 172.168.7.1/24  | 172.168.7.0/24

| R13          | e0/3      | 172.168.4.1/24  | 172.168.4.0/24

| R15          | e0/0      | 172.168.7.10/24 | 172.168.7.0/24

| R15          | e0/1      | 172.168.3.10/24 | 172.168.3.0/24

| R15          | e0/3      | 172.168.8.10/24 | 172.168.8.0/24

| R20          | e0/0      | 172.168.8.1/24  | 172.168.8.0/24

| VPC1         | e0/0      | 172.168.1.1/24  | 172.168.1.0/24

| VPC7         | e0/0      | 172.168.1.2/24  | 172.168.1.0./24

### Пример настроек

VPC 1

set pcname VPC 1

ip add ress 172.168.1.1 255.255.255.0  gateway 172.168.1.10

SW4 

spanning-tree mode rapid-pvst

spanning-tree vlan 1 root primary

R12

Router#sh ip route static
S*    0.0.0.0/0 [1/0] via 172.168.5.10

Router#sh ip route ospf
O        172.168.4.0/24 [110/20] via 172.168.5.10, 00:26:59, Ethernet0/2
O        172.168.7.0/24 [110/20] via 172.168.3.10, 00:21:33, Ethernet0/3
O IA     172.168.8.0/24 [110/20] via 172.168.3.10, 00:09:59, Ethernet0/3

Router#sh run | s r o
router ospf 33
 network 172.168.2.0 0.0.0.255 area 10
 network 172.168.3.0 0.0.0.255 area 0
 network 172.168.5.0 0.0.0.255 area 0

R13

Router#sh ip route static
S*    0.0.0.0/0 [1/0] via 172.168.7.10

Router#sh ip route ospf
O        172.168.3.0/24 [110/20] via 172.168.7.10, 00:28:17, Ethernet0/2
O        172.168.5.0/24 [110/20] via 172.168.4.10, 00:22:20, Ethernet0/3
O IA     172.168.8.0/24 [110/20] via 172.168.7.10, 00:16:42, Ethernet0/2

Router#sh run | s r o
router ospf 33
 network 172.168.1.0 0.0.0.255 area 10
 network 172.168.2.0 0.0.0.255 area 10
 network 172.168.4.0 0.0.0.255 area 0
 network 172.168.7.0 0.0.0.255 area 0

R14 

router os 33
network 172.168.4.0 0.0.0.255 area 0
network 172.168.5.0 0.0.0.255 area 0
network 172.168.6.0 0.0.0.255 area 101
area 101 stub no-summary
area 101 filter-list prefix block6 out
 
Router#sh run | s r o
router ospf 33
 area 101 stub no-summary
 area 101 filter-list prefix block6 out
 network 172.168.4.0 0.0.0.255 area 0
 network 172.168.5.0 0.0.0.255 area 0
 network 172.168.6.0 0.0.0.255 area 101
----------------------------------------
ip prefix-list block6 deny 172.168.6.0/24
ip prefix-list block6 permit 0.0.0.0/0 le 32

Router#sh ip prefix-list
ip prefix-list block6: 2 entries
   seq 5 deny 172.168.6.0/24
   seq 10 permit 0.0.0.0/0 le 32
-----------------------------------------
Router#sh ip route ospf
O IA     172.168.1.0/24 [110/20] via 172.168.4.1, 00:32:46, Ethernet0/1
O IA     172.168.2.0/24 [110/20] via 172.168.5.1, 00:32:46, Ethernet0/0
                        [110/20] via 172.168.4.1, 00:32:46, Ethernet0/1
O        172.168.3.0/24 [110/20] via 172.168.5.1, 00:32:46, Ethernet0/0
O        172.168.7.0/24 [110/20] via 172.168.4.1, 00:32:46, Ethernet0/1
O IA     172.168.8.0/24 [110/30] via 172.168.5.1, 00:27:13, Ethernet0/0
                        [110/30] via 172.168.4.1, 00:27:13, Ethernet0/1
------------------------------------------
Router#sh ip os neighbor
Neighbor ID     Pri   State           Dead Time   Address         Interface
172.168.5.1       1   FULL/DR         00:00:36    172.168.5.1     Ethernet0/0
172.168.7.1       1   FULL/DR         00:00:38    172.168.4.1     Ethernet0/1
172.168.6.1       1   FULL/DR         00:00:34    172.168.6.1     Ethernet0/3
------------------------------------------
Router#sh ip os int brief
Interface    PID   Area            IP Address/Mask    Cost  State Nbrs F/C
Et0/0        33    0               172.168.5.10/24    10    BDR   1/1
Et0/1        33    0               172.168.4.10/24    10    BDR   1/1
Et0/3        33    101             172.168.6.10/24    10    BDR   1/1
------------------------------------------
Router#sh ip os database

            OSPF Router with ID (172.168.6.10) (Process ID 33)

                Router Link States (Area 0)

Link ID         ADV Router      Age         Seq#       Checksum Link count
172.168.5.1     172.168.5.1     1007        0x80000003 0x00A536 2
172.168.6.10    172.168.6.10    1004        0x80000004 0x00D1E9 2
172.168.7.1     172.168.7.1     1005        0x80000004 0x0013C6 2
172.168.8.10    172.168.8.10    1006        0x80000003 0x007A31 2

                Net Link States (Area 0)

Link ID         ADV Router      Age         Seq#       Checksum
172.168.3.10    172.168.8.10    1007        0x80000001 0x00EEBE
172.168.4.1     172.168.7.1     1010        0x80000001 0x00A31C
172.168.5.10    172.168.6.10    1005        0x80000001 0x00D4DA
172.168.7.10    172.168.8.10    1007        0x80000001 0x00DCCA

                Summary Net Link States (Area 0)

Link ID         ADV Router      Age         Seq#       Checksum
172.168.1.0     172.168.5.1     978         0x80000001 0x001A5D

R15

Router#sh run | s r o
router ospf 33
 area 102 nssa default-information-originate
 network 172.168.3.0 0.0.0.255 area 0
 network 172.168.7.0 0.0.0.255 area 0
 network 172.168.8.0 0.0.0.255 area 102
--------------------------------------------
Router#sh ip route os
O IA     172.168.1.0/24 [110/20] via 172.168.7.1, 00:01:07, Ethernet0/0
O IA     172.168.2.0/24 [110/20] via 172.168.7.1, 00:01:07, Ethernet0/0
                        [110/20] via 172.168.3.1, 00:01:07, Ethernet0/1
O        172.168.4.0/24 [110/20] via 172.168.7.1, 00:01:07, Ethernet0/0
O        172.168.5.0/24 [110/20] via 172.168.3.1, 00:01:07, Ethernet0/1
-------------------------------------------
Router#sh ip os nei
Neighbor ID     Pri   State           Dead Time   Address         Interface
172.168.7.1       1   FULL/BDR        00:00:37    172.168.7.1     Ethernet0/0
172.168.5.1       1   FULL/BDR        00:00:35    172.168.3.1     Ethernet0/1
172.168.8.1       1   FULL/BDR        00:00:35    172.168.8.1     Ethernet0/3
------------------------------------------
Router#sh ip os int brief
Interface    PID   Area            IP Address/Mask    Cost  State Nbrs F/C
Et0/0        33    0               172.168.7.10/24    10    DR    1/1
Et0/1        33    0               172.168.3.10/24    10    DR    1/1
Et0/3        33    102             172.168.8.10/24    10    DR    1/1

R19

Router#sh run | s r o
router ospf 33
 area 101 stub no-summary
 network 172.168.6.0 0.0.0.255 area 101
-------------------------------------------
Router#sh ip route os
O*IA  0.0.0.0/0 [110/11] via 172.168.6.10, 00:07:06, Ethernet0/0
-------------------------------------------
Router#sh ip os neighbor
Neighbor ID     Pri   State           Dead Time   Address         Interface
172.168.6.10      1   FULL/DR         00:00:31    172.168.6.10    Ethernet0/0

R20

Router#sh run | s r o
router ospf 33
 area 102 nssa default-information-originate
 area 102 filter-list prefix block6 in
 network 172.168.8.0 0.0.0.255 area 102
------------------------------------------
Router#sh ip route os
O IA     172.168.1.0/24 [110/30] via 172.168.8.10, 00:14:38, Ethernet0/0
O IA     172.168.2.0/24 [110/30] via 172.168.8.10, 00:14:38, Ethernet0/0
O IA     172.168.3.0/24 [110/20] via 172.168.8.10, 00:14:48, Ethernet0/0
O IA     172.168.4.0/24 [110/30] via 172.168.8.10, 00:14:38, Ethernet0/0
O IA     172.168.5.0/24 [110/30] via 172.168.8.10, 00:14:38, Ethernet0/0
O IA     172.168.7.0/24 [110/20] via 172.168.8.10, 00:14:48, Ethernet0/0
-----------------------------------------
Router#sh ip os neighbor
Neighbor ID     Pri   State           Dead Time   Address         Interface
172.168.8.10      1   FULL/DR         00:00:31    172.168.8.10    Ethernet0/0

### Выполнение

Маршрутизаторы R14-R15 находятся в зоне 0 - backbone.

Маршрутизаторы R12-R13 находятся в зоне 10. Дополнительно к маршрутам должны получать маршрут по умолчанию.

Маршрутизатор R19 находится в зоне 101 и получает только маршрут по умолчанию. Сделал зону 101 totally stub . На зону 101 повесил филтер-лист на выход.

Маршрутизатор R20 находится в зоне 102 и получает все маршруты, кроме маршрутов до сетей зоны 101. Сделал зону 102 totally nssa .

План работы и изменения зафиксированы в документации .

