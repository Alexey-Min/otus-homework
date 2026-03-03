### Цель:

Настроить политику маршрутизации в офисе Чокурдах;
Распределить трафик между 2 линками.

### Задание:

1 Настроить политику маршрутизации для сетей офиса.
2 Распределение трафика между двумя линками с провайдером.
3 Настроить отслеживание линка через технологию IP SLA.(только для IPv4)
4 Настройть для офиса Лабытнанги маршрут по-умолчанию.
5 План работы и изменения зафиксировать в документации.

### Общая таблица сетевых адресов нв интерфейсах:
| наименование | интерфейс | ip address      | Сеть

| R1           | e0/0      | 191.112.84.1/24 | 191.112.84.0/24

| R1           | e0/1      | 192.168.1.10/24 | 192.168.1.0/24

| R2           | e0/0      | 191.114.86.10/24| 191.114.86.0/24

| R2           | e0/1      | 191.112.84.10/24| 191.112.84.0/24

| R3           | e0/0      | 191.114.86.1/24 | 191.114.86.0/24

| R3           | e0/1      | 192.113.85.1/24 | 192.113.85.0/24

| R3           | e0/2      | 172.168.1.10/24 | 172.168.1.0/24

| R7           | e0/0      | 191.113.85.10/24| 191.113.85.0/24

| VPC1         | e0/0      | 172.168.1.1/24  | 172.168.1.0/24

| VPC2         | e0/0      | 172.168.1.2/24  | 172.168.1.0./24

| VPC3         | e0/0      | 192.168.1.1/24  | 192.168.1.0/24

### Пример настроек

VPC 1

set pcname VPC 1

ip add ress 172.168.1.1 255.255.255.0  gateway 172.168.1.10

R4

spanning-tree mode rapid-pvst

R1

ip route 0.0.0.0 0.0.0.0 191.112.84.10

R3

ip route 0.0.0.0 0.0.0.0 191.114.86.10 50 track 1

ip route 0.0.0.0 0.0.0.0 191.113.85.10 100


Gateway of last resort is 191.114.86.10 to network 0.0.0.0

S*    0.0.0.0/0 [50/0] via 191.114.86.10
      172.168.0.0/16 is variably subnetted, 2 subnets, 2 masks
C        172.168.1.0/24 is directly connected, Ethernet0/2
L        172.168.1.10/32 is directly connected, Ethernet0/2
      191.114.0.0/16 is variably subnetted, 2 subnets, 2 masks
C        191.114.86.0/24 is directly connected, Ethernet0/0
L        191.114.86.1/32 is directly connected, Ethernet0/0
      192.113.85.0/24 is variably subnetted, 2 subnets, 2 masks
C        192.113.85.0/24 is directly connected, Ethernet0/1
L        192.113.85.1/32 is directly connected, Ethernet0/1

------------------------------------------------

ip access-list extended PBR-VPC1

permit ip 172.168.1.1 255.255.255.0 any

Router#show access-lists
Extended IP access list PBR-VPC1
    10 permit ip 0.0.0.1 255.255.255.0 any
Router#

-------------------------------------------------

route-map PBR permit 10

match ip address PBR-VPC1

set ip next-hop 191.113.85.10

route-map PBR, permit, sequence 10
  Match clauses:
    ip address (access-lists): PBR-VPC1
  Set clauses:
    ip next-hop 191.113.85.10
  Policy routing matches: 0 packets, 0 bytes
Router#

-------------------------------------------------

int eth 0/2

ip policy route-map PBR

Router#show ip policy
Interface      Route map
Ethernet0/2    PBR
Router#Router#

-------------------------------------------------

track 1 ip sla 1 reachability 

delay down 30 up 30

-------------------------------------------------

ip sla 1

icmp-echo 191.114.86.10 seource-ip 191.113.85.1

frequency 10

ip sla scheduler 1 life forever start-time now

Router#show run | s sla
track 1 ip sla 1 reachability
 delay down 30 up 30
ip sla 1
 icmp-echo 191.114.86.10 source-ip 191.114.86.1
 frequency 10
ip sla schedule 1 life forever start-time now
Router#
show int


