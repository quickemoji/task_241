1. Создайте скрипт который создаёт папку заполняет её файлами ( имена 1-4 ) и записывает в них информацию о текущей дате, версии ядра, имени компьютера и списе всех файлов в домашнем каталоге пользователя от которого выполняется скрипт( не забудьте сдлеать проверку на существование файлов и папок)
```bash
#!/usr/bin/env bash
dir="${1}"

if [ ! -d "$dir" ]; then
  mkdir "$dir"
fi

for n in 1 2 3 4; do
  file="$dir/$n"

  if [ -e "$file" ]; then
    echo "Файл '$file' есть"
    continue
  fi

  echo "Дата $(date)" > "$file"
  echo "Версия ядра: $(uname -r)" >> "$file"
  echo "Имя компьютера: $(hostname)" >> "$file"
  echo "Список файлов в домашнем каталоге:" >> "$file"
  ls -a "$HOME" >> "$file"

  echo "Был создан: $file"
done
```
2. Создайте юнит который будет вызывать этот скрипт при запуске. Проверьте
```bash
[lnx@lnx ~]$ sudo nano /etc/systemd/system/scr.service
[sudo] password for lnx:
[lnx@lnx ~]$ sudo cat /etc/systemd/system/scr.service
[Unit]
Description=Мой юнит
After=network.target

[Service]
Type=oneshot
ExecStart=/usr/local/bin/scr info

[Install]
WantedBy=multi-user.target
```
```bash
[root@lnx /]# ls -l info
итого 0
-rw-r--r-- 1 root root 0 дек 29 13:45 1
-rw-r--r-- 1 root root 0 дек 29 13:45 2
-rw-r--r-- 1 root root 0 дек 29 13:45 3
-rw-r--r-- 1 root root 0 дек 29 13:45 4
```
3. Создайте таймер который будет вызывать выполнение одноимённого systemd юнита каждые 5 минут.
```bash
[root@lnx /]# sudo nano /etc/systemd/system/scr.timer
[root@lnx /]# cat /etc/systemd/system/scr.timer
[Unit]
Description=Мой таймер

[Timer]
OnBootSec=5min
OnUnitActiveSec=5min

[Install]
WantedBy=timers.target
```
4. От какого пользователя вызыаются юниты поумолчанию?
```
От root
```
5. Создайте пользователя от имени которого будет выполняться ваш скрипт.
```
[root@lnx ~]# useradd -m -s /bin/bash test
```
6. Дополните юнит информацией о пользователе от которого должен выплняться скрипт.
```
[root@lnx ~]# nano /etc/systemd/system/scr.service
[root@lnx ~]# cat /etc/systemd/system/scr.service
[Unit]
Description=Мой юнит
After=network.target

[Service]
Type=oneshot
ExecStart=/usr/local/bin/scr info
User=test

[Install]
WantedBy=multi-user.target
```
7. Дополните ваш скрипт так, что бы он независимо от местоположения всега выполнялся в домашней папке того кто его вызывает.
```bash
#!/usr/bin/env bash
cd ~
dir="${1}"

if [ ! -d "$dir" ]; then
  mkdir "$dir"
fi

for n in 1 2 3 4; do
  file="$dir/$n"

  if [ -e "$file" ]; then
    echo "Файл '$file' есть"
    continue
  fi

  echo "Дата $(date)" > "$file"
  echo "Версия ядра: $(uname -r)" >> "$file"
  echo "Имя компьютера: $(hostname)" >> "$file"
  echo "Список файлов в домашнем каталоге:" >> "$file"
  ls -a "$HOME" >> "$file"

  echo "Был создан: $file"
done
```