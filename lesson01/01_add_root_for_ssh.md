task: 
Ты случайно обнаружил, что пользователь root подключиться по ssh не может. Надо разобраться, и сделать так, чтобы пользователь root мог подключаться по ssh к машине с Ubuntu.

1:
Определяем причину сбоя авторизации с именем root.
Если нам известно время попытки авторизации:

```ShellSession
~$ journalctl --since "2019-12-13 18:00" --unit=ssh -q
дек 13 18:02:38 cbpi-VirtualBox sshd[2039]: Invalid user \320root from 192.168.41.2 port 60250
дек 13 18:02:40 cbpi-VirtualBox sshd[2039]: pam_unix(sshd:auth): check pass; user unknown
дек 13 18:02:40 cbpi-VirtualBox sshd[2039]: pam_unix(sshd:auth): authentication failure; logname= uid=0 euid=0 tty=ssh ruser= rhost=192.1
дек 13 18:02:42 cbpi-VirtualBox sshd[2039]: Failed password for invalid user \320root from 192.168.41.2 port 60250 ssh2
дек 13 18:02:44 cbpi-VirtualBox sshd[2039]: pam_unix(sshd:auth): check pass; user unknown
дек 13 18:02:47 cbpi-VirtualBox sshd[2039]: Failed password for invalid user \320root from 192.168.41.2 port 60250 ssh2
дек 13 18:02:50 cbpi-VirtualBox sshd[2039]: pam_unix(sshd:auth): check pass; user unknown
дек 13 18:02:52 cbpi-VirtualBox sshd[2039]: Failed password for invalid user \320root from 192.168.41.2 port 60250 ssh2
дек 13 18:02:56 cbpi-VirtualBox sshd[2039]: pam_unix(sshd:auth): check pass; user unknown
дек 13 18:02:58 cbpi-VirtualBox sshd[2039]: Failed password for invalid user \320root from 192.168.41.2 port 60250 ssh2
lines 1-10/10 (END)
```

Если нам известен IP адрес:

```ShellSession
~$ journalctl --unit=ssh -r | grep 192.168.41.2
дек 13 18:02:58 cbpi-VirtualBox sshd[2039]: Failed password for invalid user \320root from 192.168.41.2 port 60250 ssh2
дек 13 18:02:52 cbpi-VirtualBox sshd[2039]: Failed password for invalid user \320root from 192.168.41.2 port 60250 ssh2
дек 13 18:02:47 cbpi-VirtualBox sshd[2039]: Failed password for invalid user \320root from 192.168.41.2 port 60250 ssh2
дек 13 18:02:42 cbpi-VirtualBox sshd[2039]: Failed password for invalid user \320root from 192.168.41.2 port 60250 ssh2
дек 13 18:02:40 cbpi-VirtualBox sshd[2039]: pam_unix(sshd:auth): authentication failure; logname= uid=0 euid=0 tty=ssh ruser= rhost=192.168.41.2
дек 13 18:02:38 cbpi-VirtualBox sshd[2039]: Invalid user \320root from 192.168.41.2 port 60250
```

2:
Меняем конфигурацию sshd для разрешения авторизации как root:

```ShellSession
~$ sudo vim /etc/ssh/sshd_config
```

```vim
#LoginGraceTime 2m
#PermitRootLogin prohibit-password
#StrictModes yes
#MaxAuthTries 6
#MaxSessions 10
PermitRootLogin yes
```

Перезапускаем sshd:

```ShellSession
~$ service sshd restart
```

3:
Пробуем авторизоваться как root:

```console
login as: root
root@192.168.41.251's password:
Welcome to Ubuntu 18.04.3 LTS (GNU/Linux 5.0.0-37-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage


 * Canonical Livepatch is available for installation.
   - Reduce system reboots and improve kernel security. Activate at:
     https://ubuntu.com/livepatch

Могут быть обновлены 0 пакетов.
0 обновлений касаются безопасности системы.

Your Hardware Enablement Stack (HWE) is supported until April 2023.
Last login: Fri Dec 13 18:46:17 2019 from 192.168.41.2
root@cbpi-VirtualBox:~#
```

Проверяем:

```ShellSession
~$ who
cbpi     :0           2019-12-13 16:52 (:0)
root     pts/1        2019-12-13 18:49 (192.168.41.2)
~$ journalctl --since "2019-12-13 18:49" --unit=ssh -q
дек 13 18:49:23 cbpi-VirtualBox sshd[3121]: Accepted password for root from 192.168.41.2 port 63692 ssh2
дек 13 18:49:23 cbpi-VirtualBox sshd[3121]: pam_unix(sshd:session): session opened for user root by (uid=0)
```
