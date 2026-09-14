### Цель:
Настроить eBGP/iBGP IPv6 unicast для всех сегментов сети по аналогичной логике с настройкой eBGP/iBGP IPv4 unicast .

### Задание:
Настроить eBGP IPv6 unicast между офисом Москва и двумя провайдерами - Киторн и Ламас.
Настроить eBGP IPv6 unicast между провайдерами Киторн и Ламас.
Настроить eBGP IPv6 unicast между Ламас и Триада.
Настроить eBGP IPv6 unicast между офисом С.-Петербург и провайдером Триада.
Организовать IPv6 unicast связность между пограничными роутерами офисов Москва и С.-Петербург.
Настроить iBGP IPv6 unicast в офисе Москва между маршрутизаторами R14 и R15.
Настроить iBGP IPv6 unicast в провайдере Триада, с использованием RR.
Все сети IPv6 в лабораторной работе должны иметь связность между собой.

### Общая таблица сетевых адресов на интерфейсах.

| наименование | интерфейс | ip address      | Сеть

| R14          | e0/0      | 1111:aaa:1:1::1 | 1111:aaa:1:1::0

| R14          | e0/2      | 1111:bbb:1:1::1 | 1111:bbb:1:1::0

| R15          | e0/0      | 1111:aaa:1:1::2 | 1111:aaa:1:1::0

| R15          | e0/2      | 1111:ccc:1:1::1 | 1111:ccc:1:1::0

| R21          | e0/0      | 1111:ccc:1:1::2 | 1111:ccc:1:1::0

| R21          | e0/1      | 1111:ddd:1:1::1 | 1111:ddd:1:1::0

| R21          | e0/2      | 1111:fff:1:1::1 | 1111:aaa:1:1::0

| R22          | e0/0      | 1111:bbb:1:1::2 | 1111:bbb:1:1::0

| R22          | e0/1      | 1111:ddd:1:1::2 | 1111:ddd:1:1::0

| R22          | e0/2      | 1111:eee:1:1::1 | 1111:eee:1:1::0

| R24          | e0/0      | 1111:fff:1:1::2 | 1111:fff:1:1::0

| R24          | e0/1      | 1111:ddd:1:1::1 | 1111:ddd:1:1::0

| R24          | e0/2      | 2222:bbb:1:1::2 | 2222:bbb:1:1::0

| R24          | e0/3      | 2222:eee:1:1::1 | 2222:eee:1:1::0

| R23          | e0/0      | 1111:eee:1:1::2 | 1111:eee:1:1::0

| R23          | e0/1      | 2222:aaa:1:1::1 | 2222:aaa:1:1::0

| R23          | e0/2      | 2222:bbb:1:1::1 | 2222:bbb:1:1::0

| R26          | e0/0      | 2222:ddd:1:1::2 | 2222:ddd:1:1::0

| R26          | e0/2      | 2222:ccc:1:1::2 | 2222:ccc:1:1::0

| R26          | e0/3      | 2222:fff:1:1::1 | 2222:fff:1:1::0

| R25          | e0/0      | 2222:aaa:1:1::2 | 2222:aaa:1:1::0

| R25          | e0/2      | 2222:ccc:1:1::1 | 2222:ccc:1:1::0

| R18          | e0/2      | 2222:eee:1:1::2 | 2222:eee:1:1::0

| R18          | e0/3      | 2222:fff:1:1::2 | 2222:fff:1:1::0

### Пример настроек

R14

Router#sh  ipv6 int br
Ethernet0/0            [up/up]
    FE80::A8BB:CCFF:FE00:E000
    1111:AAA:1:1::1
Ethernet0/1            [administratively down/down]
    unassigned
Ethernet0/2            [up/up]
    FE80::A8BB:CCFF:FE00:E020
    1111:BBB:1:1::1
Ethernet0/3            [administratively down/down]
    unassigned
----------------------------------------------------------------
Router#sh run | s ospf                                          -------- Настроил iBGP IPv6 unicast в офисе Москва между маршрутизаторами R14 и R15.
 ipv6 ospf 1001 area 0
ipv6 router ospf 1001
 router-id 1.4.0.0

Router#sh run
interface Ethernet0/0
 no ip address
 ipv6 address 1111:AAA:1:1::1/36
 ipv6 ospf 1001 area 0
----------------------------------------------------------------
Router#sh run | s bgp
router bgp 1001
 bgp router-id 1.4.0.0
 bgp log-neighbor-changes
 no bgp default ipv4-unicast
 neighbor 1111:AAA:1:1::2 remote-as 1001
 neighbor 1111:BBB:1:1::2 remote-as 101                         --------Настроил eBGP IPv6 unicast между офисом Москва и двумя провайдерами - Киторн и Ламас.
 !
 address-family ipv4
 exit-address-family
 !
 address-family ipv6
  redistribute connected
  neighbor 1111:AAA:1:1::2 activate
 exit-address-family
---------------------------------------------------------------
Router#sh ipv6 route bgp
B   1111:CCC::/36 [200/0]
     via 1111:AAA:1:1::2
B   1111:DDD::/36 [200/0]
     via 1111:CCC:1:1::2
B   1111:EEE::/36 [200/0]
     via 1111:CCC:1:1::2
B   1111:FFF::/36 [200/0]
     via 1111:CCC:1:1::2
B   2222:AAA::/36 [200/0]
     via 1111:CCC:1:1::2
B   2222:BBB::/36 [200/0]
     via 1111:CCC:1:1::2
B   2222:CCC::/36 [200/0]
     via 1111:CCC:1:1::2
B   2222:DDD::/36 [200/0]
     via 1111:CCC:1:1::2
B   2222:EEE::/36 [200/0]
     via 1111:CCC:1:1::2
B   2222:FFF::/36 [200/0]
     via 1111:CCC:1:1::2

R15

Router#sh ipv6 int br
Ethernet0/0            [up/up]
    FE80::A8BB:CCFF:FE00:F000
    1111:AAA:1:1::2
Ethernet0/1            [administratively down/down]
    unassigned
Ethernet0/2            [up/up]
    FE80::A8BB:CCFF:FE00:F020
    1111:CCC:1:1::1
Ethernet0/3            [administratively down/down]
    unassigned
----------------------------------------------------------
Router#sh run | s ospf                                    -------- Настроил iBGP IPv6 unicast в офисе Москва между маршрутизаторами R14 и R15.
 ipv6 ospf 1001 area 0
ipv6 router ospf 1001
 router-id 1.5.0.0

interface Ethernet0/0
 no ip address
 ipv6 address 1111:AAA:1:1::2/36
 ipv6 ospf 1001 area 0
---------------------------------------------------------
Router#sh ipv6 route static
S   2222:EEE::/36 [1/0]
     via 1111:CCC:1:1::2

Router#sh run
ipv6 route 2222:EEE::/36 1111:CCC:1:1::2                 ----------Организовал IPv6 unicast связность между пограничными роутерами офисов Москва и С.-Петербург.
---------------------------------------------------------
Router#sh run | s bgp
router bgp 1001
 bgp router-id 1.5.0.0
 bgp log-neighbor-changes
 no bgp default ipv4-unicast
 neighbor 1111:AAA:1:1::1 remote-as 1001
 neighbor 1111:CCC:1:1::2 remote-as 301                  ----------Настроил eBGP IPv6 unicast между офисом Москва и двумя провайдерами - Киторн и Ламас.
 !
 address-family ipv4
 exit-address-family
 !
 address-family ipv6
  redistribute connected
  neighbor 1111:AAA:1:1::1 activate
  neighbor 1111:CCC:1:1::2 activate
 exit-address-family
-------------------------------------------------------
Router#sh ipv6 route  bgp
B   1111:BBB::/36 [200/0]
     via 1111:AAA:1:1::1
B   1111:DDD::/36 [20/0]
     via FE80::A8BB:CCFF:FE01:5000, Ethernet0/2
B   1111:EEE::/36 [20/0]
     via FE80::A8BB:CCFF:FE01:5000, Ethernet0/2
B   1111:FFF::/36 [20/0]
     via FE80::A8BB:CCFF:FE01:5000, Ethernet0/2
B   2222:AAA::/36 [20/0]
     via FE80::A8BB:CCFF:FE01:5000, Ethernet0/2
B   2222:BBB::/36 [20/0]
     via FE80::A8BB:CCFF:FE01:5000, Ethernet0/2
B   2222:CCC::/36 [20/0]
     via FE80::A8BB:CCFF:FE01:5000, Ethernet0/2
B   2222:DDD::/36 [20/0]
     via FE80::A8BB:CCFF:FE01:5000, Ethernet0/2
B   2222:FFF::/36 [20/0]
     via FE80::A8BB:CCFF:FE01:5000, Ethernet0/2

R21

Router#sh ipv6 int br
Ethernet0/0            [up/up]
    FE80::A8BB:CCFF:FE01:5000
    1111:CCC:1:1::2
Ethernet0/1            [up/up]
    FE80::A8BB:CCFF:FE01:5010
    1111:DDD:1:1::1
Ethernet0/2            [up/up]
    FE80::A8BB:CCFF:FE01:5020
    1111:FFF:1:1::1
-----------------------------------------------------
Router#sh ipv6 route static
S   2222:EEE::/36 [1/0]
     via 1111:FFF:1:1::2

Router#sh run
ipv6 route 1111:CCC::/36 1111:CCC:1:1::1             --------Организовал IPv6 unicast связность между пограничными роутерами офисов Москва и С.-Петербург.
ipv6 route 2222:EEE::/36 1111:FFF:1:1::2
-----------------------------------------------------
Router#sh run | s bgp
router bgp 301
 bgp router-id 2.1.0.0
 bgp log-neighbor-changes
 no bgp default ipv4-unicast
 neighbor 1111:CCC:1:1::1 remote-as 1001             -------Настроил eBGP IPv6 unicast между офисом Москва и двумя провайдерами - Киторн и Ламас.
 neighbor 1111:DDD:1:1::2 remote-as 101              -------Настроил eBGP IPv6 unicast между провайдерами Киторн и Ламас.
 neighbor 1111:FFF:1:1::2 remote-as 520              -------Настроил eBGP IPv6 unicast между Ламас и Триада.
 !
 address-family ipv4
 exit-address-family
 !
 address-family ipv6
  redistribute connected
  network 1111:CCC:1:1::/64
  network 1111:DDD:1:1::/64
  network 1111:FFF:1:1::/64
  neighbor 1111:CCC:1:1::1 activate
  neighbor 1111:DDD:1:1::2 activate
  neighbor 1111:FFF:1:1::2 activate
  neighbor 1111:FFF:1:1::2 next-hop-self all
 exit-address-family
---------------------------------------------------
Router#sh ipv6 route bgp
B   1111:AAA::/36 [20/0]
     via FE80::A8BB:CCFF:FE00:F020, Ethernet0/0
B   1111:BBB::/36 [20/0]
     via FE80::A8BB:CCFF:FE00:F020, Ethernet0/0
B   1111:EEE::/36 [20/0]
     via FE80::A8BB:CCFF:FE01:8000, Ethernet0/2
B   2222:AAA::/36 [20/0]
     via FE80::A8BB:CCFF:FE01:8000, Ethernet0/2
B   2222:BBB::/36 [20/0]
     via FE80::A8BB:CCFF:FE01:8000, Ethernet0/2
B   2222:CCC::/36 [20/0]
     via FE80::A8BB:CCFF:FE01:8000, Ethernet0/2
B   2222:DDD::/36 [20/0]
     via FE80::A8BB:CCFF:FE01:8000, Ethernet0/2
B   2222:FFF::/36 [20/0]
     via FE80::A8BB:CCFF:FE01:8000, Ethernet0/2

R22

Router#sh ipv6 int br
Ethernet0/0            [up/up]
    FE80::A8BB:CCFF:FE01:6000
    1111:BBB:1:1::2
Ethernet0/1            [up/up]
    FE80::A8BB:CCFF:FE01:6010
    1111:DDD:1:1::2
Ethernet0/2            [up/up]
    FE80::A8BB:CCFF:FE01:6020
    1111:EEE:1:1::1
---------------------------------------------------
Router#sh run | s bgp
router bgp 101
 bgp router-id 2.2.0.0
 bgp log-neighbor-changes
 no bgp default ipv4-unicast
 neighbor 1111:BBB:1:1::1 remote-as 1001           ------Настроил eBGP IPv6 unicast между офисом Москва и двумя провайдерами - Киторн и Ламас.
 neighbor 1111:DDD:1:1::1 remote-as 301            ------Настроил eBGP IPv6 unicast между провайдерами Киторн и Ламас.
 neighbor 1111:EEE:1:1::2 remote-as 520
 !
 address-family ipv4
 exit-address-family
 !
 address-family ipv6
  redistribute connected
  neighbor 1111:BBB:1:1::1 activate
  neighbor 1111:DDD:1:1::1 activate
  neighbor 1111:DDD:1:1::1 next-hop-self all
  neighbor 1111:EEE:1:1::2 activate
  neighbor 1111:EEE:1:1::2 next-hop-self all
 exit-address-family
---------------------------------------------------
Router#sh ipv6 route bgp
B   1111:AAA::/36 [20/0]
     via FE80::A8BB:CCFF:FE01:5010, Ethernet0/1
B   1111:CCC::/36 [20/0]
     via FE80::A8BB:CCFF:FE01:5010, Ethernet0/1
B   1111:FFF::/36 [20/0]
     via FE80::A8BB:CCFF:FE01:5010, Ethernet0/1
B   2222:AAA::/36 [20/0]
     via FE80::A8BB:CCFF:FE01:7000, Ethernet0/2
B   2222:BBB::/36 [20/0]
     via FE80::A8BB:CCFF:FE01:7000, Ethernet0/2
B   2222:CCC::/36 [20/0]
     via FE80::A8BB:CCFF:FE01:7000, Ethernet0/2
B   2222:DDD::/36 [20/0]
     via FE80::A8BB:CCFF:FE01:7000, Ethernet0/2
B   2222:EEE::/36 [20/0]
     via FE80::A8BB:CCFF:FE01:7000, Ethernet0/2
B   2222:FFF::/36 [20/0]
     via FE80::A8BB:CCFF:FE01:7000, Ethernet0/2

R24

Router#sh ipv6 int br
Ethernet0/0            [up/up]
    FE80::A8BB:CCFF:FE01:8000
    1111:FFF:1:1::2
Ethernet0/1            [up/up]
    FE80::A8BB:CCFF:FE01:8010
    2222:DDD:1:1::1
Ethernet0/2            [up/up]
    FE80::A8BB:CCFF:FE01:8020
    2222:BBB:1:1::2
Ethernet0/3            [up/up]
    FE80::A8BB:CCFF:FE01:8030
    2222:EEE:1:1::1
-------------------------------------------------------
Router#sh ipv6 route static
S   1111:CCC::/36 [1/0]
     via 1111:FFF:1:1::1

Router#sh run
ipv6 route 1111:CCC::/36 1111:FFF:1:1::1               --------Организуете IPv6 unicast связность между пограничными роутерами офисов Москва и С.-Петербург.
ipv6 route 2222:EEE::/36 2222:EEE:1:1::2
-------------------------------------------------------
Router#sh run | s bgp
router bgp 520
 bgp router-id 2.4.0.0
 bgp log-neighbor-changes
 no bgp default ipv4-unicast
 neighbor 1111:FFF:1:1::1 remote-as 301                ---------Настроите eBGP IPv6 unicast между Ламас и Триада.
 neighbor 2222:BBB:1:1::1 remote-as 520
 neighbor 2222:DDD:1:1::2 remote-as 520
 neighbor 2222:EEE:1:1::2 remote-as 2042               ---------Настроил eBGP IPv6 unicast между офисом С.-Петербург и провайдером Триада.
 !
 address-family ipv4
 exit-address-family
 !
 address-family ipv6
  redistribute connected
  network 1111:FFF:1:1::/64
  network 2222:BBB:1:1::/64
  network 2222:DDD:1:1::/64
  network 2222:EEE:1:1::/64
  neighbor 1111:FFF:1:1::1 activate
  neighbor 2222:BBB:1:1::1 activate
  neighbor 2222:BBB:1:1::1 route-reflector-client       ------------Настроил iBGP IPv6 unicast в провайдере Триада, с использованием RR.
  neighbor 2222:BBB:1:1::1 next-hop-self all
  neighbor 2222:DDD:1:1::2 activate
  neighbor 2222:DDD:1:1::2 route-reflector-client       ------------Настроил iBGP IPv6 unicast в провайдере Триада, с использованием RR.
  neighbor 2222:DDD:1:1::2 next-hop-self all
  neighbor 2222:EEE:1:1::2 activate
  neighbor 2222:EEE:1:1::2 next-hop-self all
 exit-address-family
-----------------------------------------------------
Router#sh ipv6 route bgp
B   1111:AAA::/36 [20/0]
     via FE80::A8BB:CCFF:FE01:5020, Ethernet0/0
B   1111:BBB::/36 [200/0]
     via 1111:EEE:1:1::1
B   1111:DDD::/36 [20/0]
     via FE80::A8BB:CCFF:FE01:5020, Ethernet0/0
B   1111:EEE::/36 [200/0]
     via 2222:BBB:1:1::1
B   2222:AAA::/36 [200/0]
     via 2222:BBB:1:1::1
B   2222:CCC::/36 [200/0]
     via 2222:DDD:1:1::2
B   2222:FFF::/36 [200/0]
     via 2222:DDD:1:1::2

R23

Router#sh ipv6 int br
Ethernet0/0            [up/up]
    FE80::A8BB:CCFF:FE01:7000
    1111:EEE:1:1::2
Ethernet0/1            [up/up]
    FE80::A8BB:CCFF:FE01:7010
    2222:AAA:1:1::1
Ethernet0/2            [up/up]
    FE80::A8BB:CCFF:FE01:7020
    2222:BBB:1:1::1
-------------------------------------------
Router#sh run | s bgp
router bgp 520
 bgp router-id 2.3.0.0
 bgp log-neighbor-changes
 no bgp default ipv4-unicast
 neighbor 1111:EEE:1:1::1 remote-as 101
 neighbor 2222:AAA:1:1::2 remote-as 520
 neighbor 2222:BBB:1:1::2 remote-as 520
 !
 address-family ipv4
 exit-address-family
 !
 address-family ipv6
  redistribute connected
  network 1111:EEE:1:1::/64
  network 2222:AAA:1:1::/64
  network 2222:BBB:1:1::/64
  neighbor 1111:EEE:1:1::1 activate
  neighbor 2222:AAA:1:1::2 activate
  neighbor 2222:BBB:1:1::2 activate
 exit-address-family
-----------------------------------------
Router#sh ipv6 route bgp
B   1111:AAA::/36 [200/0]
     via 2222:BBB:1:1::2
B   1111:BBB::/36 [20/0]
     via FE80::A8BB:CCFF:FE01:6020, Ethernet0/0
B   1111:CCC::/36 [200/0]
     via 2222:BBB:1:1::2
B   1111:DDD::/36 [20/0]
     via FE80::A8BB:CCFF:FE01:6020, Ethernet0/0
B   1111:FFF::/36 [200/0]
     via 2222:BBB:1:1::2
B   2222:CCC::/36 [200/0]
     via 2222:AAA:1:1::2
B   2222:DDD::/36 [200/0]
     via 2222:BBB:1:1::2
B   2222:EEE::/36 [200/0]
     via 2222:BBB:1:1::2
B   2222:FFF::/36 [200/0]
     via 2222:BBB:1:1::2

R25

Router#sh ipv6 int br
Ethernet0/0            [up/up]
    FE80::A8BB:CCFF:FE01:9000
    2222:AAA:1:1::2
Ethernet0/1            [administratively down/down]
    unassigned
Ethernet0/2            [up/up]
    FE80::A8BB:CCFF:FE01:9020
    2222:CCC:1:1::1
------------------------------------------------------
Router#sh run | s bgp
router bgp 520
 bgp router-id 2.5.0.0
 bgp log-neighbor-changes
 no bgp default ipv4-unicast
 neighbor 2222:AAA:1:1::1 remote-as 520
 neighbor 2222:CCC:1:1::2 remote-as 520
 !
 address-family ipv4
 exit-address-family
 !
 address-family ipv6
  redistribute connected
  neighbor 2222:AAA:1:1::1 activate
  neighbor 2222:CCC:1:1::2 activate
 exit-address-family
-----------------------------------------------------
Router#sh ipv6 route bgp
B   1111:AAA::/36 [200/0]
     via 2222:CCC:1:1::2
B   1111:BBB::/36 [200/0]
     via 1111:EEE:1:1::1
B   1111:CCC::/36 [200/0]
     via 2222:CCC:1:1::2
B   1111:DDD::/36 [200/0]
     via 1111:EEE:1:1::1
B   1111:EEE::/36 [200/0]
     via 2222:AAA:1:1::1
B   1111:FFF::/36 [200/0]
     via 2222:CCC:1:1::2
B   2222:BBB::/36 [200/0]
     via 2222:AAA:1:1::1
B   2222:DDD::/36 [200/0]
     via 2222:CCC:1:1::2
B   2222:EEE::/36 [200/0]
     via 2222:CCC:1:1::2
B   2222:FFF::/36 [200/0]
     via 2222:CCC:1:1::2





