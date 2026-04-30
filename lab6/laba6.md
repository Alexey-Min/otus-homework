### Цель:
Настроить BGP между автономными системами
Организовать доступность между офисами Москва и С.-Петербург

### Задание
1 Настроите eBGP между офисом Москва и двумя провайдерами - Киторн и Ламас.
2 Настроите eBGP между провайдерами Киторн и Ламас.
3 Настроите eBGP между Ламас и Триада.
4 Настроите eBGP между офисом С.-Петербург и провайдером Триада.
5 Организуете IP доступность между пограничным роутерами офисами Москва и С.-Петербург.

### Общая таблица сетевых адресов на интерфейсах.
| наименование | интерфейс | ip address      | Сеть

| R14          | e0/2      | 172.168.2.1/24  | 172.168.2.0/24

| R14          | e0/3      | 172.168.1.1/24  | 172.168.1.0/24

| R15          | e0/0      | 172.168.1.2/24  | 172.168.1.0/24

| R15          | e0/2      | 172.168.3.1/24  | 172.168.3.0/24

| R22          | e0/0      | 172.168.2.10/24 | 172.168.2.0/24

| R22          | e0/1      | 172.168.4.10/24 | 172.168.4.0/24

| R22          | e0/2      | 172.168.5.1/24  | 172.168.5.0/24

| R21          | e0/0      | 172.168.3.10/24 | 172.168.3.0/24

| R21          | e0/1      | 172.168.4.1/24  | 172.168.4.0/24

| R21          | e0/2      | 172.168.6.1/24  | 172.168.6.0/24

| R24          | e0/0      | 172.168.6.2/24  | 172.168.6.0/24

| R24          | e0/1      | 40.40.40.1/24   | 40.40.40.0/24

| R24          | e0/2      | 30.30.30.1/24   | 30.30.30.0/24

| R24          | e0/3      | 192.168.1.10/24 | 192.168.1.0/24

| R23          | e0/0      | 172.168.5.2/24  | 172.168.5.0/24

| R23          | e0/1      | 10.10.10.1/24   | 10.10.10.0/24

| R23          | e0/2      | 30.30.30.10/24  | 30.30.30.0/24

| R25          | e0/0      | 10.10.10.2/24   | 10.10.10.0/24

| R25          | e0/2      | 20.20.20.10/24  | 20.20.20.0/24

| R26          | e0/0      | 40.40.40.2/24   | 40.40.40.0/24

| R26          | e0/2      | 20.20.20.1/24   | 20.20.20.0/24

| R26          | e0/3      | 192.168.2.10/24 | 192.168.2.0/24

| R18          | e0/2      | 192.168.1.1/24  | 192.168.1.0/24

| R18          | e0/3      | 192.168.2.1/24  | 192.168.2.0/24

### Пример настроек

R14

Router#sh run | s bgp
router bgp 1001
 bgp log-neighbor-changes
 network 172.168.1.0 mask 255.255.255.0
 network 172.168.2.0 mask 255.255.255.0
 neighbor 172.168.1.2 remote-as 1001
 neighbor 172.168.2.10 remote-as 101
--------------------------------------------
Router#sh ip bgp
BGP table version is 17, local router ID is 172.168.2.1
Status codes: s suppressed, d damped, h history, * valid, > best, i - internal,
              r RIB-failure, S Stale, m multipath, b backup-path, f RT-Filter,
              x best-external, a additional-path, c RIB-compressed,
Origin codes: i - IGP, e - EGP, ? - incomplete
RPKI validation codes: V valid, I invalid, N Not found

     Network          Next Hop            Metric LocPrf Weight Path
 *>i 20.20.20.0/24    172.168.3.10             0    100      0 301 520 i
 *                    172.168.2.10                           0 101 301 520 i
 *>i 30.30.30.0/24    172.168.3.10             0    100      0 301 520 i
 *                    172.168.2.10                           0 101 301 520 i
 *>i 40.40.40.0/24    172.168.3.10             0    100      0 301 520 i
 *                    172.168.2.10                           0 101 301 520 i
 * i 172.168.1.0/24   172.168.1.2              0    100      0 i
 *>                   0.0.0.0                  0         32768 i
 *   172.168.2.0/24   172.168.2.10             0             0 101 i
 *>                   0.0.0.0                  0         32768 i
 *>i 172.168.3.0/24   172.168.1.2              0    100      0 i
 *>  172.168.4.0/24   172.168.2.10             0             0 101 i
 * i                  172.168.3.10             0    100      0 301 i
 *   172.168.6.0/24   172.168.2.10                           0 101 301 i
 *>i                  172.168.3.10             0    100      0 301 i
 *>i 192.168.1.0      172.168.3.10             0    100      0 301 520 i
 *                    172.168.2.10                           0 101 301 520 i
 *   192.168.2.0      172.168.2.10                           0 101 301 520 i
 *>i                  172.168.3.10             0    100      0 301 520 i
--------------------------------------------
Router#sh ip bgp sum
BGP router identifier 172.168.2.1, local AS number 1001
BGP table version is 17, main routing table version 17
10 network entries using 1440 bytes of memory
19 path entries using 1520 bytes of memory
7/5 BGP path/bestpath attribute entries using 1064 bytes of memory
5 BGP AS-PATH entries using 120 bytes of memory
0 BGP route-map cache entries using 0 bytes of memory
0 BGP filter-list cache entries using 0 bytes of memory
BGP using 4144 total bytes of memory
BGP activity 10/0 prefixes, 19/0 paths, scan interval 60 secs

Neighbor        V           AS MsgRcvd MsgSent   TblVer  InQ OutQ Up/Down  State/PfxRcd
172.168.1.2     4         1001     124     123       17    0    0 01:45:27        9
172.168.2.10    4          101     137     138       17    0    0 01:57:32        8
--------------------------------------------------
Router#ping 192.168.1.1
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 192.168.1.1, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 1/1/2 ms

R15

Router#sh run | s bgp
router bgp 1001
 bgp log-neighbor-changes
 network 172.168.1.0 mask 255.255.255.0
 network 172.168.3.0 mask 255.255.255.0
 neighbor 172.168.1.1 remote-as 1001
 neighbor 172.168.3.10 remote-as 301
------------------------------------------
Router#ping 192.168.1.1
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 192.168.1.1, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 1/1/2 ms

R22

Router#sh run | s bgp
router bgp 101
 bgp log-neighbor-changes
 network 172.168.2.0 mask 255.255.255.0
 network 172.168.4.0 mask 255.255.255.0
 neighbor 172.168.2.1 remote-as 1001
 neighbor 172.168.4.1 remote-as 301

### В задании не прописано настраивать BGP между Киторном и Триадой

R21

Router#sh run | s bgp
router bgp 301
 bgp log-neighbor-changes
 network 172.168.3.0 mask 255.255.255.0
 network 172.168.4.0 mask 255.255.255.0
 network 172.168.6.0 mask 255.255.255.0
 neighbor 172.168.3.1 remote-as 1001
 neighbor 172.168.4.10 remote-as 101
 neighbor 172.168.6.2 remote-as 520

R24

Router#sh run | s bgp
router bgp 520
 bgp log-neighbor-changes
 network 30.30.30.0 mask 255.255.255.0
 network 40.40.40.0 mask 255.255.255.0
 network 172.168.6.0 mask 255.255.255.0
 neighbor 40.40.40.2 remote-as 520
 neighbor 172.168.6.1 remote-as 301
 neighbor 192.168.1.1 remote-as 2042

R23 

Router#sh ip bgp
% BGP not active

### В задании не прописано настраивать BGP во всей Триаде

R25

Router#sh ip bgp
% BGP not active

### В задании не прописано настраивать BGP во всей Триаде

R26

Router#sh run | s bgp
router bgp 520
 bgp log-neighbor-changes
 network 20.20.20.0 mask 255.255.255.0
 network 40.40.40.0 mask 255.255.255.0
 neighbor 40.40.40.1 remote-as 520
 neighbor 192.168.2.1 remote-as 2042

R18

Router#sh run | s bgp
router bgp 2042
 bgp log-neighbor-changes
 network 192.168.1.0
 network 192.168.2.0
 neighbor 192.168.1.10 remote-as 520
 neighbor 192.168.2.10 remote-as 520
---------------------------------------
Router#ping 172.168.3.1
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 172.168.3.1, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 1/1/2 ms

### Выполнение:

1 Настроил eBGP между офисом Москва и двумя провайдерами - Киторн и Ламас.
2 Настроил eBGP между провайдерами Киторн и Ламас.
3 Настроил eBGP между Ламас и Триада.
4 Настроил eBGP между офисом С.-Петербург и провайдером Триада.
5 Организовал IP доступность между пограничным роутерами офисами Москва и С.-Петербург.   Показал пингом.





