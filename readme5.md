#### 1. Подключитесь к публичному маршрутизатору в интернет. Найдите маршрут к вашему публичному IP

```sh
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
```
```sh
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
  ....
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
```    
#### 2. Создайте dummy0 интерфейс в Ubuntu. Добавьте несколько статических маршрутов. Проверьте таблицу маршрутизации.
```sh
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
169.254.0.0/16 dev wlp3s0 scope link metric 1000 
192.168.0.0/24 via 10.1.1.1 dev dummy0 
192.168.0.0/24 dev enp4s0f1 proto kernel scope link src 192.168.0.163 metric 100 
192.168.0.0/24 dev wlp3s0 proto kernel scope link src 192.168.0.185 metric 600 
```
#### 3. Проверьте открытые TCP порты в Ubuntu, какие протоколы и приложения используют эти порты? Приведите несколько примеров.
```sh
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
```
#### 4. Проверьте используемые UDP сокеты в Ubuntu, какие протоколы и приложения используют эти порты?
```sh
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
```
#### 5. Используя diagrams.net, создайте L3 диаграмму вашей домашней сети или любой другой сети, с которой вы работали.

![](https://github.com/AlexeyGrv/devops-netology/blob/main/images/network_diagram.png)

