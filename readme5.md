1. Подключитесь к публичному маршрутизатору в интернет. Найдите маршрут к вашему публичному IP

route-views>show ip rout 95.37.0.65                
Routing entry for 95.37.0.0/18
  Known via "bgp 6447", distance 20, metric 0
  Tag 2497, type external
  Last update from 202.232.0.2 7w0d ago
  Routing Descriptor Blocks:
  * 202.232.0.2, from 202.232.0.2, 7w0d ago
      Route metric is 0, traffic share count is 1
      AS Hops 2
      Route tag 2497
      MPLS label: none
route-views>show bgp camera P:Q
route-views>P?
*p=ping
pad  ping  port-channel  ppp


route-views>show bgp 95.37.0.65                    
BGP routing table entry for 95.37.0.0/18, version 281622434
Paths: (24 available, best #20, table default)
  Not advertised to any peer
  Refresh Epoch 1
  4901 6079 1299 12389
    162.250.137.254 from 162.250.137.254 (162.250.137.254)
      Origin IGP, localpref 100, valid, external
      Community: 65000:10100 65000:10300 65000:10400
      path 7FE1279B16E8 RPKI State not found
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 1
  53767 174 12389
    162.251.163.2 from 162.251.163.2 (162.251.162.3)
      Origin IGP, localpref 100, valid, external
      Community: 174:21101 174:22005 53767:5000
      path 7FE135AEF860 RPKI State not found
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 1
  20912 3257 1273 12389
    212.66.96.126 from 212.66.96.126 (212.66.96.126)
      Origin IGP, localpref 100, valid, external
      Community: 3257:8070 3257:30352 3257:50001 3257:53900 3257:53902 20912:65004
      path 7FE0192E74F8 RPKI State not found
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 1
  3561 3910 3356 12389
    206.24.210.80 from 206.24.210.80 (206.24.210.80)
      Origin IGP, localpref 100, valid, external
      path 7FE123608608 RPKI State not found
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 1
  701 1273 12389
    137.39.3.55 from 137.39.3.55 (137.39.3.55)
      Origin IGP, localpref 100, valid, external
      path 7FE0C1F22388 RPKI State not found
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 1
  8283 1299 12389
    94.142.247.3 from 94.142.247.3 (94.142.247.3)
      Origin IGP, metric 0, localpref 100, valid, external
      Community: 1299:30000 8283:1 8283:101
      unknown transitive attribute: flag 0xE0 type 0x20 length 0x18
        value 0000 205B 0000 0000 0000 0001 0000 205B
              0000 0005 0000 0001 
      path 7FE16003C150 RPKI State not found
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 1
  3356 12389
    4.68.4.46 from 4.68.4.46 (4.69.184.201)
      Origin IGP, metric 0, localpref 100, valid, external
      Community: 3356:2 3356:22 3356:100 3356:123 3356:501 3356:903 3356:2065
      path 7FE0F5ECAA48 RPKI State not found
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 1
  3549 3356 12389
    208.51.134.254 from 208.51.134.254 (67.16.168.191)
      Origin IGP, metric 0, localpref 100, valid, external
      Community: 3356:2 3356:22 3356:100 3356:123 3356:501 3356:903 3356:2065 3549:2581 3549:30840
      path 7FE127398F18 RPKI State not found
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 1
  3267 1299 12389
    194.85.40.15 from 194.85.40.15 (185.141.126.1)
      Origin IGP, metric 0, localpref 100, valid, external
      path 7FE1373D9C10 RPKI State not found
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 1
  6939 12389
    64.71.137.241 from 64.71.137.241 (216.218.252.164)
      Origin IGP, localpref 100, valid, external
      path 7FE0FA0C65E8 RPKI State not found
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 1
  3333 1103 12389
    193.0.0.56 from 193.0.0.56 (193.0.0.56)
      Origin IGP, localpref 100, valid, external
      path 7FE162D7CB30 RPKI State not found
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 1
  7018 3356 12389
    12.0.1.63 from 12.0.1.63 (12.0.1.63)
      Origin IGP, localpref 100, valid, external
      Community: 7018:5000 7018:37232
      path 7FE141A3B2B8 RPKI State not found
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 1
  19214 174 12389
    208.74.64.40 from 208.74.64.40 (208.74.64.40)
      Origin IGP, localpref 100, valid, external
      Community: 174:21101 174:22005
      path 7FE187565A98 RPKI State not found
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 1
  20130 6939 12389
    140.192.8.16 from 140.192.8.16 (140.192.8.16)
      Origin IGP, localpref 100, valid, external
      path 7FE001128238 RPKI State not found
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 1
  101 174 12389
    209.124.176.223 from 209.124.176.223 (209.124.176.223)
      Origin IGP, localpref 100, valid, external
      Community: 101:20100 101:20110 101:22100 174:21101 174:22005
      Extended Community: RT:101:22100
      path 7FE0E78BE358 RPKI State not found
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 1
  1351 6939 12389
    132.198.255.253 from 132.198.255.253 (132.198.255.253)
      Origin IGP, localpref 100, valid, external
      path 7FE15B716D88 RPKI State not found
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 1
  852 3356 12389
    154.11.12.212 from 154.11.12.212 (96.1.209.43)
      Origin IGP, metric 0, localpref 100, valid, external
      path 7FE12EEE1C08 RPKI State not found
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 1
  57866 3356 12389
    37.139.139.17 from 37.139.139.17 (37.139.139.17)
      Origin IGP, metric 0, localpref 100, valid, external
      Community: 3356:2 3356:22 3356:100 3356:123 3356:501 3356:903 3356:2065
      path 7FE046C1E810 RPKI State not found
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 1
  3303 12389
    217.192.89.50 from 217.192.89.50 (138.187.128.158)
      Origin IGP, localpref 100, valid, external
      Community: 3303:1004 3303:1006 3303:1030 3303:3056
      path 7FE1240A3F48 RPKI State not found
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 2
  2497 12389
    202.232.0.2 from 202.232.0.2 (58.138.96.254)
      Origin IGP, localpref 100, valid, external, best
      path 7FE17FD79B88 RPKI State not found
      rx pathid: 0, tx pathid: 0x0
  Refresh Epoch 1
  7660 2516 12389
    203.181.248.168 from 203.181.248.168 (203.181.248.168)
      Origin IGP, localpref 100, valid, external
      Community: 2516:1050 7660:9003
      path 7FE0D6E962B8 RPKI State not found
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 1
  49788 12552 12389
    91.218.184.60 from 91.218.184.60 (91.218.184.60)
      Origin IGP, localpref 100, valid, external
      Community: 12552:12000 12552:12100 12552:12101 12552:22000
      Extended Community: 0x43:100:1
      path 7FE1318E7710 RPKI State not found
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 1
  1221 4637 5511 12389
    203.62.252.83 from 203.62.252.83 (203.62.252.83)
      Origin IGP, localpref 100, valid, external
      path 7FE01C72A3B8 RPKI State not found
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 1
  3257 1299 12389
    89.149.178.10 from 89.149.178.10 (213.200.83.26)
      Origin IGP, metric 10, localpref 100, valid, external
      Community: 3257:8794 3257:30052 3257:50001 3257:54900 3257:54901
      path 7FE1872A7788 RPKI State not found
      rx pathid: 0, tx pathid: 0
route-views> 

2. Создайте dummy0 интерфейс в Ubuntu. Добавьте несколько статических маршрутов. Проверьте таблицу маршрутизации.

# modprobe -v dummy numdummies=2
# ip link add dummy0 type dummy
# ip addr add 10.1.1.1/32 dev dummy0
# ip link set dummy0 up

# ip route add 192.168.0.0/24 via 10.1.1.1
# ip route add 8.8.8.0/24 via 10.1.1.1
# ip route add 10.1.1.1/32 via 192.168.0.1

# ip route
default via 192.168.0.1 dev enp4s0f1 proto dhcp metric 20100 
default via 192.168.0.1 dev wlp3s0 proto dhcp metric 20600 
8.8.8.0/24 via 10.1.1.1 dev dummy0 
10.1.1.1 via 192.168.0.1 dev enp4s0f1 
169.254.0.0/16 dev wlp3s0 scope link metric 1000 
192.168.0.0/24 via 10.1.1.1 dev dummy0 
192.168.0.0/24 dev enp4s0f1 proto kernel scope link src 192.168.0.163 metric 100 
192.168.0.0/24 dev wlp3s0 proto kernel scope link src 192.168.0.185 metric 600 

3. Проверьте открытые TCP порты в Ubuntu, какие протоколы и приложения используют эти порты? Приведите несколько примеров.

$ ss -tnlp
State     Recv-Q    Send-Q       Local Address:Port        Peer Address:Port   Process    
LISTEN    0         4096         127.0.0.53%lo:53               0.0.0.0:*                 
LISTEN    0         5                127.0.0.1:631              0.0.0.0:*                 
LISTEN    0         4096             127.0.0.1:8125             0.0.0.0:*                 
LISTEN    0         50                 0.0.0.0:445              0.0.0.0:*                 
LISTEN    0         4096               0.0.0.0:19999            0.0.0.0:*                 
LISTEN    0         5                  0.0.0.0:902              0.0.0.0:*                 
LISTEN    0         50                 0.0.0.0:139              0.0.0.0:*                 
LISTEN    0         5                    [::1]:631                 [::]:*                 
LISTEN    0         4096                     *:3000                   *:*                 
LISTEN    0         50                    [::]:445                 [::]:*                 
LISTEN    0         4096                     *:9090                   *:*                 
LISTEN    0         5                     [::]:902                 [::]:*                 
LISTEN    0         50                    [::]:139                 [::]:*                 
LISTEN    0         4096                     *:9100                   *:*                 

:53 - DNS
:139 - NETBIOS-SSN

4. Проверьте используемые UDP сокеты в Ubuntu, какие протоколы и приложения используют эти порты?

ss -unap
State  Recv-Q Send-Q                       Local Address:Port   Peer Address:Port Process 
UNCONN 0      0                                  0.0.0.0:5353        0.0.0.0:*            
UNCONN 0      0                                  0.0.0.0:51290       0.0.0.0:*            
UNCONN 0      0                                127.0.0.1:8125        0.0.0.0:*            
UNCONN 0      0                            127.0.0.53%lo:53          0.0.0.0:*            
ESTAB  0      0                   192.168.0.163%enp4s0f1:68      192.168.0.1:67           
ESTAB  0      0                     192.168.0.185%wlp3s0:68      192.168.0.1:67           
UNCONN 0      0                            192.168.0.255:137         0.0.0.0:*            
UNCONN 0      0                            192.168.0.163:137         0.0.0.0:*            
UNCONN 0      0                            192.168.0.255:137         0.0.0.0:*            
UNCONN 0      0                            192.168.0.185:137         0.0.0.0:*            
UNCONN 0      0                                  0.0.0.0:137         0.0.0.0:*            
UNCONN 0      0                            192.168.0.255:138         0.0.0.0:*            
UNCONN 0      0                            192.168.0.163:138         0.0.0.0:*            
UNCONN 0      0                            192.168.0.255:138         0.0.0.0:*            
UNCONN 0      0                            192.168.0.185:138         0.0.0.0:*            
UNCONN 0      0                                  0.0.0.0:138         0.0.0.0:*            
UNCONN 0      0                                127.0.0.1:323         0.0.0.0:*            
UNCONN 0      0                                  0.0.0.0:631         0.0.0.0:*            
UNCONN 0      0                                     [::]:5353           [::]:*            
UNCONN 0      0                                     [::]:55066          [::]:*            
UNCONN 0      0                                    [::1]:323            [::]:*            
UNCONN 0      0      [fe80::fd27:c931:70aa:479]%enp4s0f1:546            [::]:*            
UNCONN 0      0       [fe80::9fd5:7dfb:f2dd:56eb]%wlp3s0:546            [::]:*   

5. Используя diagrams.net, создайте L3 диаграмму вашей домашней сети или любой другой сети, с которой вы работали.



