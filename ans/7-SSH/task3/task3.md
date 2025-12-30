1. Что такое ssh ключи и зачем они нужны?
```
SSH-ключи - это пара криптографических файлов, которые могут служить средством идентификации на SSH-сервере. Их использованиет позволяет входить на сервер без пароля, а так же сделать подключение более безопасным.
```
2. Как их создать?
```bash
С помощью команды
ssh-keygen
-t [тип ключа] - создание ключа определённого типа
-C [Коментарий] - метка ключа
```
3. Создайт пару публичный/приватный ключ ed_25519, где они хранятся?
```bash
[lnx@lnx system-connections]$ ssh-keygen -t ed25519 -C "test"
Generating public/private ed25519 key pair.
Enter file in which to save the key (/home/lnx/.ssh/id_ed25519): 
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in /home/lnx/.ssh/id_ed25519
Your public key has been saved in /home/lnx/.ssh/id_ed25519.pub
...
```
```
Публичный в ~/.ssh/id_ed25519.pub
Приватный в ~/.ssh/id_ed25519

```
4. Скопируйте публичный ключ на ваш сервер, в каком файле он будет храниться?
```bash
[lnx@lnx ~]$ ssh-copy-id test
/usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
/usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
student@ternar.io's password: 

Number of key(s) added: 1

Now try logging into the machine, with:   "ssh 'test'"
and check to make sure that only the key(s) you wanted were added.
```
```
Будет хранится в ~/.ssh/authorized_keys
```
5. Попробуйте подключиться к серверу, у вас запросили пароль?
```bash
[lnx@lnx ~]$ ssh test
Last login: Tue Dec 30 08:18:19 2025 from 85.140.4.96
```
```
Нет
```
6. Запретите подключение с паролем для всех пользователей, оставьте только с помощью ключа.
```
Раскоментировал PubkeyAuthentication yes. Раскоментировал PasswordAuthentication и заменил yes на no. Добавил ChallengeResponseAuthentication no.
```
```bash
[student@S-vm-211 ~]$ sudo cat /etc/openssh/sshd_config
#	$OpenBSD: sshd_config,v 1.104 2021/07/02 05:11:21 dtucker Exp $

# This is the sshd server system-wide configuration file.  See
# sshd_config(5) for more information.

# This sshd was compiled with PATH=/bin:/usr/bin:/usr/local/bin

# The strategy used for options in the default sshd_config shipped with
# OpenSSH is to specify options with their default value where
# possible, but leave them commented.  Uncommented options override the
# default value.

#Port 22
#AddressFamily any
#ListenAddress 0.0.0.0
#ListenAddress ::

#HostKey /etc/openssh/ssh_host_rsa_key
#HostKey /etc/openssh/ssh_host_ecdsa_key
#HostKey /etc/openssh/ssh_host_ed25519_key

# Ciphers and keying
#RekeyLimit default none

# Logging
#SyslogFacility AUTHPRIV
#LogLevel INFO

# Authentication:

#LoginGraceTime 2m
PermitRootLogin without-password
#StrictModes yes
MaxAuthTries 10 
#MaxSessions 10

PubkeyAuthentication yes
#PubkeyAcceptedAlgorithms ssh-ed25519-cert-v01@openssh.com,sk-ssh-ed25519-cert-v01@openssh.com,ssh-ed25519,sk-ssh-ed25519@openssh.com,rsa-sha2-512-cert-v01@openssh.com,rsa-sha2-512,rsa-sha2-256-cert-v01@openssh.com,rsa-sha2-256,ecdsa-sha2-nistp521-cert-v01@openssh.com,ecdsa-sha2-nistp521,ecdsa-sha2-nistp384-cert-v01@openssh.com,ecdsa-sha2-nistp384,ecdsa-sha2-nistp256-cert-v01@openssh.com,sk-ecdsa-sha2-nistp256-cert-v01@openssh.com,ecdsa-sha2-nistp256,sk-ecdsa-sha2-nistp256@openssh.com

#AuthorizedKeysFile	/etc/openssh/authorized_keys/%u /etc/openssh/authorized_keys2/%u .ssh/authorized_keys .ssh/authorized_keys2

#AuthorizedPrincipalsFile none

#AuthorizedKeysCommand none
#AuthorizedKeysCommandUser nobody

# For this to work you will also need host keys in /etc/openssh/ssh_known_hosts
#HostbasedAuthentication no
# Change to yes if you don't trust ~/.ssh/known_hosts for
# HostbasedAuthentication
#IgnoreUserKnownHosts no
# Don't read the user's ~/.rhosts and ~/.shosts files
#IgnoreRhosts yes

# To disable tunneled clear text passwords, change to no here!
PasswordAuthentication no 
#PermitEmptyPasswords no

# Change to yes to enable s/key passwords
#KbdInteractiveAuthentication no

# Kerberos options
#KerberosAuthentication no
#KerberosOrLocalPasswd yes
#KerberosTicketCleanup yes
#KerberosGetAFSToken no

# GSSAPI options
#GSSAPIAuthentication no
#GSSAPICleanupCredentials yes

# Set this to 'yes' to enable PAM authentication, account processing,
# and session processing. If this is enabled, PAM authentication will
# be allowed through the KbdInteractiveAuthentication and
# PasswordAuthentication.  Depending on your PAM configuration,
# PAM authentication via KbdInteractiveAuthentication may bypass
# the setting of "PermitRootLogin prohibit-password".
# If you just want the PAM account and session checks to run without
# PAM authentication, then enable this but set PasswordAuthentication
# and KbdInteractiveAuthentication to 'no'.
#UsePAM yes

#AllowAgentForwarding yes
#AllowTcpForwarding yes
#GatewayPorts no
#X11Forwarding yes
#X11DisplayOffset 10
#X11UseLocalhost yes
#PermitTTY yes
#PrintMotd yes
#PrintLastLog yes
#TCPKeepAlive yes
#PermitUserEnvironment no
#Compression delayed
#ClientAliveInterval 0
#ClientAliveCountMax 3
#UseDNS no
#PidFile /var/run/sshd.pid
#MaxStartups 10:30:100
#PermitTunnel no
#ChrootDirectory none
#VersionAddendum none

# no default banner path
#Banner none

# override default of no subsystems
Subsystem sftp	/usr/lib/openssh/sftp-server

#AllowGroups wheel users

# Accept locale environment variables
AcceptEnv LANG LANGUAGE LC_ADDRESS LC_ALL LC_COLLATE LC_CTYPE
AcceptEnv LC_IDENTIFICATION LC_MEASUREMENT LC_MESSAGES LC_MONETARY
AcceptEnv LC_NAME LC_NUMERIC LC_PAPER LC_TELEPHONE LC_TIME

# Example of overriding settings on a per-user basis
#Match User anoncvs
#	X11Forwarding yes
#	AllowTcpForwarding no
#	PermitTTY no
#	ForceCommand cvs server
#Match Group secured
#	AuthorizedKeysFile /etc/openssh/authorized_keys/%u
#Match Group !secured,*
#	AuthorizedKeysFile /etc/openssh/authorized_keys/%u .ssh/authorized_keys

DenyUsers ssh-user
ChallengeResponseAuthentication no
```