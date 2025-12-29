1. Посмотретите журналы ssh
```bash
[root@lnx ~]# journalctl -u sshd
дек 18 13:33:58 lnx systemd[1]: Starting sshd.service - OpenSSH server daemon...
дек 18 13:33:58 lnx systemd[1]: Started sshd.service - OpenSSH server daemon.
дек 18 13:33:58 lnx sshd[1001]: Server listening on 0.0.0.0 port 22.
дек 18 13:33:58 lnx sshd[1001]: Server listening on :: port 22.
-- Boot 7a2b33f883bf4eeca9b7d86bbc6ec744 --
дек 20 19:09:10 lnx systemd[1]: Starting sshd.service - OpenSSH server daemon...
дек 20 19:09:10 lnx systemd[1]: Started sshd.service - OpenSSH server daemon.
дек 20 19:09:10 lnx sshd[1001]: Server listening on 0.0.0.0 port 22.
дек 20 19:09:10 lnx sshd[1001]: Server listening on :: port 22.
-- Boot ca70e26345d84bec82bae1f938823957 --
дек 21 00:33:31 lnx systemd[1]: Starting sshd.service - OpenSSH server daemon...
дек 21 00:33:31 lnx systemd[1]: Started sshd.service - OpenSSH server daemon.
...
```
2. Выведите журналы в реальном времени
```bash
[root@lnx ~]# journalctl -f
дек 29 18:27:35 lnx PackageKit[4559]: get-updates transaction /1178_aadaedbc from uid 1000 finished with success after 4412ms
дек 29 18:30:47 lnx systemd[5096]: Starting systemd-tmpfiles-clean.service - Cleanup of User's Temporary Files and Directories...
дек 29 18:30:47 lnx systemd[5096]: Finished systemd-tmpfiles-clean.service - Cleanup of User's Temporary Files and Directories.
дек 29 18:31:20 lnx systemd[1]: Starting scr.service - Мой юнит...
дек 29 18:31:20 lnx scr[6353]: Файл 'info/1' есть
дек 29 18:31:20 lnx scr[6353]: Файл 'info/2' есть
дек 29 18:31:20 lnx scr[6353]: Файл 'info/3' есть
дек 29 18:31:20 lnx scr[6353]: Файл 'info/4' есть
дек 29 18:31:20 lnx systemd[1]: scr.service: Deactivated successfully.
дек 29 18:31:20 lnx systemd[1]: Finished scr.service - Мой юнит.
```
3. Выведите лог в реальном времени для службы sshd
```bash
[root@lnx ~]# journalctl -u sshd -f
дек 29 14:48:58 lnx systemd[1]: Starting sshd.service - OpenSSH server daemon...
дек 29 14:48:59 lnx systemd[1]: Started sshd.service - OpenSSH server daemon.
дек 29 14:48:59 lnx sshd[1092]: Server listening on 0.0.0.0 port 22.
дек 29 14:48:59 lnx sshd[1092]: Server listening on :: port 22.
```
4. Можно ли без комады journalctl прочитать логи systemd?
'''
Да в /var/log/journal, но они имеют бинарный формат, поэтому при просмотре через cat вывод будет нечитаемым для человека.
'''
5. Сколько будет 2-2?
```
[lnx@lnx ~]$ python3
Python 3.12.7 (main, Oct  2 2024, 04:23:59) [GCC 13.2.1 20240128 (ALT Sisyphus 13.2.1-alt3)] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> 2 - 2
0
```