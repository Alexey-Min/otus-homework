### Цель:
разобрать различные варианты маршрутизации в DMVPN

### Задание:
Настроите GRE между офисами Москва и С.-Петербург.
Настроите DMVMN между Москва и Чокурдах, Лабытнанги.
Все узлы в офисах в лабораторной работе должны иметь IP связность.

### Общая таблица сетевых адресов на интерфейсах.

| наименование | интерфейс | ip address      | Сеть

| R15          | e0/2      | 192.168.1.1/24  | 192.168.1.0/24

| R21          | e0/0      | 192.168.1.2/24  | 192.168.1.0/24

| R21          | e0/2      | 192.168.2.1/24  | 192.168.2.0/24

| R24          | e0/0      | 192.168.2.2/24  | 192.168.2.0/24

| R24          | e0/1      | 172.168.2.1/24  | 172.168.2.0/24

| R24          | e0/2      | 172.168.1.2/24  | 172.168.1.0/24

| R24          | e0/3      | 192.168.3.1/24  | 192.168.3.0/24

| R23          | e0/1      | 172.168.4.1/24  | 172.168.4.0/24

| R23          | e0/2      | 172.168.1.1/24  | 172.168.1.0/24

| R26          | e0/0      | 172.168.2.2/24  | 172.168.2.0/24

| R26          | e0/1      | 192.168.6.1/24  | 192.168.6.0/24

| R26          | e0/2      | 172.168.3.2/24  | 172.168.3.0/24

| R26          | e0/3      | 192.168.4.1/24  | 192.168.4.0/24

| R25          | e0/0      | 172.168.4.2/24  | 172.168.4.0/24

| R25          | e0/1      | 192.168.5.1/24  | 192.168.5.0/24

| R25          | e0/2      | 172.168.3.1/24  | 172.168.3.0/24

| R25          | e0/3      | 192.168.7.1/24  | 192.168.7.0/24

| R18          | e0/2      | 192.168.3.2/24  | 192.168.3.0/24

| R18          | e0/3      | 192.168.4.2/24  | 192.168.4.0/24

| R2           | e0/0      | 192.168.6.2/24  | 192.168.6.0/24

| R2           | e0/1      | 192.168.7.2/24  | 192.168.7.0/24

| R2           | e0/0      | 192.168.5.2/24  | 192.168.5.0/24

### Пример настроек:

R15

Router#ping 192.168.3.2                                            ----------------Все узлы в офисах в лабораторной работе должны иметь IP связность.
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 192.168.3.2, timeout is 2 seconds:
...!!
Success rate is 40 percent (2/5), round-trip min/avg/max = 3/3/3 ms
Router#ping 192.168.100.2
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 192.168.100.2, timeout is 2 seconds:
.!!!!
Success rate is 80 percent (4/5), round-trip min/avg/max = 1/1/2 ms
Router#ping 192.168.6.2
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 192.168.6.2, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 1/1/2 ms
Router#ping 192.168.200.3
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 192.168.200.3, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 2/2/3 ms
Router#ping 192.168.5.2
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 192.168.5.2, timeout is 2 seconds:
..!!!
Success rate is 60 percent (3/5), round-trip min/avg/max = 4/4/5 ms
Router#ping 192.168.200.2
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 192.168.200.2, timeout is 2 seconds:
..!!!
Success rate is 60 percent (3/5), round-trip min/avg/max = 4/4/5 ms
-----------------------------------------------------------------------
interface Tunnel15                                                     ---------------Настроите GRE между офисами Москва и С.-Петербург.
 ip address 192.168.100.1 255.255.255.0
 ip mtu 1400
 ip tcp adjust-mss 1360
 keepalive 50 10
 tunnel source 192.168.1.1
 tunnel destination 192.168.3.2
-----------------------------------------------------------------------
Router#sh int tun 15
Tunnel15 is up, line protocol is down
  Hardware is Tunnel
  Internet address is 192.168.100.1/24
  MTU 17916 bytes, BW 100 Kbit/sec, DLY 50000 usec,
     reliability 255/255, txload 1/255, rxload 1/255
  Encapsulation TUNNEL, loopback not set
  Keepalive set (50 sec), retries 10
  Tunnel linestate evaluation down - keepalive down
  Tunnel source 192.168.1.1, destination 192.168.3.2
  Tunnel protocol/transport GRE/IP
    Key disabled, sequencing disabled
    Checksumming of packets disabled
  Tunnel TTL 255, Fast tunneling enabled
  Tunnel transport MTU 1476 bytes
  Tunnel transmit bandwidth 8000 (kbps)
  Tunnel receive bandwidth 8000 (kbps)
  Last input 00:06:12, output 00:00:25, output hang never
  Last clearing of "show interface" counters 00:08:55
  Input queue: 0/75/0/0 (size/max/drops/flushes); Total output drops: 0
  Queueing strategy: fifo
  Output queue: 0/0 (size/max)
  5 minute input rate 0 bits/sec, 0 packets/sec
  5 minute output rate 0 bits/sec, 0 packets/sec
     4 packets input, 496 bytes, 0 no buffer                                ---------------
     Received 0 broadcasts (0 IP multicasts)
     0 runts, 0 giants, 0 throttles
     0 input errors, 0 CRC, 0 frame, 0 overrun, 0 ignored, 0 abort
     15 packets output, 1100 bytes, 0 underruns                             ----------------
     0 output errors, 0 collisions, 0 interface resets
     0 unknown protocol drops
     0 output buffer failures, 0 output buffers swapped out
-----------------------------------------------------------------------------
interface Tunnel200                                                          ---------------Настроите DMVMN между Москва и Чокурдах, Лабытнанги.
 ip address 192.168.200.1 255.255.255.0
 no ip redirects
 ip mtu 1400
 ip nhrp map multicast dynamic
 ip nhrp network-id 200
 ip nhrp redirect
 ip tcp adjust-mss 1360
 tunnel source 192.168.1.1
 tunnel mode gre multipoint
-----------------------------------------------------------------------------
Router#sh int tun 200
Tunnel200 is up, line protocol is up
  Hardware is Tunnel
  Internet address is 192.168.200.1/24
  MTU 17916 bytes, BW 100 Kbit/sec, DLY 50000 usec,
     reliability 255/255, txload 1/255, rxload 1/255
  Encapsulation TUNNEL, loopback not set
  Keepalive not set
  Tunnel linestate evaluation up
  Tunnel source 192.168.1.1
  Tunnel protocol/transport multi-GRE/IP
    Key disabled, sequencing disabled
    Checksumming of packets disabled
  Tunnel TTL 255, Fast tunneling enabled
  Tunnel transport MTU 1476 bytes
  Tunnel transmit bandwidth 8000 (kbps)
  Tunnel receive bandwidth 8000 (kbps)
  Last input 00:08:01, output 00:08:01, output hang never
  Last clearing of "show interface" counters 00:12:05
  Input queue: 0/75/0/0 (size/max/drops/flushes); Total output drops: 0
  Queueing strategy: fifo
  Output queue: 0/0 (size/max)
  5 minute input rate 0 bits/sec, 0 packets/sec
  5 minute output rate 0 bits/sec, 0 packets/sec
     10 packets input, 1224 bytes, 0 no buffer                            -----------
     Received 0 broadcasts (0 IP multicasts)
     0 runts, 0 giants, 0 throttles
     0 input errors, 0 CRC, 0 frame, 0 overrun, 0 ignored, 0 abort
     12 packets output, 1512 bytes, 0 underruns                           ------------
     0 output errors, 0 collisions, 0 interface resets
     0 unknown protocol drops
     0 output buffer failures, 0 output buffers swapped out
--------------------------------------------------------------------------
Router#sh dmvpn
Legend: Attrb --> S - Static, D - Dynamic, I - Incomplete
        N - NATed, L - Local, X - No Socket
        T1 - Route Installed, T2 - Nexthop-override
        C - CTS Capable
        # Ent --> Number of NHRP entries with same NBMA peer
        NHS Status: E --> Expecting Replies, R --> Responding, W --> Waiting
        UpDn Time --> Up or Down Time for a Tunnel
==========================================================================

Interface: Tunnel200, IPv4 NHRP Details
Type:Hub, NHRP Peers:2,

 # Ent  Peer NBMA Addr Peer Tunnel Add State  UpDn Tm Attrb
 ----- --------------- --------------- ----- -------- -----
     1 192.168.5.2       192.168.200.2    UP 00:08:56     D
     1 192.168.6.2       192.168.200.3    UP 00:09:47     D
------------------------------------------------------------------------------
Router#sh ip nhrp
192.168.200.2/32 via 192.168.200.2
   Tunnel200 created 00:10:09, expire 01:50:09
   Type: dynamic, Flags: unique registered used nhop
   NBMA address: 192.168.5.2
192.168.200.3/32 via 192.168.200.3
   Tunnel200 created 00:10:41, expire 01:49:18
   Type: dynamic, Flags: unique registered used nhop
   NBMA address: 192.168.6.2

R18

interface Tunnel18                                                           --------------Настроите GRE между офисами Москва и С.-Петербург.
 ip address 192.168.100.2 255.255.255.0
 ip mtu 1400
 ip tcp adjust-mss 1360
 keepalive 50 10
 tunnel source 192.168.3.2
 tunnel destination 192.168.1.1
----------------------------------------------------------------------------
Router#sh int tun 18
Tunnel18 is up, line protocol is down
  Hardware is Tunnel
  Internet address is 192.168.100.2/24
  MTU 17916 bytes, BW 100 Kbit/sec, DLY 50000 usec,
     reliability 255/255, txload 1/255, rxload 1/255
  Encapsulation TUNNEL, loopback not set
  Keepalive set (50 sec), retries 10
  Tunnel linestate evaluation down - keepalive down
  Tunnel source 192.168.3.2, destination 192.168.1.1
  Tunnel protocol/transport GRE/IP
    Key disabled, sequencing disabled
    Checksumming of packets disabled
  Tunnel TTL 255, Fast tunneling enabled
  Tunnel transport MTU 1476 bytes
  Tunnel transmit bandwidth 8000 (kbps)
  Tunnel receive bandwidth 8000 (kbps)
  Last input 00:00:08, output 00:00:09, output hang never
  Last clearing of "show interface" counters 00:16:59
  Input queue: 0/75/0/0 (size/max/drops/flushes); Total output drops: 0
  Queueing strategy: fifo
  Output queue: 0/0 (size/max)
  5 minute input rate 0 bits/sec, 0 packets/sec
  5 minute output rate 0 bits/sec, 0 packets/sec
     25 packets input, 1580 bytes, 0 no buffer                                         ------------------
     Received 0 broadcasts (0 IP multicasts)
     0 runts, 0 giants, 0 throttles
     0 input errors, 0 CRC, 0 frame, 0 overrun, 0 ignored, 0 abort
     25 packets output, 1580 bytes, 0 underruns                                        ------------------
     0 output errors, 0 collisions, 0 interface resets
     0 unknown protocol drops
     0 output buffer failures, 0 output buffers swapped out

R2

interface Tunnel200                                                                   --------------------Настроите DMVMN между Москва и Чокурдах, Лабытнанги.
 ip address 192.168.200.3 255.255.255.0
 no ip redirects
 ip mtu 1400
 ip nhrp map multicast 192.168.1.1
 ip nhrp map 192.168.200.1 192.168.1.1
 ip nhrp network-id 200
 ip nhrp nhs 192.168.200.1
 ip nhrp shortcut
 ip tcp adjust-mss 1360
 tunnel source 192.168.6.2
 tunnel mode gre multipoint
---------------------------------------------------------------------------------------
Router#sh int tun 200
Tunnel200 is up, line protocol is up
  Hardware is Tunnel
  Internet address is 192.168.200.3/24
  MTU 17916 bytes, BW 100 Kbit/sec, DLY 50000 usec,
     reliability 255/255, txload 1/255, rxload 1/255
  Encapsulation TUNNEL, loopback not set
  Keepalive not set
  Tunnel linestate evaluation up
  Tunnel source 192.168.6.2
  Tunnel protocol/transport multi-GRE/IP
    Key disabled, sequencing disabled
    Checksumming of packets disabled
  Tunnel TTL 255, Fast tunneling enabled
  Tunnel transport MTU 1476 bytes
  Tunnel transmit bandwidth 8000 (kbps)
  Tunnel receive bandwidth 8000 (kbps)
  Last input 00:15:41, output 00:15:41, output hang never
  Last clearing of "show interface" counters 00:18:47
  Input queue: 0/75/0/0 (size/max/drops/flushes); Total output drops: 0
  Queueing strategy: fifo
  Output queue: 0/0 (size/max)
  5 minute input rate 0 bits/sec, 0 packets/sec
  5 minute output rate 0 bits/sec, 0 packets/sec
     6 packets input, 756 bytes, 0 no buffer                                               ---------------
     Received 0 broadcasts (0 IP multicasts)
     0 runts, 0 giants, 0 throttles
     0 input errors, 0 CRC, 0 frame, 0 overrun, 0 ignored, 0 abort
     14 packets output, 1664 bytes, 0 underruns                                            ----------------
     0 output errors, 0 collisions, 0 interface resets
     0 unknown protocol drops
     0 output buffer failures, 0 output buffers swapped out
------------------------------------------------------------------------------------------
Router#sh dmvpn
Legend: Attrb --> S - Static, D - Dynamic, I - Incomplete
        N - NATed, L - Local, X - No Socket
        T1 - Route Installed, T2 - Nexthop-override
        C - CTS Capable
        # Ent --> Number of NHRP entries with same NBMA peer
        NHS Status: E --> Expecting Replies, R --> Responding, W --> Waiting
        UpDn Time --> Up or Down Time for a Tunnel
==========================================================================

Interface: Tunnel200, IPv4 NHRP Details
Type:Spoke, NHRP Peers:2,

 # Ent  Peer NBMA Addr Peer Tunnel Add State  UpDn Tm Attrb
 ----- --------------- --------------- ----- -------- -----
     1 192.168.1.1       192.168.200.1    UP 00:17:17     S
     1 192.168.5.2       192.168.200.2    UP 00:00:08     D
-------------------------------------------------------------------------------------------
Router#sh ip nhrp
192.168.200.1/32 via 192.168.200.1
   Tunnel200 created 00:26:33, never expire
   Type: static, Flags: used
   NBMA address: 192.168.1.1
192.168.200.2/32 via 192.168.200.2
   Tunnel200 created 00:06:19, expire 01:53:40
   Type: dynamic, Flags: router used nhop
   NBMA address: 192.168.5.2
192.168.200.3/32 via 192.168.200.3
   Tunnel200 created 00:00:15, expire 00:02:49
   Type: dynamic, Flags: used temporary
   NBMA address: 192.168.1.1

R1

interface Tunnel200                                                          -------------------Настроите DMVMN между Москва и Чокурдах, Лабытнанги.
 ip address 192.168.200.2 255.255.255.0
 no ip redirects
 ip mtu 1400
 ip nhrp map multicast 192.168.1.1
 ip nhrp map 192.168.200.1 192.168.1.1
 ip nhrp network-id 200
 ip nhrp nhs 192.168.200.1
 ip nhrp shortcut
 ip tcp adjust-mss 1360
 tunnel source 192.168.5.2
 tunnel mode gre multipoint
--------------------------------------------------------------------------------
Router#sh int tun 200
Tunnel200 is up, line protocol is up
  Hardware is Tunnel
  Internet address is 192.168.200.2/24
  MTU 17916 bytes, BW 100 Kbit/sec, DLY 50000 usec,
     reliability 255/255, txload 1/255, rxload 1/255
  Encapsulation TUNNEL, loopback not set
  Keepalive not set
  Tunnel linestate evaluation up
  Tunnel source 192.168.5.2
  Tunnel protocol/transport multi-GRE/IP
    Key disabled, sequencing disabled
    Checksumming of packets disabled
  Tunnel TTL 255, Fast tunneling enabled
  Tunnel transport MTU 1476 bytes
  Tunnel transmit bandwidth 8000 (kbps)
  Tunnel receive bandwidth 8000 (kbps)
  Last input 00:01:00, output 00:01:00, output hang never
  Last clearing of "show interface" counters 00:24:34
  Input queue: 0/75/0/0 (size/max/drops/flushes); Total output drops: 0
  Queueing strategy: fifo
  Output queue: 0/0 (size/max)
  5 minute input rate 0 bits/sec, 0 packets/sec
  5 minute output rate 0 bits/sec, 0 packets/sec
     20 packets input, 2468 bytes, 0 no buffer                                   --------------
     Received 0 broadcasts (0 IP multicasts)
     0 runts, 0 giants, 0 throttles
     0 input errors, 0 CRC, 0 frame, 0 overrun, 0 ignored, 0 abort
     31 packets output, 3784 bytes, 0 underruns                                  --------------
     0 output errors, 0 collisions, 0 interface resets
     0 unknown protocol drops
     0 output buffer failures, 0 output buffers swapped out
-----------------------------------------------------------------------------------
Router#sh ip nhrp
192.168.200.1/32 via 192.168.200.1
   Tunnel200 created 00:23:41, never expire
   Type: static, Flags: used
   NBMA address: 192.168.1.1
192.168.200.2/32 via 192.168.200.2
   Tunnel200 created 00:03:27, expire 01:56:32
   Type: dynamic, Flags: router unique local
   NBMA address: 192.168.5.2
    (no-socket)
192.168.200.3/32 via 192.168.200.3
   Tunnel200 created 00:03:27, expire 01:56:32
   Type: dynamic, Flags: router implicit used nhop
   NBMA address: 192.168.6.2
-------------------------------------------------------------------------------------
Router#sh dmvpn
Legend: Attrb --> S - Static, D - Dynamic, I - Incomplete
        N - NATed, L - Local, X - No Socket
        T1 - Route Installed, T2 - Nexthop-override
        C - CTS Capable
        # Ent --> Number of NHRP entries with same NBMA peer
        NHS Status: E --> Expecting Replies, R --> Responding, W --> Waiting
        UpDn Time --> Up or Down Time for a Tunnel
==========================================================================

Interface: Tunnel200, IPv4 NHRP Details
Type:Spoke, NHRP Peers:2,

 # Ent  Peer NBMA Addr Peer Tunnel Add State  UpDn Tm Attrb
 ----- --------------- --------------- ----- -------- -----
     1 192.168.1.1       192.168.200.1    UP 00:21:27     S
     1 192.168.6.2       192.168.200.3    UP 00:05:08     D

### Выполнение:
Настроил GRE между офисами Москва и С.-Петербург.
Настроил DMVMN между Москва и Чокурдах, Лабытнанги.
Все узлы в офисах в лабораторной работе имеют IP связность.
