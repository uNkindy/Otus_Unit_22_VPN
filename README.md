### Домашнее задание №22 (VPN)
#### Часть 1. Тестирование туннеля openvpn
1. Создан [Vagrantfile](), поднимающий 2 ВМ server, client;
2. Написан [playbook], который:
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
