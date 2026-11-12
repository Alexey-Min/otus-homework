### Цель:

Настроить GRE поверх IPSec между офисами Москва и Санкт-Петербург, а также DMVPN поверх IPSec между Москвой, Чокурдахом и Лабытнанги.

### Задание:

Настроите GRE поверх IPSec между офисами Москва и С.-Петербург.
Настроите DMVPN поверх IPSec между Москва и Чокурдах, Лабытнанги.
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

R15#ping 192.168.3.2                                                 ---------------Все узлы в офисах в лабораторной работе должны иметь IP связность.
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 192.168.3.2, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 1/1/2 ms

R15#ping 192.168.100.2
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 192.168.100.2, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 1/1/1 ms

R15#ping 192.168.5.2
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 192.168.5.2, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 1/2/3 ms

R15#ping 192.168.6.2
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 192.168.6.2, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 1/1/1 ms

R15#ping 192.168.200.2
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 192.168.200.2, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 6/8/17 ms

R15#ping 192.168.200.3
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 192.168.200.3, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 6/6/7 ms
-----------------------------------------------------------------------
interface Tunnel123                                                    
 ip address 192.168.100.1 255.255.255.255
 tunnel source Ethernet0/2
 tunnel destination 192.168.3.2
 tunnel protection ipsec profile TUN123                                --------------Настроите GRE поверх IPSec между офисами Москва и С.-Петербург.
-----------------------------------------------------------------------
R15#sh int tun 123
Tunnel123 is up, line protocol is up
  Hardware is Tunnel
  Internet address is 192.168.100.1/32
  MTU 17916 bytes, BW 100 Kbit/sec, DLY 50000 usec,
     reliability 255/255, txload 1/255, rxload 1/255
  Encapsulation TUNNEL, loopback not set
  Keepalive not set
  Tunnel linestate evaluation down - linestate mode reg down
  Tunnel source 192.168.1.1 (Ethernet0/2), destination 192.168.3.2
   Tunnel Subblocks:
      src-track:
         Tunnel123 source tracking subblock associated with Ethernet0/2
          Set of tunnels with source Ethernet0/2, 2 members (includes iterators), on interface <OK>
  Tunnel protocol/transport GRE/IP
    Key disabled, sequencing disabled
    Checksumming of packets disabled
  Tunnel TTL 255, Fast tunneling enabled
  Tunnel transport MTU 1476 bytes
  Tunnel transmit bandwidth 8000 (kbps)
  Tunnel receive bandwidth 8000 (kbps)
  Tunnel protection via IPSec (profile "TUN123")
  Last input 00:01:26, output 00:01:46, output hang never
  Last clearing of "show interface" counters 1d02h
  Input queue: 0/75/0/0 (size/max/drops/flushes); Total output drops: 0
  Queueing strategy: fifo
  Output queue: 0/0 (size/max)
  5 minute input rate 0 bits/sec, 0 packets/sec
  5 minute output rate 0 bits/sec, 0 packets/sec
     34 packets input, 2235 bytes, 0 no buffer
     Received 0 broadcasts (0 IP multicasts)
     0 runts, 0 giants, 0 throttles
     0 input errors, 0 CRC, 0 frame, 0 overrun, 0 ignored, 0 abort
     56 packets output, 5661 bytes, 0 underruns
     0 output errors, 0 collisions, 0 interface resets
     0 unknown protocol drops
     0 output buffer failures, 0 output buffers swapped out
----------------------------------------------------------------------
interface Tunnel456    
 ip address 192.168.200.1 255.255.255.0
 no ip redirects
 ip mtu 1400
 ip nhrp map multicast dynamic
 ip nhrp network-id 200
 ip nhrp redirect
 ip tcp adjust-mss 1360
 tunnel source Ethernet0/2
 tunnel mode gre multipoint
 tunnel key 200
 tunnel protection ipsec profile TUN456                               -----------Настроите DMVPN поверх IPSec между Москва и Чокурдах, Лабытнанги
----------------------------------------------------------------------
R15#sh int tun 456
Tunnel456 is up, line protocol is up
  Hardware is Tunnel
  Internet address is 192.168.200.1/24
  MTU 17912 bytes, BW 100 Kbit/sec, DLY 50000 usec,
     reliability 255/255, txload 1/255, rxload 1/255
  Encapsulation TUNNEL, loopback not set
  Keepalive not set
  Tunnel linestate evaluation up
  Tunnel source 192.168.1.1 (Ethernet0/2)
   Tunnel Subblocks:
      src-track:
         Tunnel456 source tracking subblock associated with Ethernet0/2
          Set of tunnels with source Ethernet0/2, 2 members (includes iterators)     , on interface <OK>
  Tunnel protocol/transport multi-GRE/IP
    Key 0xC8, sequencing disabled
    Checksumming of packets disabled
  Tunnel TTL 255, Fast tunneling enabled
  Tunnel transport MTU 1472 bytes
  Tunnel transmit bandwidth 8000 (kbps)
  Tunnel receive bandwidth 8000 (kbps)
  Tunnel protection via IPSec (profile "TUN456")
  Last input 00:05:56, output 00:07:56, output hang never
  Last clearing of "show interface" counters 1d01h
  Input queue: 0/75/0/0 (size/max/drops/flushes); Total output drops: 1
  Queueing strategy: fifo
  Output queue: 0/0 (size/max)
  5 minute input rate 0 bits/sec, 0 packets/sec
  5 minute output rate 0 bits/sec, 0 packets/sec
     140 packets input, 17180 bytes, 0 no buffer
     Received 0 broadcasts (0 IP multicasts)
     0 runts, 0 giants, 0 throttles
     0 input errors, 0 CRC, 0 frame, 0 overrun, 0 ignored, 0 abort
     99 packets output, 13500 bytes, 0 underruns
     0 output errors, 0 collisions, 0 interface resets
     0 unknown protocol drops
     0 output buffer failures, 0 output buffers swapped out
-------------------------------------------------------------------------     
ip domain name otus.lab
ip name-server 192.168.6.1

crypto isakmp policy 10
 encr aes 256
 group 14
!
crypto ipsec transform-set tun123 esp-aes 256 esp-sha-hmac
 mode transport
crypto ipsec transform-set tun456 esp-aes 256 esp-sha-hmac
 mode transport
!
crypto ipsec profile TUN123
 set transform-set tun123
!
crypto ipsec profile TUN456
 set transform-set tun456

crypto pki trustpoint otus
 enrollment url http://192.168.4.1:80
 serial-number
 ip-address 192.168.1.1
 subject-name cn=R15.cer
 revocation-check none
 rsakeypair R15KEY
----------------------------------------
R15#sh crypto pki certificates
Certificate
  Status: Available
  Certificate Serial Number (hex): 06
  Certificate Usage: General Purpose
  Issuer:
    cn=otus
  Subject:
    Name: R15.otus.lab
    IP Address: 192.168.1.1
    Serial Number: 67109104
    serialNumber=67109104+ipaddress=192.168.1.1+hostname=R15.otus.lab
    cn=R15.cer
  Validity Date:
    start date: 14:37:59 UTC Nov 11 2026
    end   date: 14:37:59 UTC Nov 11 2027
  Associated Trustpoints: otus
  Storage: nvram:otus#6.cer

CA Certificate
  Status: Available
  Certificate Serial Number (hex): 01
  Certificate Usage: Signature
  Issuer:
    cn=otus
  Subject:
    cn=otus
  Validity Date:
    start date: 15:56:33 UTC Nov 10 2026
    end   date: 09:28:17 UTC Oct 2 1900
  Associated Trustpoints: otus
  Storage: nvram:otus#1CA.cer

R18

interface Tunnel123
 ip address 192.168.100.2 255.255.255.0
 tunnel source Ethernet0/2
 tunnel destination 192.168.1.1
 tunnel protection ipsec profile TUN123             --------------Настроите GRE поверх IPSec между офисами Москва и С.-Петербург.
 ---------------------------------------
R18#sh int tun 123
Tunnel123 is up, line protocol is up
  Hardware is Tunnel
  Internet address is 192.168.100.2/32
  MTU 17916 bytes, BW 100 Kbit/sec, DLY 50000 usec,
     reliability 255/255, txload 1/255, rxload 1/255
  Encapsulation TUNNEL, loopback not set
  Keepalive not set
  Tunnel linestate evaluation up
  Tunnel source 192.168.3.2 (Ethernet0/2), destination 192.168.1.1
   Tunnel Subblocks:
      src-track:
         Tunnel123 source tracking subblock associated with Ethernet0/2
          Set of tunnels with source Ethernet0/2, 1 member (includes iterators), on interface <OK>
  Tunnel protocol/transport GRE/IP
    Key disabled, sequencing disabled
    Checksumming of packets disabled
  Tunnel TTL 255, Fast tunneling enabled
  Tunnel transport MTU 1476 bytes
  Tunnel transmit bandwidth 8000 (kbps)
  Tunnel receive bandwidth 8000 (kbps)
  Tunnel protection via IPSec (profile "TUN123")
  Last input 00:01:23, output 00:01:43, output hang never
  Last clearing of "show interface" counters 1d02h
  Input queue: 0/75/0/0 (size/max/drops/flushes); Total output drops: 18
  Queueing strategy: fifo
  Output queue: 0/0 (size/max)
  5 minute input rate 0 bits/sec, 0 packets/sec
  5 minute output rate 0 bits/sec, 0 packets/sec
     13 packets input, 2230 bytes, 0 no buffer
     Received 0 broadcasts (0 IP multicasts)
     0 runts, 0 giants, 0 throttles
     0 input errors, 0 CRC, 0 frame, 0 overrun, 0 ignored, 0 abort
     18 packets output, 1467 bytes, 0 underruns
     0 output errors, 0 collisions, 0 interface resets
     0 unknown protocol drops
     0 output buffer failures, 0 output buffers swapped out
--------------------------------------------------------------------------- 
ip domain name otus.lab
ip name-server 192.168.6.1

crypto isakmp policy 10
 encr aes 256
 group 14
!
crypto ipsec transform-set tun123 esp-aes 256 esp-sha-hmac
 mode transport
!
crypto ipsec profile TUN123
 set transform-set tun123

crypto pki trustpoint otus
 enrollment url http://192.168.4.1:80
 serial-number
 ip-address 192.168.4.1
 subject-name cn=R18.cer
 revocation-check none
 rsakeypair R18KEY
---------------------------------------------------------------
R18#sh crypto pki certificates
Certificate
  Status: Available
  Certificate Serial Number (hex): 03
  Certificate Usage: General Purpose
  Issuer:
    cn=otus
  Subject:
    Name: R18.otus.lab
    IP Address: 192.168.4.1
    Serial Number: 67109152
    serialNumber=67109152+ipaddress=192.168.4.1+hostname=R18.otus.lab
    cn=R18.cer
  Validity Date:
    start date: 16:34:25 UTC Nov 9 2026
    end   date: 16:34:25 UTC Nov 9 2027
  Associated Trustpoints: otus
  Storage: nvram:otus#3.cer

CA Certificate
  Status: Available
  Certificate Serial Number (hex): 01
  Certificate Usage: Signature
  Issuer:
    cn=otus
  Subject:
    cn=otus
  Validity Date:
    start date: 15:44:39 UTC Nov 9 2026
    end   date: 09:16:23 UTC Oct 1 1900
  Associated Trustpoints: otus
  Storage: nvram:otus#1CA.cer

R1

ip domain name otus.lab
ip name-server 192.168.6.1

crypto isakmp policy 10
 encr aes 256
 group 14
!
crypto ipsec transform-set tun456 esp-aes 256 esp-sha-hmac
 mode transport
!
crypto ipsec profile TUN456
 set transform-set tun456

 crypto pki trustpoint otus
 enrollment url http://192.168.6.1:80
 serial-number
 ip-address 192.168.5.2
 subject-name cn=R1.cer
 revocation-check none
 rsakeypair R1KEY
-----------------------------------------------------------
interface Tunnel456
 ip address 192.168.200.2 255.255.255.0
 no ip redirects
 ip mtu 1400
 ip nhrp map 192.168.200.1 192.168.1.1
 ip nhrp map multicast 192.168.1.1
 ip nhrp network-id 200
 ip nhrp nhs 192.168.200.1
 ip nhrp shortcut
 ip tcp adjust-mss 1360
 tunnel source Ethernet0/0
 tunnel mode gre multipoint
 tunnel key 200
 tunnel protection ipsec profile TUN456                       -----------Настроите DMVPN поверх IPSec между Москва и Чокурдах, Лабытнанги
-------------------------------------------------------------
R1#sh crypto pki certificates
Certificate
  Status: Available
  Certificate Serial Number (hex): 04
  Certificate Usage: General Purpose
  Issuer:
    cn=otus
  Subject:
    Name: R1.otus.lab
    IP Address: 192.168.5.2
    Serial Number: 67108880
    serialNumber=67108880+hostname=R1.otus.lab+ipaddress=192.168.5.2
    cn=R1.cer
  Validity Date:
    start date: 14:12:05 UTC Nov 11 2026
    end   date: 14:12:05 UTC Nov 11 2027
  Associated Trustpoints: otus
  Storage: nvram:otus#4.cer

CA Certificate
  Status: Available
  Certificate Serial Number (hex): 01
  Certificate Usage: Signature
  Issuer:
    cn=otus
  Subject:
    cn=otus
  Validity Date:
    start date: 15:56:33 UTC Nov 10 2026
    end   date: 09:28:17 UTC Oct 2 1900
  Associated Trustpoints: otus
  Storage: nvram:otus#1CA.cer
------------------------------------------------------------------------
R1#sh int tun 456
Tunnel456 is up, line protocol is up
  Hardware is Tunnel
  Internet address is 192.168.200.2/24
  MTU 17912 bytes, BW 100 Kbit/sec, DLY 50000 usec,
     reliability 255/255, txload 1/255, rxload 1/255
  Encapsulation TUNNEL, loopback not set
  Keepalive not set
  Tunnel linestate evaluation up
  Tunnel source 192.168.5.2 (Ethernet0/0)
   Tunnel Subblocks:
      src-track:
         Tunnel456 source tracking subblock associated with Ethernet0/0
          Set of tunnels with source Ethernet0/0, 1 member (includes iterators), on interface <OK>
  Tunnel protocol/transport multi-GRE/IP
    Key 0xC8, sequencing disabled
    Checksumming of packets disabled
  Tunnel TTL 255, Fast tunneling enabled
  Tunnel transport MTU 1472 bytes
  Tunnel transmit bandwidth 8000 (kbps)
  Tunnel receive bandwidth 8000 (kbps)
  Tunnel protection via IPSec (profile "TUN456")
  Last input 00:18:51, output never, output hang never
  Last clearing of "show interface" counters 1d02h
  Input queue: 0/75/0/0 (size/max/drops/flushes); Total output drops: 0
  Queueing strategy: fifo
  Output queue: 0/0 (size/max)
  5 minute input rate 0 bits/sec, 0 packets/sec
  5 minute output rate 0 bits/sec, 0 packets/sec
     52 packets input, 7040 bytes, 0 no buffer
     Received 0 broadcasts (0 IP multicasts)
     0 runts, 0 giants, 0 throttles
     0 input errors, 0 CRC, 0 frame, 0 overrun, 0 ignored, 0 abort
     78 packets output, 9600 bytes, 0 underruns
     0 output errors, 0 collisions, 0 interface resets
     0 unknown protocol drops
     0 output buffer failures, 0 output buffers swapped out

R2

ip domain name otus.lab
ip name-server 192.168.6.1

crypto isakmp policy 10
 encr aes 256
 group 14
!
!
crypto ipsec transform-set tun456 esp-aes 256 esp-sha-hmac
 mode transport
!
crypto ipsec profile TUN456
 set transform-set tun456

 crypto pki trustpoint otus
 enrollment url http://192.168.6.1:80
 serial-number
 ip-address 192.168.6.2
 subject-name cn=R2.cer
 revocation-check none
 rsakeypair R2KEY

-------------------------------------------------------------
interface Tunnel456
 ip address 192.168.200.3 255.255.255.0
 no ip redirects
 ip mtu 1400
 ip nhrp map 192.168.200.1 192.168.1.1
 ip nhrp map multicast 192.168.1.1
 ip nhrp network-id 200
 ip nhrp nhs 192.168.200.1
 ip nhrp shortcut
 ip tcp adjust-mss 1360
 tunnel source Ethernet0/0
 tunnel mode gre multipoint
 tunnel key 200
 tunnel protection ipsec profile TUN456                     -----------Настроите DMVPN поверх IPSec между Москва и Чокурдах, Лабытнанги
------------------------------------------------------------
R2#sh crypto pki certificates
Certificate
  Status: Available
  Certificate Serial Number (hex): 05
  Certificate Usage: General Purpose
  Issuer:
    cn=otus
  Subject:
    Name: R2.otus.lab
    IP Address: 192.168.6.2
    Serial Number: 67108896
    serialNumber=67108896+hostname=R2.otus.lab+ipaddress=192.168.6.2
    cn=R2.cer
  Validity Date:
    start date: 14:13:47 UTC Nov 11 2026
    end   date: 14:13:47 UTC Nov 11 2027
  Associated Trustpoints: otus
  Storage: nvram:otus#5.cer

CA Certificate
  Status: Available
  Certificate Serial Number (hex): 01
  Certificate Usage: Signature
  Issuer:
    cn=otus
  Subject:
    cn=otus
  Validity Date:
    start date: 15:56:33 UTC Nov 10 2026
    end   date: 09:28:17 UTC Oct 2 1900
  Associated Trustpoints: otus
  Storage: nvram:otus#1CA.cer
--------------------------------------------------------
R2#sh int tun 456
Tunnel456 is up, line protocol is up
  Hardware is Tunnel
  Internet address is 192.168.200.3/24
  MTU 17912 bytes, BW 100 Kbit/sec, DLY 50000 usec,
     reliability 255/255, txload 1/255, rxload 1/255
  Encapsulation TUNNEL, loopback not set
  Keepalive not set
  Tunnel linestate evaluation up
  Tunnel source 192.168.6.2 (Ethernet0/0)
   Tunnel Subblocks:
      src-track:
         Tunnel456 source tracking subblock associated with Ethernet0/0
          Set of tunnels with source Ethernet0/0, 1 member (includes iterators), on interface <OK>
  Tunnel protocol/transport multi-GRE/IP
    Key 0xC8, sequencing disabled
    Checksumming of packets disabled
  Tunnel TTL 255, Fast tunneling enabled
  Tunnel transport MTU 1472 bytes
  Tunnel transmit bandwidth 8000 (kbps)
  Tunnel receive bandwidth 8000 (kbps)
  Tunnel protection via IPSec (profile "TUN456")
  Last input 00:19:52, output never, output hang never
  Last clearing of "show interface" counters 1d02h
  Input queue: 0/75/0/0 (size/max/drops/flushes); Total output drops: 0
  Queueing strategy: fifo
  Output queue: 0/0 (size/max)
  5 minute input rate 0 bits/sec, 0 packets/sec
  5 minute output rate 0 bits/sec, 0 packets/sec
     48 packets input, 6600 bytes, 0 no buffer
     Received 0 broadcasts (0 IP multicasts)
     0 runts, 0 giants, 0 throttles
     0 input errors, 0 CRC, 0 frame, 0 overrun, 0 ignored, 0 abort
     64 packets output, 8040 bytes, 0 underruns
     0 output errors, 0 collisions, 0 interface resets
     0 unknown protocol drops
     0 output buffer failures, 0 output buffers swapped out

R26

ip domain name otus.lab
ip host ca 192.168.6.1
ip host ca.otus.lab 192.168.6.1

crypto pki server otus
 redundancy
 no database archive
 issuer-name cn=otus
 grant auto
 lifetime ca-certificate 3650
!
crypto pki trustpoint otus
 redundancy
 revocation-check crl
 rsakeypair otus

ntp master 2
ntp update-calendar

### Выполнение:
Настроил GRE поверх IPSec между офисами Москва и С.-Петербург.
Настроил DMVPN поверх IPSec между Москва и Чокурдах, Лабытнанги.
Все узлы в офисах в лабораторной работе имеют IP связность.

