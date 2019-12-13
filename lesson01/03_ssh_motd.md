task:
Все в Ubuntu хорошо, да вот столько фигни всякой показывает она при подключении по ssh! Нужно бы разобраться, как сделать так, чтобы после подключения по ssh показывались ТОЛЬКО сообщения о дистрибутиве/ядре, состоянии пакетов и Last login. Например:

```Console
Welcome to Ubuntu 18.04.3 LTS (GNU/Linux 5.0.0-23-generic x86_64)

240 packages can be updated.
142 updates are security updates.

Last login: Fri Dec 13 04:40:07 2019 from 127.0.0.1
```

- А теперь сделай так, чтобы для всех пользователей эти сообщения показывались, кроме пользователя **root**.

1:
Проверяем какие скрипты выполняются для вывода **motd**:

```ShellSession
~ $ ls -la /etc/update-motd.d/
итого 64
drwxr-xr-x   2 root root  4096 дек 13 20:45 .
drwxr-xr-x 125 root root 12288 дек 12 21:57 ..
-rwxr-xr-x   1 root root  1220 апр  9  2018 00-header
-rwxr-xr-x   1 root root  1157 апр  9  2018 10-help-text
-rwxr-xr-x   1 root root  4646 сен 27 21:24 50-motd-news
-rwxr-xr-x   1 root root   604 мар 21  2018 80-esm
-rwxr-xr-x   1 root root  3017 мар 21  2018 80-livepatch
-rwxr-xr-x   1 root root    97 ноя 12  2018 90-updates-available
-rwxr-xr-x   1 root root   299 июн  3  2019 91-release-upgrade
-rwxr-xr-x   1 root root   165 ноя 25 18:23 92-unattended-upgrades
-rwxr-xr-x   1 root root   129 ноя 12  2018 95-hwe-eol
-rwxr-xr-x   1 root root   142 ноя 12  2018 98-fsck-at-reboot
-rwxr-xr-x   1 root root   144 ноя 12  2018 98-reboot-required
```

2:
Отключаем ненужные скрипты в **/etc/update-motd.d/**:

```ShellSession
~ $ cd /etc/update-motd.d/
cbpi@cbpi-VirtualBox:/etc/update-motd.d $ sudo chmod -x 10-help-text 
cbpi@cbpi-VirtualBox:/etc/update-motd.d $ sudo chmod -x 50-motd-news 
cbpi@cbpi-VirtualBox:/etc/update-motd.d $ sudo chmod -x 80-esm 
cbpi@cbpi-VirtualBox:/etc/update-motd.d $ sudo chmod -x 80-livepatch 
cbpi@cbpi-VirtualBox:/etc/update-motd.d $ sudo chmod -x 95-hwe-eol 
```

3:
Проверяем:

```Console
login as: root
root@192.168.41.251's password:
Welcome to Ubuntu 18.04.3 LTS (GNU/Linux 5.0.0-37-generic x86_64)

Могут быть обновлены 0 пакетов.
0 обновлений касаются безопасности системы.

Last login: Fri Dec 13 18:49:23 2019 from 192.168.41.2
root@cbpi-VirtualBox:~#
```

4:
Отключаем **motd** для пользователя **root**:

Создаем пустой файл **.hushlogin** для пользователя **root** и перезапускаем **sshd**:

```ShellSession
~ $ su
Пароль: 
root@cbpi-VirtualBox:/etc/profile.d# cd ~
root@cbpi-VirtualBox:~# touch .hushlogin
root@cbpi-VirtualBox:~# service sshd restart
root@cbpi-VirtualBox:~# exit
exit
```

Проверяем:

```Console
login as: root
root@192.168.41.251's password:
root@cbpi-VirtualBox:~#
```

```Console
login as: cbpi
********************************************************************
*                        <<< WARNING >>>                           *
*==================================================================*
* This ubuntu machine is solely for the use of authorized users.   *
* Hopefully, you know what you're up to. Just remember:            *
*                                                                  *
*      Do not allow children to play in the dishwasher!!!          *
*                                                                  *
* Oh, sorry... I meant be aware:                                   *
*                                                                  *
*        You will not see symbols while typing your password.      *
*  That's ok - they are still typed. We care about your security.  *
********************************************************************
cbpi@192.168.41.251's password:
Welcome to Ubuntu 18.04.3 LTS (GNU/Linux 5.0.0-37-generic x86_64)

Могут быть обновлены 0 пакетов.
0 обновлений касаются безопасности системы.

Last login: Fri Dec 13 22:05:13 2019 from 192.168.41.2
cbpi@cbpi-VirtualBox:~ $
```

