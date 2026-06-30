### Цель:

Настроить фильтрацию для офисе Москва
Настроить фильтрацию для офисе С.-Петербург

### Задание:

Настроить фильтрацию в офисе Москва так, чтобы не появилось транзитного трафика(As-path).
Настроить фильтрацию в офисе С.-Петербург так, чтобы не появилось транзитного трафика(Prefix-list).
Настроить провайдера Киторн так, чтобы в офис Москва отдавался только маршрут по умолчанию.
Настроить провайдера Ламас так, чтобы в офис Москва отдавался только маршрут по умолчанию и префикс офиса С.-Петербург.
Все сети в лабораторной работе должны иметь IP связность.

### Общая таблица сетевых адресов на интерфейсах.

| наименование | интерфейс | ip address      | Сеть

| R1           | e0/0      | 172.168.1.1/24  | 172.168.1.0/24

| R1           | l0        | 1.1.1.1/24      | 1.1.1.0/24 

| R1           | e0/1      | 172.168.4.2/24  | 172.168.4.0/24

| R2           | e0/0      | 172.168.1.2/24  | 172.168.1.0/24

| R2           | e0/1      | 172.168.2.1/24  | 172.168.2.0/24

| R2           | e0/2      | 172.168.6.2/24  | 172.168.6.0/24

| R2           | l0        | 2.2.2.2/24      | 2.2.2.0/24 

| R3           | e0/0      | 172.168.2.2/24  | 172.168.2.0/24

| R3           | e0/1      | 172.168.3.1/24  | 172.168.3.0/24

| R3           | l0        | 3.3.3.3/24      | 3.3.3.0/24 

| R4           | e0/1      | 172.168.3.2/24  | 172.168.3.0/24

| R4           | l0        | 4.4.4.4/24      | 4.4.4.0/24 

| R5           | e0/0      | 172.168.5.2/24  | 172.168.5.0/24

| R5           | e0/1      | 172.168.6.1/24  | 172.168.6.0/24

| R5           | l0        | 5.5.5.5/24      | 5.5.5.0/24 

| R6           | e0/0      | 172.168.4.1/24  | 172.168.4.0/24

| R6           | e0/1      | 172.168.5.1/24  | 172.168.5.0/24

| R6           | l0        | 6.6.6.6/24      | 6.6.6.0/24 

### Пример настроек

R1

Router#sh run | s bgp
router bgp 1001
 bgp router-id 1.1.1.1
 bgp log-neighbor-changes
 bgp bestpath as-path multipath-relax
 redistribute connected
 neighbor 172.168.1.2 remote-as 301
 neighbor 172.168.1.2 default-originate
 neighbor 172.168.1.2 soft-reconfiguration inbound
 neighbor 172.168.1.2 filter-list 100 in
 neighbor 172.168.4.1 remote-as 1001
 maximum-paths 2
------------------------------------------------------
Router#sh run | s as-path
 bgp bestpath as-path multipath-relax          
ip as-path access-list 101 permit ^$                     ----------------настраиваю фильтрацию в оф. Москва , что бы не появилось транзитного трафика

R6

Router#sh run | s bgp
router bgp 1001
 bgp router-id 6.6.6.6
 bgp log-neighbor-changes
 bgp bestpath as-path multipath-relax
 redistribute connected
 neighbor 172.168.4.2 remote-as 1001
 neighbor 172.168.5.2 remote-as 101
 neighbor 172.168.5.2 default-originate
 neighbor 172.168.5.2 soft-reconfiguration inbound
 neighbor 172.168.5.2 filter-list 100 in
 maximum-paths 2
-----------------------------------------------------
Router#sh run | s as-path
 bgp bestpath as-path multipath-relax
ip as-path access-list 100 permit ^$                  ----------------настраиваю фильтрацию в оф. Москва , что бы не появилось транзитного трафика

R5

Router#sh run | s bgp
router bgp 101
 bgp router-id 5.5.5.5
 bgp log-neighbor-changes
 network 0.0.0.0
 redistribute connected
 neighbor 172.168.5.1 remote-as 1001
 neighbor 172.168.5.1 route-map DEFAULT out
 neighbor 172.168.6.2 remote-as 301
----------------------------------------------------
Router#sh run | s route-map
 neighbor 172.168.5.1 route-map DEFAULT out
route-map DEFAULT permit 10
 match ip address prefix-list DEF
---------------------------------------------------
Router#sh run | s prefix-list
ip prefix-list DEF seq 5 permit 0.0.0.0/0           ----------------------- настраиваю провайдер Киторн так , что бы в офис Москва отдавался маршрут по умолчанию    
 match ip address prefix-list DEF

R2

Router#sh run | s bgp
router bgp 301
 bgp router-id 2.2.2.2
 bgp log-neighbor-changes
 redistribute connected
 neighbor 172.168.1.1 remote-as 1001
 neighbor 172.168.1.1 route-map UM_SP out
 neighbor 172.168.2.2 remote-as 520
 neighbor 172.168.2.2 next-hop-self
 neighbor 172.168.6.1 remote-as 101
---------------------------------------------
Router#sh run | s prefix-list
ip prefix-list UM seq 5 permit 0.0.0.0/0 le 32     ------------------------- Настроил Ламас так, чтобы в офис Москва отдавался только маршрут по умолчанию и префикс офиса С.-Петербург.
ip prefix-list UM seq 10 permit 4.4.4.0/24 le 32
---------------------------------------------
Router#sh run | s route-map
 neighbor 172.168.1.1 route-map UM_SP out
route-map UM_SP permit 10
 match ip address UM

R3

Router#sh run | s bgp
router bgp 520
 bgp router-id 3.3.3.3
 bgp log-neighbor-changes
 redistribute connected
 neighbor 172.168.2.1 remote-as 301
 neighbor 172.168.3.2 remote-as 2042
 neighbor 172.168.3.2 next-hop-self

R4

Router#ping 172.168.6.1
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 172.168.6.1, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 1/1/1 ms     ------------------ сети в лабораторной работе имеют IP связность
---------------------------------------------------------------------
Router#ping 172.168.4.1
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 172.168.4.1, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 1/1/2 ms
---------------------------------------------------------------------
Router#sh run | s bgp
router bgp 2042
 bgp router-id 4.4.4.4
 bgp log-neighbor-changes
 network 4.4.4.0 mask 255.255.255.0
 redistribute connected
 neighbor 172.168.3.1 remote-as 520
 neighbor 172.168.3.1 default-originate
 neighbor 172.168.3.1 route-map NO_TR in
--------------------------------------------------------------------
Router#sh run | s route-map
 neighbor 172.168.3.1 route-map NO_TR in
route-map NO_TR_A permit 10
 match ip address prefix-list NO_TR
--------------------------------------------------------------------
Router#sh run | s prefix-list
ip prefix-list NO_TR seq 5 permit 172.168.3.0/24                     ----------------------- Настроил фильтрацию в офисе С.-Петербург так, чтобы не появилось транзитного трафика(Prefix-list).
ip prefix-list NO_TR seq 10 permit 4.4.4.0/24
ip prefix-list NO_TR seq 15 deny 0.0.0.0/24

### Выполнение:

Настроил фильтрацию в офисе Москва так, чтобы не появилось транзитного трафика(As-path).
Настроил фильтрацию в офисе С.-Петербург так, чтобы не появилось транзитного трафика(Prefix-list).
Настроил провайдер Киторн так, чтобы в офис Москва отдавался только маршрут по умолчанию.
Настроил провайдера Ламас так, чтобы в офис Москва отдавался только маршрут по умолчанию и префикс офиса С.-Петербург.
Сети в лабораторной работе имеют IP связность.














