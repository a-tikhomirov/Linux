**task:**
Используя скрипт из установленного пакета, сделай так, чтобы при подключении показывалась информация о погоде в твоем городе. Используй **man** для получения дополнительной информации об использовании скриптов. Должно получиться что-то вроде:

```
Welcome to Ubuntu 18.04.3 LTS (GNU/Linux 5.0.0-23-generic x86_64)

  System load:  0.0                Processes:             248
  Usage of /:   11.6% of 62.74GB   Users logged in:       1
  Memory usage: 51%                IP address for enp0s5: 10.211.55.6
  Swap usage:   0%

 Weather in Moscow => 2 °C - Wind => 4 m/s S - Humidity => 74 % - Pressure => 1018 hPa

240 packages can be updated.
142 updates are security updates.

Last login: Fri Dec 13 05:00:22 2019 from 127.0.0.1
```

- Теперь тебя уже не остановить! Почему бы не сделать так, чтобы эта информация показывалась не только при подключении по ssh, но и когда открываешь Терминал локально...

**1:**
Немного поправим скрипт утилиты **ansiweather**, чтобы город по умолчанию был **Moscow**:

```ShellSession
~ $ which ansiweather 
/usr/bin/ansiweather
~ $ sudo vim /usr/bin/ansiweather
```

```bash
# Location: example "Rzeszow,PL"
#[ -z "$location" ] && location=$(get_config "location" || echo "Rzeszow,PL")
[ -z "$location" ] && location=$(get_config "location" || echo "Moscow")
```

**2:**
Добавим запуск утилиты **ansiweather** в состав **motd**:

Добавим скрипт в **/etc/update-motd.d**:

```ShellSession
~ $ sudo vim /etc/update-motd.d/60-weatherinfo
```

```bash
#!/bin/sh
#
#       60-weatherinfo - print the weather info from ansiweather utility
#
#       Author: Andrey Tikhomirov <andrey.tikhomirov.88@gmail.com>
#

printf "\n"
/usr/bin/ansiweather
```

```ShellSession
~ $ sudo chmod +x /etc/update-motd.d/60-weatherinfo
```

**3:**
Проверим:

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

  System information as of Fri Dec 13 23:51:35 MSK 2019

  System load:  0.07               Processes:             228
  Usage of /:   23.7% of 27.52GB   Users logged in:       1
  Memory usage: 13%                IP address for enp0s3: 192.168.41.251
  Swap usage:   0%

 Weather in Moscow => 0 °C - Wind => 4 m/s SSE - Humidity => 89 % - Pressure => 1015 hPa

Могут быть обновлены 0 пакетов.
0 обновлений касаются безопасности системы.

Last login: Fri Dec 13 23:51:05 2019 from 192.168.41.2
cbpi@cbpi-VirtualBox:~ $
```

**4:**
Добавим отображение информации из **landscape-sysinfo** и **ansiweather** при открытии терминала:

Отредактируем **~/.bashrc**:

```ShellSession
~ $ vim ~/.bashrc
```

```bash
#adding landscape-sysinfo and ansiweather as terminal motd
/usr/share/landscape/landscape-sysinfo.wrapper
echo
/usr/bin/ansiweather
echo
```

**5:**
Проверим, откроем новое окно терминала:

```ShellSession

  System information as of Сб дек 14 00:09:50 MSK 2019

  System load:  0.65               Processes:             229
  Usage of /:   23.8% of 27.52GB   Users logged in:       1
  Memory usage: 15%                IP address for enp0s3: 192.168.41.251
  Swap usage:   0%

 Weather in Moscow => 0 °C - Wind => 4 m/s SSE - Humidity => 89 % - Pressure => 1015 hPa 

cbpi@cbpi-VirtualBox:~ $ 
```
