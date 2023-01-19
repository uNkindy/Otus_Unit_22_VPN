### Домашнее задание №22 (VPN)
#### Часть 1. Тестирование туннеля openvpn
1. Создан [Vagrantfile](https://github.com/uNkindy/Otus_Unit_22_VPN/blob/main/Vagrantfile), поднимающий 2 ВМ server, client;
2. Написан [playbook](https://github.com/uNkindy/Otus_Unit_22_VPN/blob/main/playbook.yml), который:
- устанавливвает необходимое ПО для стенда (epel-release, openvpn, iperf3);
- раскатывает конфигурацию openvpn на сервер и клиент;
- генерит ключ на сервере и пушит ключ на клиента;
- запускает процесс openvpn;
- создана переменная type_of_work, которая принимает значения tun или tap для автоматизации проверки работы туннеля.
3. Программой iperf3 проверим скорость работы туннелей в разных режимах:
- режим tun:
```console
Accepted connection from 10.10.10.2, port 51450
[  5] local 10.10.10.1 port 5201 connected to 10.10.10.2 port 51452
[ ID] Interval           Transfer     Bandwidth
[  5]   0.00-1.00   sec  26.9 MBytes   225 Mbits/sec                  
[  5]   1.00-2.00   sec  30.9 MBytes   259 Mbits/sec                  
[  5]   2.00-3.00   sec  31.6 MBytes   265 Mbits/sec                  
[  5]   3.00-4.00   sec  32.4 MBytes   272 Mbits/sec                  
[  5]   4.00-5.00   sec  32.5 MBytes   273 Mbits/sec                  
                  
...                  
[  5]  32.00-33.00  sec  33.9 MBytes   284 Mbits/sec                  
[  5]  33.00-34.00  sec  33.2 MBytes   278 Mbits/sec                  
[  5]  34.00-35.00  sec  33.4 MBytes   280 Mbits/sec                  
[  5]  35.00-36.00  sec  33.2 MBytes   278 Mbits/sec                  
[  5]  36.00-37.00  sec  33.2 MBytes   278 Mbits/sec                  
[  5]  37.00-38.00  sec  33.1 MBytes   278 Mbits/sec                  
[  5]  38.00-39.00  sec  32.8 MBytes   275 Mbits/sec                  
[  5]  39.00-40.00  sec  33.2 MBytes   278 Mbits/sec                  
[  5]  40.00-40.11  sec  2.54 MBytes   194 Mbits/sec                  
- - - - - - - - - - - - - - - - - - - - - - - - -
[ ID] Interval           Transfer     Bandwidth
[  5]   0.00-40.11  sec  0.00 Bytes  0.00 bits/sec                  sender
[  5]   0.00-40.11  sec  1.28 GBytes   275 Mbits/sec                  receiver
```
- в режиме tap:
```console
Accepted connection from 10.10.10.2, port 51446
[  5] local 10.10.10.1 port 5201 connected to 10.10.10.2 port 51448
[ ID] Interval           Transfer     Bandwidth
[  5]   0.00-1.00   sec  25.9 MBytes   217 Mbits/sec                  
[  5]   1.00-2.00   sec  27.5 MBytes   230 Mbits/sec                  
[  5]   2.00-3.00   sec  28.2 MBytes   236 Mbits/sec                  
[  5]   3.00-4.00   sec  28.2 MBytes   236 Mbits/sec                  
[  5]   4.00-5.00   sec  28.2 MBytes   236 Mbits/sec                  
...                
[  5]  29.00-30.00  sec  28.3 MBytes   237 Mbits/sec                  
[  5]  30.00-31.00  sec  28.4 MBytes   239 Mbits/sec                  
[  5]  31.00-32.00  sec  28.4 MBytes   238 Mbits/sec                  
[  5]  32.00-33.00  sec  28.4 MBytes   238 Mbits/sec                  
[  5]  33.00-34.00  sec  28.5 MBytes   239 Mbits/sec                  
[  5]  34.00-35.00  sec  28.5 MBytes   239 Mbits/sec                  
[  5]  35.00-36.00  sec  28.5 MBytes   239 Mbits/sec                  
[  5]  36.00-37.00  sec  28.4 MBytes   238 Mbits/sec                  
[  5]  37.00-38.00  sec  28.3 MBytes   238 Mbits/sec                  
[  5]  38.00-39.00  sec  28.1 MBytes   236 Mbits/sec                  
[  5]  39.00-40.00  sec  28.5 MBytes   239 Mbits/sec                  
[  5]  40.00-40.11  sec  3.03 MBytes   238 Mbits/sec                  
- - - - - - - - - - - - - - - - - - - - - - - - -
[ ID] Interval           Transfer     Bandwidth
[  5]   0.00-40.11  sec  0.00 Bytes  0.00 bits/sec                  sender
[  5]   0.00-40.11  sec  1.11 GBytes   237 Mbits/sec                  receiver
-----------------------------------------------------------
Server listening on 5201
-----------------------------------------------------------
```
### Вывод: в режиме tul скорость работы туннеля выше.
___
#### Часть 2. RAS на базе OpenVPN
1. Написан [playbook2](https://github.com/uNkindy/Otus_Unit_22_VPN/blob/main/playbook2.yml) для создания туннеля на базе server;
2. Подключаемся с хоста на ВМ server при помощи openvpn:
```console
[root@devops tmp]# openvpn --config client.conf
Wed Jan 18 23:48:41 2023 WARNING: file './client.key' is group or others accessible
Wed Jan 18 23:48:41 2023 OpenVPN 2.4.12 x86_64-redhat-linux-gnu [Fedora EPEL patched] [SSL (OpenSSL)] [LZO] [LZ4] [EPOLL] [PKCS11] [MH/PKTINFO] [AEAD] built on Mar 17 2022
Wed Jan 18 23:48:41 2023 library versions: OpenSSL 1.0.2k-fips  26 Jan 2017, LZO 2.06
Wed Jan 18 23:48:41 2023 WARNING: No server certificate verification method has been enabled.  See http://openvpn.net/howto.html#mitm for more info.
Wed Jan 18 23:48:41 2023 TCP/UDP: Preserving recently used remote address: [AF_INET]192.168.10.10:1207
Wed Jan 18 23:48:41 2023 Socket Buffers: R=[212992->212992] S=[212992->212992]
Wed Jan 18 23:48:41 2023 UDP link local (bound): [AF_INET][undef]:1194
Wed Jan 18 23:48:41 2023 UDP link remote: [AF_INET]192.168.10.10:1207
Wed Jan 18 23:48:41 2023 TLS: Initial packet from [AF_INET]192.168.10.10:1207, sid=93021fed 8c98a726
Wed Jan 18 23:48:41 2023 VERIFY OK: depth=1, CN=rasvpn
Wed Jan 18 23:48:41 2023 VERIFY OK: depth=0, CN=rasvpn
Wed Jan 18 23:48:41 2023 Control Channel: TLSv1.2, cipher TLSv1/SSLv3 ECDHE-RSA-AES256-GCM-SHA384, 2048 bit RSA
Wed Jan 18 23:48:41 2023 [rasvpn] Peer Connection Initiated with [AF_INET]192.168.10.10:1207
Wed Jan 18 23:48:42 2023 SENT CONTROL [rasvpn]: 'PUSH_REQUEST' (status=1)
Wed Jan 18 23:48:42 2023 PUSH: Received control message: 'PUSH_REPLY,route 192.168.10.0 255.255.255.0,route 10.10.10.0 255.255.255.0,topology net30,ping 10,ping-restart 120,ifconfig 10.10.10.6 10.10.10.5,peer-id 0,cipher AES-256-GCM'
Wed Jan 18 23:48:42 2023 OPTIONS IMPORT: timers and/or timeouts modified
Wed Jan 18 23:48:42 2023 OPTIONS IMPORT: --ifconfig/up options modified
Wed Jan 18 23:48:42 2023 OPTIONS IMPORT: route options modified
Wed Jan 18 23:48:42 2023 OPTIONS IMPORT: peer-id set
Wed Jan 18 23:48:42 2023 OPTIONS IMPORT: adjusting link_mtu to 1625
Wed Jan 18 23:48:42 2023 OPTIONS IMPORT: data channel crypto options modified
Wed Jan 18 23:48:42 2023 Data Channel: using negotiated cipher 'AES-256-GCM'
Wed Jan 18 23:48:42 2023 Outgoing Data Channel: Cipher 'AES-256-GCM' initialized with 256 bit key
Wed Jan 18 23:48:42 2023 Incoming Data Channel: Cipher 'AES-256-GCM' initialized with 256 bit key
Wed Jan 18 23:48:42 2023 ROUTE_GATEWAY 192.168.2.1/255.255.255.0 IFACE=p1p4 HWADDR=b4:96:91:95:44:07
Wed Jan 18 23:48:42 2023 TUN/TAP device tun0 opened
Wed Jan 18 23:48:42 2023 TUN/TAP TX queue length set to 100
Wed Jan 18 23:48:42 2023 /sbin/ip link set dev tun0 up mtu 1500
Wed Jan 18 23:48:42 2023 /sbin/ip addr add dev tun0 local 10.10.10.6 peer 10.10.10.5
Wed Jan 18 23:48:42 2023 /sbin/ip route add 192.168.10.0/24 via 10.10.10.5
RTNETLINK answers: File exists
Wed Jan 18 23:48:42 2023 ERROR: Linux route add command failed: external program exited with error status: 2
Wed Jan 18 23:48:42 2023 /sbin/ip route add 192.168.10.0/24 via 10.10.10.5
RTNETLINK answers: File exists
Wed Jan 18 23:48:42 2023 ERROR: Linux route add command failed: external program exited with error status: 2
Wed Jan 18 23:48:42 2023 /sbin/ip route add 10.10.10.0/24 via 10.10.10.5
Wed Jan 18 23:48:42 2023 WARNING: this configuration may cache passwords in memory -- use the auth-nocache option to prevent this
Wed Jan 18 23:48:42 2023 Initialization Sequence Completed
```
Подключение прошло успешно, проверим пинг:
```console
[root@server vagrant]# ping 10.10.10.1
PING 10.10.10.1 (10.10.10.1) 56(84) bytes of data.
64 bytes from 10.10.10.1: icmp_seq=1 ttl=64 time=0.009 ms
64 bytes from 10.10.10.1: icmp_seq=2 ttl=64 time=0.063 ms
^C
--- 10.10.10.1 ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1004ms
rtt min/avg/max/mdev = 0.009/0.036/0.063/0.027 ms
```
И проверим наличие туннеля командой ip -c a:
```console
10: tun0: <POINTOPOINT,MULTICAST,NOARP,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UNKNOWN group default qlen 100
    link/none 
    inet 10.10.10.1 peer 10.10.10.2/32 scope global tun0
       valid_lft forever preferred_lft forever
    inet6 fe80::a956:9500:af38:ab1f/64 scope link flags 800 
       valid_lft forever preferred_lft forever
```
