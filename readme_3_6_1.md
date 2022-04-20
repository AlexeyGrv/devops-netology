#### 1. Работа c HTTP через телнет.
```sh
  ~$ telnet stackoverflow.com 80
  Trying 151.101.193.69...
  Connected to stackoverflow.com.
  Escape character is '^]'.
  GET /questions HTTP/1.0
  HOST: stackoverflow.com
```
##### В ответе укажите полученный HTTP код, что он означает?
```sh
HTTP/1.1 301 Moved Permanently
location: https://stackoverflow.com/questions
```
> Код перенаправления "301 Moved Permanently" показывает, что ресурс 'questions' хоста 'stackoverflow.com' был окончательно перемещён в URL, указанный в заголовке location: https://stackoverflow.com/questions

#### 2. Повторите задание 1 в браузере, используя консоль разработчика F12.
- откройте вкладку Network
- отправьте запрос http://stackoverflow.com
- найдите первый ответ HTTP сервера, откройте вкладку Headers
- укажите в ответе полученный HTTP код.
- проверьте время загрузки страницы, какой запрос обрабатывался дольше всего?
- приложите скриншот консоли браузера в ответ.

#### 3. Какой IP адрес у вас в интернете?
https://whoer.net/ru
89.109.26.160 
```sh
~$  wget -qO- eth0.me
89.109.26.160
```
#### 4. Какому провайдеру принадлежит ваш IP адрес? Какой автономной системе AS? Воспользуйтесь утилитой whois
```sh
~$ whois -h whois.ripe.net 89.109.26.160 | grep descr
descr:          Network for OJSC VolgaTelecom
```
```sh
~$ whois -h whois.radb.net 89.109.26.160
route:          89.109.24.0/21
descr:          NMTS Autonomous System
origin:         AS25405
mnt-by:         NMTS-MNT
created:        2009-02-06T07:55:37Z
last-modified:  2009-02-06T07:55:37Z
source:         RIPE
```
#### 5. Через какие сети проходит пакет, отправленный с вашего компьютера на адрес 8.8.8.8? Через какие AS? Воспользуйтесь утилитой traceroute
```sh
~$ traceroute -An 8.8.8.8
traceroute to 8.8.8.8 (8.8.8.8), 30 hops max, 60 byte packets
 1  172.17.0.1 [*]  0.307 ms  0.499 ms  0.484 ms
 2  192.168.0.1 [*]  2.714 ms  2.737 ms  2.727 ms
 3  81.177.115.149 [AS12389]  8.131 ms  8.440 ms  7.586 ms
 4  81.177.115.148 [AS12389]  7.976 ms  7.964 ms  7.953 ms
 5  * * *
 6  * * *
 7  108.170.250.130 [AS15169]  20.094 ms 108.170.250.146 [AS15169]  19.663 ms 108.170.250.130 [AS15169]  22.600 ms
 8  142.251.238.84 [AS15169]  35.712 ms 209.85.255.136 [AS15169]  36.031 ms 142.251.49.158 [AS15169]  34.723 ms
 9  216.239.48.224 [AS15169]  34.779 ms  33.088 ms  34.755 ms
10  216.239.42.21 [AS15169]  33.300 ms 172.253.51.243 [AS15169]  88.889 ms 142.250.56.125 [AS15169]  36.462 ms
11  * * *
12  * * *
13  * * *
14  * * *
15  * * *
16  * * *
17  * * *
18  * * *
19  * * 8.8.8.8 [AS15169]  39.344 ms
```
#### 6. Повторите задание 5 в утилите mtr. На каком участке наибольшая задержка - delay?

#### 7. Какие DNS сервера отвечают за доменное имя dns.google? Какие A записи? воспользуйтесь утилитой dig
```sh
dig NS dns.google +noall +answer
dns.google.		2885	IN	NS	ns1.zdns.google.
dns.google.		2885	IN	NS	ns3.zdns.google.
dns.google.		2885	IN	NS	ns4.zdns.google.
dns.google.		2885	IN	NS	ns2.zdns.google.
```
```sh
dig dns.google +noall +answer
dns.google.		20	IN	A	8.8.4.4
dns.google.		20	IN	A	8.8.8.8
```
#### 8. Проверьте PTR записи для IP адресов из задания 7. Какое доменное имя привязано к IP? воспользуйтесь утилитой dig
```sh
dig -x 8.8.8.8 +noall +answer
8.8.8.8.in-addr.arpa.	1732	IN	PTR	dns.google.
dig -x 8.8.4.4 +noall +answer
4.4.8.8.in-addr.arpa.	3059	IN	PTR	dns.google.
``
