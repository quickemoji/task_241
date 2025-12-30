1. Выведите список интерфейсов, какими способами можно это сделать?
```bash
[lnx@lnx ~]$ ip link
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN mode DEFAULT group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP mode DEFAULT group default qlen 1000
    link/ether 08:00:27:5d:87:f7 brd ff:ff:ff:ff:ff:ff
    altname enx0800275d87f7
```
```bash
[lnx@lnx ~]$ ip addr show  
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host noprefixroute 
       valid_lft forever preferred_lft forever
2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:5d:87:f7 brd ff:ff:ff:ff:ff:ff
    altname enx0800275d87f7
    inet 10.0.2.15/24 brd 10.0.2.255 scope global dynamic noprefixroute enp0s3
       valid_lft 74979sec preferred_lft 74979sec
    inet6 fd17:625c:f037:2:a00:27ff:fe5d:87f7/64 scope global dynamic mngtmpaddr proto kernel_ra 
       valid_lft 86182sec preferred_lft 14182sec
    inet6 fe80::a00:27ff:fe5d:87f7/64 scope link proto kernel_ll 
       valid_lft forever preferred_lft forever
```
```bash
[lnx@lnx ~]$ nmcli device status
DEVICE  TYPE      STATE                 CONNECTION    
enp0s3  ethernet  подключено            System enp0s3 
lo      loopback  подключено (внешнее)  lo 
```
2. Попробуйте изменить ip адрес
```bash
[lnx@lnx ~]$ ip -brief addr
lo               UNKNOWN        127.0.0.1/8 ::1/128 
enp0s3           UP             10.0.2.15/24 fd17:625c:f037:2:a00:27ff:fe5d:87f7/64 fe80::a00:27ff:fe5d:87f7/64 
[lnx@lnx ~]$ sudo ip addr add 10.0.2.20/24 dev enp0s3
[sudo] password for lnx:
[lnx@lnx ~]$ sudo ip addr del 10.0.2.15/24 dev enp0s3
[lnx@lnx ~]$ ip -brief addr
lo               UNKNOWN        127.0.0.1/8 ::1/128 
enp0s3           UP             10.0.2.20/24 fd17:625c:f037:2:a00:27ff:fe5d:87f7/64 fe80::a00:27ff:fe5d:87f7/64 
[lnx@lnx ~]$ 
```
3. Попробуте добавить несколько ip адресов на сетевую карту
```bash
[lnx@lnx ~]$ sudo ip addr add 10.0.2.21/24 dev enp0s3
[lnx@lnx ~]$ sudo ip addr add 10.0.2.22/24 dev enp0s3
[lnx@lnx ~]$ ip -brief addr
lo               UNKNOWN        127.0.0.1/8 ::1/128 
enp0s3           UP             10.0.2.20/24 10.0.2.21/24 10.0.2.22/24 fd17:625c:f037:2:a00:27ff:fe5d:87f7/64 fe80::a00:27ff:fe5d:87f7/64 
```
4. Выведите список маршрутов
```bash
[lnx@lnx ~]$ ip route
10.0.2.0/24 dev enp0s3 proto kernel scope link src 10.0.2.20 
```
5. Выведите arp таблицу
```bash
[lnx@lnx ~]$ ip neigh
10.0.2.2 dev enp0s3 lladdr 52:55:0a:00:02:02 STALE 
fe80::2 dev enp0s3 lladdr 52:56:00:00:00:02 router STALE 
```
6. Что такое ip адрес?
```
IP-адрес (IPv4) — 32-битный логический идентификатор сетевого устройства при использовании протоколов TCP/IP. Состоит из двух частей — некоторое количество бит от начала определяет номер сети, а остальные — номер узла.
```
7. Для чего нужны маршруты?
```
Маршруты нужны, чтобы определятьэто правила, которые говорят системе, через какой интерфейс и шлюз доставлять пакеты к нужному IP-адресу или подсети.
```
8. Что за протокол arp?
```
ARP — протокол в компьютерных сетях, предназначенный для определения MAC-адреса другого компьютера по известному IP-адресу.
```
9.  Что такое dhcp?
```
DHCP — сетевой протокол, позволяющий сетевым устройствам автоматически получать IP-адрес и другие параметры, необходимые для работы в сети TCP/IP. 
```
10. Что такое dns?
```
DNS — компьютерная распределённая система для получения информации о доменах. Чаще всего используется для получения IP-адреса по имени хоста и получения информации о маршрутизации почты.
```
11. Как называется один из протоколов синхронизации времени?
```
NTP — сетевой протокол для синхронизации внутренних часов компьютера с использованием сетей с переменной латентностью.
```
12. Что такое широковещательный запрос, зачем он нужен?
```
Широковещательный запрос — передачи данных в компьютерных сетях, при котором поток данных предназначен для приёма всеми участниками сети. Используется, когда IP получателя неизвестен. 
```
13. Какой адресс является широковещательным?
```
Адрес, который во всех битах узла имеет 1.
```
14. Какие ещё параметры можно задать сетевой карте?
```
Маска подсети
MAC-адрес - аппаратный адрес сетевой карты
Состояние - включеный или выключеный интерфейс
MTU - максимальный размер пакета
Скорость - скорость передачи
```
15. Что такое маска подсети? зачем она нужна?
```
Маска подсети — это 32-битное число, двоичная запись которого содержит непрерывную последовательность единиц в тех разрядах, которые должны в IP-адресе интерпретироваться как номер сети. Граница между последовательностями единиц и нулей в маске соответствует границе между номером сети и номером узла в IPадресе. Она нужна для разделения сети и узла.
```