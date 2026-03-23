
### Цель:
Настроить IS-IS офисе Триада

### Задание:
Настроите IS-IS в ISP Триада.
R23 и R25 находятся в зоне 2222.
R24 находится в зоне 24.
R26 находится в зоне 26

### Общая таблица сетевых адресов на интерфейсах.

| наименование | интерфейс | ip address             | Сеть  

| R23          | e0/1      | 172.168.1.1/24         |  172.168.1.0/24

| R23          | e0/2      | 10.10.1.1/24           | 10.10.1.0/24

| R24          | e0/1      | 20.20.1.1/24           | 20.20.1.0/24

| R24          | e0/2      | 10.10.1.2/24           | 10.10.1.0/24

| R25          | e0/2      | 192.168.1.2/24         | 192.168.1.0/24

| R25          | e0/0      | 172.168.1.2/24         | 172.168.1.0/24

| R26          | e0/0      | 20.20.1.2/24           | 20.20.1.0/24

| R26          | e0/2      | 192.168.1.1/24         | 192.168.1.0/24


###  Пример настройки

R23
Router#sh run | s r i
 ip router isis 1
 ip router isis 1
router isis 1
 net 49.2222.0000.0000.2222.00
--------------------------------

Router#sh isis nei

Tag 1:
System Id      Type Interface   IP Address      State Holdtime Circuit Id
Router         L2   Et0/2       10.10.1.2       UP    8        Router.01        
Router         L1   Et0/1       172.168.1.2     UP    8        Router.01        
Router         L2   Et0/1       172.168.1.2     UP    9        Router.01
---------------------------------

Router#sh int eth 0/1
Ethernet0/1 is up, line protocol is up
  Hardware is AmdP2, address is aabb.cc01.7010 (bia aabb.cc01.7010)
  Internet address is 172.168.1.1/24

Router#sh int eth 0/2
Ethernet0/2 is up, line protocol is up
  Hardware is AmdP2, address is aabb.cc01.7020 (bia aabb.cc01.7020)
  Internet address is 10.10.1.1/24
  
---------------------------------
Router#sh ip route isis
      20.0.0.0/24 is subnetted, 1 subnets
i L2     20.20.1.0 [115/20] via 10.10.1.2, 00:54:38, Ethernet0/2
i L1  192.168.1.0/24 [115/20] via 172.168.1.2, 00:53:16, Ethernet0/1

R24
Router#sh run | s r i
 ip router isis 1
 ip router isis 1
router isis 1
 net 49.0024.0000.0000.0024.00
--------------------------------

Router#sh isis nei

Tag 1:
System Id      Type Interface   IP Address      State Holdtime Circuit Id
Router         L2   Et0/1       20.20.1.2       UP    8        Router.02        
Router         L2   Et0/2       10.10.1.1       UP    25       Router.01 

R25
Router#sh  run | s r i
 ip router isis 1
 ip router isis 1
router isis 1
 net 49.2222.0000.0000.2223.00
-------------------------------

Router#sh isis nei

Tag 1:
System Id      Type Interface   IP Address      State Holdtime Circuit Id
Router         L2   Et0/2       192.168.1.1     UP    9        Router.01        
Router         L1   Et0/0       172.168.1.1     UP    29       Router.01        
Router         L2   Et0/0       172.168.1.1     UP    27       Router.01  

R26
Router#sh run | s r i
 ip router isis 1
 ip router isis 1
router isis 1
 net 49.0026.0000.0000.0026.00
--------------------------------

Router#sh isis nei

Tag 1:
System Id      Type Interface   IP Address      State Holdtime Circuit Id
Router         L2   Et0/0       20.20.1.1       UP    22       Router.02        
Router         L2   Et0/2       192.168.1.2     UP    25       Router.01  

### Выполнено

Настроиk IS-IS в ISP Триада.
R23 и R25 находятся в зоне 2222.
R24 находится в зоне 24.
R26 находится в зоне 26


