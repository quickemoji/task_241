1. Установите пакет samba
```bash
sudo apt-get install samba
```
2. ЧТо такое побщая папка, зачем оно может быть нужно?
```
в Samba общая папка - папка в файловой системе, доступ к которой предоставляется по сети другим компьютерам с помощью протокола SMB/CIFS.
Она нужна для обмена файлами, совместной работы, удалённого хранения файлов.
```
3. Создайте общую папку без пароля с правами только на чтение файлов
```bash
[lnx@lnx ~]$ sudo mkdir /smtest
[lnx@lnx ~]$ sudo chmod 755 /smtest
[lnx@lnx ~]$ sudo nano /etc/samba/smb.conf
[lnx@lnx ~]$ tail -5 /etc/samba/smb.conf
[test]
	path = /smtest
	browseable = yes
	guest ok = yes
	read only = yes
[lnx@lnx ~]$ sudo systemctl restart smb nmb
```
4. Создайте общую папку с паролем с правами на чтение и запись
```bash
[lnx@lnx ~]$ sudo mkdir /smtest1
[lnx@lnx ~]$ sudo chmod 770 /smtest1
[lnx@lnx ~]$ sudo nano /etc/samba/smb.conf
[lnx@lnx ~]$ sudo systemctl restart smb nmb
[lnx@lnx ~]$ tail -5 /etc/samba/smb.conf
[test1]
	path = /smtest1
	browserble = yes
	writable = yes
	valid user = lnx
```
5. Создайте общую папку с доступом для какой-то группы с полными правами
```bash
[lnx@lnx ~]$ sudo groupadd smtest
[lnx@lnx ~]$ sudo usermod -aG smtest lnx
[lnx@lnx ~]$ sudo mkdir /smtest2
[lnx@lnx ~]$ sudo chmod 770 /smtest2
[lnx@lnx ~]$ sudo nano /etc/samba/smb.conf
[lnx@lnx ~]$ tail -6 /etc/samba/smb.conf
[test2]
	path = /smtest2
	browseble = yes
	writable = yes
	valid user = @smtest
	force group = smtest
[lnx@lnx ~]$ sudo systemctl restart smb nmb
```
6. Создайте общую папку в которой у одной группы будет полный доступ, а у другой только доступ на чтение. Третья группа не должна иметь к ней доступа
```bash
[lnx@lnx ~]$ sudo groupadd smtest1
[lnx@lnx ~]$ sudo groupadd smtest2
[lnx@lnx ~]$ sudo mkdir /smtest3
[lnx@lnx ~]$ sudo chmod 770 /smtest3
[lnx@lnx ~]$ sudo nano /etc/samba/smb.conf
[lnx@lnx ~]$ tail -9 /etc/samba/smb.conf
[test3]
	path = /smtest3
	browseble = yes
	writeble = yes
	valid user = @smtest @smtest1
	write list = @smtest
	read list = @smtest1
	fource group = smtest

[lnx@lnx ~]$ sudo systemctl restart smb nmb
```