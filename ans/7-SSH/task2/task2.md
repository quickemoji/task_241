1. Где хранятся пользвательские и системные настройки подключения?
```
Если речь идёт в контексте SSH, то пользовательские хранятся в домашней папке пользователя по пути ~/.ssh/config. Системные в /etc/ssh/sshd_config.
```
2. Что за файл options?
```
Если необходимо подключаться к множеству различных удалённых систем по SSH, можно для таких систем создать алиасы SSH. Это позволит не запоминать различные параметры доступа и не вводить многократно одни и те же параметры при каждом использовании SSH.

Пример файла конфигурации ~/.ssh/config:
```
3. Отредактируйте файл options так, чтобы можно было подключаться не вводя имя пользвателя и порт
```bash
[lnx@lnx system-connections]$ nano ~/.ssh/config
[lnx@lnx system-connections]$ cat ~/.ssh/config
# OpenSSH client configuration file format is described in ssh_config(5) manual page.
Host test
    HostName ternar.io
    User student
    Port 211
```
4. Назовите подключение удобным для вас спсобом
```
test
```
5. Проверьте работоспособность
```bash
[lnx@lnx system-connections]$ ssh test
student@ternar.io's password: 
Last login: Tue Dec 30 06:34:24 2025 from 85.140.4.96
[student@S-vm-211 ~]$ 
```