**task:**
Очень часто бывают ситуации, когда ты не помнишь какую-то информацию о пакете, который тебе нужно установить. При этом доступа в интернет нет. Круто, что достаточно помнить пару команд, чтобы найти информацию о любом пакете. В этом я и предлагаю разобраться.

- представим, что тебе нужно установить два пакета, а ты не помнишь их названия. Но зато точно помнишь что в одном из этих пакетов был файл с названием **landscape-sysinfo**, а в другом пакете - файл с названием **ansiweather**. Нужно разобраться, как узнать имя пакета, зная имя файла, который в нем находится.

- после этого ты вспоминаешь, что тебе нельзя не при каком условии устанавливать пакеты, в которых есть файл **/usr/bin/virus**. Ты решаешь убедиться что в этих пакетах таких файлов нет. Нужно разобраться, как показать файлы, которые содержатся в пакете, зная его имя, но при этом не устанавливая его.

- разобравшись с названиями пакетов и их содержимом, ты внезапно понял, что понятия не имеешь, что это за пакеты. Нужно разобраться, как показать полную информацию о пакете, чтобы было понятно, для чего он нужен.
- После этого установи пакеты. Обрати внимание, что теперь при подключении по ssh появилась новая информация.

**1:**
Установим утилиту **apt-file** и проиндексируем репозитории:

```ShellSession
~ $ sudo apt install apt-file
~ $ sudo apt-file update
```

**2:**
С помощью **apt-file** найдем пакет с файлом **landscape-sysinfo**:

```ShellSession
~ $ apt-file search landscape-sysinfo
landscape-common: /usr/bin/landscape-sysinfo
landscape-common: /usr/share/landscape/landscape-sysinfo.wrapper
landscape-common: /usr/share/man/man1/landscape-sysinfo.1.gz
```

С помощью **apt-file** найдем пакет с файлом **ansiweather**:

```ShellSession
~ $ apt-file search ansiweather
ansiweather: /usr/bin/ansiweather
ansiweather: /usr/share/doc/ansiweather/AUTHORS
ansiweather: /usr/share/doc/ansiweather/README.md.gz
ansiweather: /usr/share/doc/ansiweather/ansiweather.plugin.zsh
ansiweather: /usr/share/doc/ansiweather/ansiweatherrc.example
ansiweather: /usr/share/doc/ansiweather/changelog.Debian.gz
ansiweather: /usr/share/doc/ansiweather/copyright
ansiweather: /usr/share/man/man1/ansiweather.1.gz
```

**3:**
Проверим пакеты на наличие файла **/usr/bin/virus**:

```ShellSession
~ $ apt-file show landscape-common | grep /usr/bin/virus
~ $ apt-file show ansiweather | grep /usr/bin/virus
```

**4:**
Посмотрим полную информацию о пакетах найденных:

```ShellSession
~ $ apt show landscape-common -a
~ $ apt show ansiweather -a
```

**5:**
Установим пакеты:

```ShellSession
~ $ sudo apt install landscape-common
~ $ sudo apt install ansiweather
```

**6:**
Проверим:

```ShellSession
~ $ ls -la /etc/update-motd.d/
итого 64
drwxr-xr-x   2 root root  4096 дек 13 23:09 .
drwxr-xr-x 126 root root 12288 дек 13 23:10 ..
-rwxr-xr-x   1 root root  1220 апр  9  2018 00-header
-rw-r--r--   1 root root  1157 апр  9  2018 10-help-text
lrwxrwxrwx   1 root root    46 дек 13 23:09 50-landscape-sysinfo -> /usr/share/landscape/landscape-sysinfo.wrapper
-rw-r--r--   1 root root  4646 сен 27 21:24 50-motd-news
-rw-r--r--   1 root root   604 мар 21  2018 80-esm
-rw-r--r--   1 root root  3017 мар 21  2018 80-livepatch
-rwxr-xr-x   1 root root    97 ноя 12  2018 90-updates-available
-rwxr-xr-x   1 root root   299 июн  3  2019 91-release-upgrade
-rwxr-xr-x   1 root root   165 ноя 25 18:23 92-unattended-upgrades
-rw-r--r--   1 root root   129 ноя 12  2018 95-hwe-eol
-rwxr-xr-x   1 root root   142 ноя 12  2018 98-fsck-at-reboot
-rwxr-xr-x   1 root root   144 ноя 12  2018 98-reboot-required
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

  System information as of Fri Dec 13 23:10:58 MSK 2019

  System load:  0.2                Processes:             230
  Usage of /:   23.6% of 27.52GB   Users logged in:       1
  Memory usage: 14%                IP address for enp0s3: 192.168.41.251
  Swap usage:   0%

Могут быть обновлены 0 пакетов.
0 обновлений касаются безопасности системы.

Last login: Fri Dec 13 22:23:39 2019 from 192.168.41.2
cbpi@cbpi-VirtualBox:~ $
```

