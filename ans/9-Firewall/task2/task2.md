1. Удалите iptables и установите firewalld
```bash
[student@S-vm-211 ~]$ sudo systemctl stop iptablestest.service
[student@S-vm-211 ~]$ sudo apt-get update
Получено: 1 http://ftp.altlinux.org Sisyphus/x86_64 release [4225B]
...
Чтение списков пакетов... Завершено
Построение дерева зависимостей... Завершено
[student@S-vm-211 ~]$ sudo apt-get remove iptables
Чтение списков пакетов... Завершено
Построение дерева зависимостей... Завершено
...
1: iptables-1.8.10-alt1                 #################################################################### [100%]
Завершено.
[student@S-vm-211 ~]$ sudo apt-get install firewalld
Чтение списков пакетов... Завершено
Построение дерева зависимостей... Завершено
Следующие дополнительные пакеты будут установлены:
  ebtables       libexpat      libnspr     python3-base                      #################################################################### [ 98%]
...
49: libcap-ng-0.8.5-alt1                #################################################################### [100%]
Завершено.
[student@S-vm-211 ~]$ sudo systemctl start firewalld
[student@S-vm-211 ~]$ sudo systemctl enable firewalld
Synchronizing state of firewalld.service with SysV service script with /usr/lib/systemd/systemd-sysv-install.
Executing: /usr/lib/systemd/systemd-sysv-install enable firewalld
[student@S-vm-211 ~]$ sudo systemctl status firewalld
● firewalld.service - firewalld - dynamic firewall daemon
     Loaded: loaded (/usr/lib/systemd/system/firewalld.service; enabled; preset: enabled)
     Active: active (running) since Tue 2025-12-30 15:05:48 UTC; 32s ago
 Invocation: bb21283f17424fdd9e719b4392eb1cd9
       Docs: man:firewalld(1)
   Main PID: 1939 (firewalld)
      Tasks: 2 (limit: 2332)
     Memory: 29.1M (peak: 31.4M)
        CPU: 728ms
     CGroup: /system.slice/firewalld.service
             └─1939 /usr/bin/python3 /usr/sbin/firewalld --nofork --nopid

дек 30 15:05:47 S-vm-211 systemd[1]: Starting firewalld.service - firewalld - dynamic firewall daemon...
дек 30 15:05:48 S-vm-211 systemd[1]: Started firewalld.service - firewalld - dynamic firewall daemon.
```
2. Попробуйте так-же проверить возможность подключения по ssh
```bash
[lnx@lnx ~]$ ssh test
Last login: Tue Dec 30 15:06:54 2025 from 85.140.6.225
[student@S-vm-211 ~]$ 
```
```
Всё работает
```
3. Если её нет то откройте порт
4. Выведите список открытых портов с помощью firewall-cmd
```
[student@S-vm-211 ~]$ sudo firewall-cmd --add-port=211/tcp
success
[student@S-vm-211 ~]$ sudo firewall-cmd --add-port=211/udp
success
[student@S-vm-211 ~]$ sudo firewall-cmd --list-ports
211/tcp 211/udp
```
5. Можно ли там добавить порты по названию сервиса?
```
Да, sudo firewall-cmd --add-service=[имя сервиса]
```
6. На вашей Локальной виртуальной машине попробуйте подключиться к серверу samba из предыдущих заданий
```
[lnx@lnx ~]$ smbclient //localhost/test -N
Try "help" to get a list of possible commands.
smb: \> 
```
7. Если не получилось то откройте нужные порты
8. Сделайте так чтобы изменения были постоянными
```
[student@S-vm-211 ~]$ sudo firewall-cmd --permanent --add-port=211/tcp
Warning: ALREADY_ENABLED: 211:tcp
success
[student@S-vm-211 ~]$ sudo firewall-cmd --permanent --add-port=211/udp
success
```