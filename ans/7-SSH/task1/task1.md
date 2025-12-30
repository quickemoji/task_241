1. Какой по умолчанию используется порт для поключения?
```
Для SSH по умолчанию используется порт 22 для поключения. 
```
2. Можно ли его изменить? если да то как?
```
Да. В файле /etc/openssh/sshd_config или другом в зависимости от дистрибутива найти "#Port 22", далее раскоментировать и заменить на нужное число.
```
3. Какая служба отвечает за обработку запросов на подключения по ssh?
```
sshd в некоторых дистрибутивах ssh.
```
4. Какой файл конфигурации отвечает за его настройку?
```
/etc/openssh/sshd_config или другом в зависимости от дистрибутива
```
5. Попробуйте подключиться по ssh к предоставленному вам серверу
```bash
[lnx@lnx ~]$ ssh student@ternar.io -p 211
The authenticity of host '[ternar.io]:211 ([95.31.204.147]:211)' can't be established.
ED25519 key fingerprint is SHA256:l2Cbfl6+dbgWJwySKiooQuKtqOXsdkrS1VB8WDf6W10.
This key is not known by any other names.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '[ternar.io]:211' (ED25519) to the list of known hosts.
student@ternar.io's password: 
Last login: Tue Nov 25 10:44:29 2025
[student@S-vm-211 ~]$ 
```
6. тредактируйте файл настроек на сервере так, чтобы была возможность подключиться к серверу используя пользователя root
```bash
раскоментировал PermitRootLogin without-password в /etc/openssh/sshd_config. Перезапустил службу sshd.
...
# Authentication:

#LoginGraceTime 2m
PermitRootLogin without-password
#StrictModes yes
#MaxAuthTries 6
#MaxSessions 10
...
```
7. Измените колличество ошибок ввода пароля перед сборосом соединения, покажите эти измененения
```bash
раскоментировал MaxAuthTries в /etc/openssh/sshd_config и изменил с 6 до 1. Перезапустил службу sshd.
# Authentication:

#LoginGraceTime 2m
PermitRootLogin without-password
#StrictModes yes
MaxAuthTries 1 
#MaxSessions 10

[lnx@lnx ~]$ ssh student@ternar.io -p 211
student@ternar.io's password: 
ssh: Received disconnect from 95.31.204.147 port 211:2: Too many authentication failures
Disconnected from 95.31.204.147 port 211
```
8. Создайте пользователя ssh-user и попробуйте им подключиться к серверу
```bash
[lnx@lnx ~]$ ssh student@ternar.io -p 211
student@ternar.io's password: 
Last login: Tue Dec 30 06:12:32 2025 from 85.140.4.96
[student@S-vm-211 ~]$ sudo useradd -m -s /bin/bash ssh-user
[student@S-vm-211 ~]$ sudo passwd ssh-user

[lnx@lnx ~]$ ssh ssh-user@ternar.io -p 211
ssh-user@ternar.io's password: 
[ssh-user@S-vm-211 ~]$ 
```
9.  Ограничте ему возможность подключения к серверу
```
[lnx@lnx ~]$ ssh ssh-user@ternar.io -p 211
ssh-user@ternar.io's password: 
ssh: Received disconnect from 95.31.204.147 port 211:2: Too many authentication failures
Disconnected from 95.31.204.147 port 211
```
10. Как вы это сделали?
```
 в /etc/openssh/sshd_config дописал DenyUsers ssh-user. Перезапустил службу sshd.
```
11.  Что хранится в файле known_hosts?
```
В файле known_hosts хранятся публичные ключи серверов, к которым пользователь уже подключался через SSH, вместе с их именами или IP. Он используется для проверки подлинности сервера при повторном подключении.
```
```bash
[lnx@lnx ~]$ cat ~/.ssh/known_hosts
[ternar.io]:211 ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIGjZ7zxcpyEQC3Atm8h2cVv1bvj+NrUZJZpYUc9JJJpp
[ternar.io]:211 ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQCX9qpedjB94LXrz3AGtGaqStUyvz0AaS//Cr3/khbU1VobxW/KNPCA4Y2ZDdHLeFX3++Ey2tE2z3ee1yVGJhdtUzMUcyGvqlhzNs3WrtWuV1A1ewCqABlWNcCDRx7tMQSC7KMcLqDsc4/MupyF1u20/VCbrveS7/Pz9a0SVOfNdvyZ6Ny6lNLWBI2OhfA/uu9ZxSr6EK2KMUUA7++St687FTP1nVB5IrcTwAP9Px8Qh6YwKbxxtWxHURlUFs9JTwwiWRaWSC3ODfD4A4lwA3OMqGk6cWpHk83SDrI6P+Rs85+O4ch0ro5MziBdYKakNtOfK7pWJGqarxL8NjKsc1lBTzaveQKV9NdecL79gVL1ChJAZQUr61WLydXxZQpl8gAdwliN6PR0AufN6E468QqwD+uW3PgZxeR1PQPGwbA7nU/1mDofXyOeQ6eIwkEv8Hl7eml84jNM24ihO2JAYRdc+iW3WVvsm3NmmasGg4avlYU2E+w0V+U9+qqC1DEYb80=
[ternar.io]:211 ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBLf3m15A8rSp3Hcz3VlLXFGH/1nQnAjmneZiUdCEVEb9YEidhrvSUfyu77FqSGillaFWwZ6MXiSIBIX0SIC54TA=
```