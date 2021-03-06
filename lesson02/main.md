# **task 1:**
Просмотреть содержимое директорий **/etc, /proc, /home**. Посмотреть пару произвольных
файлов в **/etc**.

Ознакомление с инструментами:

```ShellSession
~ $ whatis ls
...
~ $ man ls
...
~ $ apropos file | grep "view\|print\|output"
...
~ $ man head
...
~ $ man tail
...
~ $ man cat
...
~ $ man more
...
~ $ man less
...
```

Просмотр содержимого директорий:

```ShellSession
~ $ ls /etc
...
~ $ ls /proc
...
~ $ ls /home
...
~ $ ls -U /etc /proc /home
...
```

<details>
  <summary>Результаты вывода команд</summary>
  
```ShellSession
cbpi@cbpi-VirtualBox:~ $ ls /etc
acpi                    debconf.conf         hp               machine-id               printcap           ssl
adduser.conf            debian_version       ifplugd          magic                    profile            subgid
alternatives            default              ImageMagick-6    magic.mime               profile.d          subgid-
anacrontab              deluser.conf         init             mailcap                  protocols          subuid
apg.conf                depmod.d             init.d           mailcap.order            pulse              subuid-
apm                     dhcp                 initramfs-tools  manpath.config           python             sudoers
apparmor                dictionaries-common  inputrc          mc                       python2.7          sudoers.d
apparmor.d              dkms                 insserv.conf.d   mime.types               python3            sysctl.conf
apport                  dpkg                 iproute2         mke2fs.conf              python3.6          sysctl.d
appstream.conf          emacs                issue            modprobe.d               rc0.d              systemd
apt                     environment          issue.net        modules                  rc1.d              terminfo
avahi                   firefox              kernel           modules-load.d           rc2.d              thermald
bash.bashrc             fonts                kernel-img.conf  mtab                     rc3.d              thunderbird
bash_completion         fstab                kerneloops.conf  mtools.conf              rc4.d              timezone
bash_completion.d       fuse.conf            landscape        nanorc                   rc5.d              tmpfiles.d
bindresvport.blacklist  fwupd                ldap             netplan                  rc6.d              ucf.conf
binfmt.d                gai.conf             ld.so.cache      network                  rcS.d              udev
bluetooth               gdb                  ld.so.conf       networkd-dispatcher      request-key.d      udisks2
brlapi.key              gdm3                 ld.so.conf.d     NetworkManager           resolvconf         ufw
brltty                  geoclue              legal            networks                 resolv.conf        updatedb.conf
brltty.conf             ghostscript          libao.conf       newt                     rmt                update-manager
ca-certificates         glvnd                libaudit.conf    nsswitch.conf            rpc                update-motd.d
ca-certificates.conf    gnome                libblockdev      opt                      rsyslog.conf       update-notifier
calendar                groff                libibverbs.d     os-release               rsyslog.d          UPower
chatscripts             group                libnl-3          PackageKit               samba              usb_modeswitch.conf
cifs-utils              group-               libpaper.d       pam.conf                 sane.d             usb_modeswitch.d
console-setup           grub.d               libreoffice      pam.d                    securetty          vim
cracklib                gshadow              libuser.conf     papersize                security           vtrgb
cron.d                  gshadow-             lintianrc        passwd                   selinux            wgetrc
cron.daily              gss                  locale.alias     passwd-                  sensors3.conf      whoopsie
cron.hourly             gtk-2.0              locale.gen       pcmcia                   sensors.d          wpa_supplicant
cron.monthly            gtk-3.0              localtime        perl                     services           X11
crontab                 hdparm.conf          logcheck         pki                      shadow             xdg
cron.weekly             host.conf            login.defs       pm                       shadow-            zsh_command_not_found
cups                    hostname             logrotate.conf   pnm2ppa.conf             shells
cupshelpers             hosts                logrotate.d      polkit-1                 skel
dbus-1                  hosts.allow          lsb-release      popularity-contest.conf  speech-dispatcher
dconf                   hosts.deny           ltrace.conf      ppp                      ssh
cbpi@cbpi-VirtualBox:~ $ ls /proc
1     1212  1398  1632  1780  1847  2063  275   3100  4    46   727  981          interrupts   mounts         timer_list
10    1218  14    167   1784  1849  21    276   3107  40   47   728  acpi         iomem        mtrr           tty
11    1222  1454  17    1785  187   22    277   32    41   474  730  asound       ioports      net            uptime
1114  1224  147   1700  1786  1874  23    2771  320   42   48   734  buddyinfo    irq          pagetypeinfo   version
1142  1263  148   1703  1788  188   24    28    33    43   49   735  bus          kallsyms     partitions     version_signature
1143  1266  149   1705  1790  1896  243   2805  336   431  50   736  cgroups      kcore        pressure       vmallocinfo
1147  13    15    1709  1797  1922  244   2832  34    44   51   740  cmdline      keys         sched_debug    vmstat
1149  1347  150   171   18    1935  245   2837  341   441  511  745  consoles     key-users    schedstat      zoneinfo
1153  1348  151   1712  1801  1970  2471  2855  349   442  52   749  cpuinfo      kmsg         scsi
1158  1357  152   1723  1809  1979  2473  2870  35    443  548  750  crypto       kpagecgroup  self
1160  1358  154   1727  1814  2     248   2886  354   444  56   767  devices      kpagecount   slabinfo
1184  1363  155   1735  1815  20    2494  29    356   445  57   8    diskstats    kpageflags   softirqs
1196  1366  1551  1740  1825  2000  2557  2902  36    446  58   806  dma          loadavg      stat
1199  1370  158   1751  1826  2003  26    291   37    447  6    855  driver       locks        swaps
12    1372  16    1758  1829  2006  2663  2912  372   448  715  9    execdomains  mdstat       sys
1200  1383  1614  1767  1830  2019  2664  2917  374   449  718  965  fb           meminfo      sysrq-trigger
1201  1391  1619  1772  1833  2029  27    3     38    45   719  966  filesystems  misc         sysvipc
1202  1396  1630  1776  1839  2054  2738  30    39    451  723  973  fs           modules      thread-self
cbpi@cbpi-VirtualBox:~ $ ls /home
cbpi
cbpi@cbpi-VirtualBox:~ $ ls -U /etc /proc /home
/etc:
emacs                  request-key.d            mime.types            udisks2              netplan                 pki
skel                   security                 kernel                pam.conf             fuse.conf               issue
passwd-                gai.conf                 updatedb.conf         gdm3                 machine-id              rcS.d
nsswitch.conf          libblockdev              cifs-utils            usb_modeswitch.conf  rmt                     binfmt.d
apg.conf               popularity-contest.conf  initramfs-tools       hosts.allow          apport                  ufw
libibverbs.d           network                  pcmcia                ld.so.conf           logrotate.d             subgid-
brlapi.key             gnome                    whoopsie              ltrace.conf          hostname                default
manpath.config         os-release               landscape             hp                   logcheck                rc2.d
ld.so.cache            libaudit.conf            selinux               dconf                legal                   X11
resolvconf             fonts                    thermald              papersize            timezone                ucf.conf
NetworkManager         group                    firefox               pnm2ppa.conf         cron.monthly            nanorc
thunderbird            passwd                   rc0.d                 protocols            networkd-dispatcher     UPower
debconf.conf           bash.bashrc              ssl                   cron.hourly          services                subuid
gss                    localtime                locale.gen            cracklib             libuser.conf            PackageKit
python3.6              group-                   appstream.conf        dpkg                 ghostscript             apparmor
ld.so.conf.d           resolv.conf              update-manager        acpi                 gdb                     anacrontab
zsh_command_not_found  wpa_supplicant           mtab                  libao.conf           bindresvport.blacklist  grub.d
rc4.d                  modprobe.d               ca-certificates.conf  ImageMagick-6        environment             cron.d
console-setup          subuid-                  cron.daily            sysctl.d             python3                 shadow
mailcap                shadow-                  profile.d             mailcap.order        pulse                   dbus-1
sysctl.conf            init.d                   logrotate.conf        update-motd.d        rsyslog.d               cupshelpers
inputrc                apparmor.d               cron.weekly           crontab              lsb-release             pam.d
update-notifier        vim                      udev                  insserv.conf.d       issue.net               apt
dictionaries-common    securetty                profile               geoclue              sudoers.d               dkms
systemd                fwupd                    sensors.d             hosts                brltty.conf             gshadow-
bash_completion        magic.mime               xdg                   locale.alias         pm                      host.conf
perl                   sensors3.conf            newt                  login.defs           rc1.d                   networks
mc                     modules-load.d           groff                 gtk-3.0              subgid                  cups
rc3.d                  ldap                     terminfo              libnl-3              kerneloops.conf         hosts.deny
libpaper.d             speech-dispatcher        usb_modeswitch.d      brltty               gtk-2.0                 ssh
shells                 apm                      ca-certificates       printcap             depmod.d                rsyslog.conf
hdparm.conf            calendar                 glvnd                 rc5.d                rc6.d                   fstab
modules                bash_completion.d        kernel-img.conf       chatscripts          dhcp                    samba
python                 sane.d                   lintianrc             python2.7            adduser.conf            vtrgb
bluetooth              sudoers                  mtools.conf           debian_version       ppp
rpc                    init                     gshadow               opt                  avahi
wgetrc                 mke2fs.conf              polkit-1              iproute2             libreoffice
ifplugd                magic                    alternatives          tmpfiles.d           deluser.conf

/proc:
fb    iomem    ioports    schedstat          thread-self  18  36  51   171  354  474  749   1158  1348  1630  1776  1830  2029  2886
fs    kcore    loadavg    interrupts         1            20  37  52   187  356  511  750   1160  1357  1632  1780  1833  2054  2902
bus   locks    meminfo    kpagecount         2            21  38  56   188  372  548  767   1184  1358  1700  1784  1839  2063  2912
dma   swaps    modules    kpageflags         3            22  39  57   243  374  715  806   1196  1363  1703  1785  1847  2471  2917
irq   asound   sysvipc    partitions         4            23  40  58   244  431  718  855   1199  1366  1705  1786  1849  2473  3114
net   crypto   version    timer_list         6            24  41  147  245  441  719  965   1200  1370  1709  1788  1874  2494  3118
sys   driver   consoles   execdomains        8            26  42  148  248  442  723  966   1201  1372  1712  1790  1896  2557  3182
tty   mdstat   kallsyms   filesystems        9            27  43  149  275  443  727  973   1202  1383  1723  1797  1922  2663  3224
acpi  mounts   pressure   kpagecgroup        10           28  44  150  276  444  728  981   1212  1391  1727  1801  1935  2664  3287
keys  uptime   slabinfo   sched_debug        11           29  45  151  277  445  730  1114  1218  1396  1735  1809  1970  2738  3326
kmsg  vmstat   softirqs   vmallocinfo        12           30  46  152  291  446  734  1142  1222  1398  1740  1814  1979  2771  3405
misc  cgroups  zoneinfo   pagetypeinfo       14           32  47  154  320  447  735  1143  1224  1454  1751  1815  2000  2805  3424
mtrr  cmdline  buddyinfo  sysrq-trigger      15           33  48  155  336  448  736  1147  1263  1551  1758  1825  2003  2832  3504
scsi  cpuinfo  diskstats  version_signature  16           34  49  158  341  449  740  1149  1266  1614  1767  1826  2006  2837  3568
stat  devices  key-users  self               17           35  50  167  349  451  745  1153  1347  1619  1772  1829  2019  2870

/home:
cbpi
```
  
</details>

Просмотр произвольных файлов в **/etc**:

```ShellSession
~ $ ls -lA /etc | grep ^-
...
~ $ head /etc/bash.bashrc
...
~ $ tail /etc/hdparm.conf
...
~ $ cat /etc/fstab
...
~ $ more /etc/sysctl.conf
...
~ $ less /etc/sysctl.conf
...
```

<details>
  <summary>Результаты вывода команд</summary>
  
```ShellSession
cbpi@cbpi-VirtualBox:~ $ ls -lA /etc | grep ^-
-rw-r--r--  1 root root       3028 авг  5 21:58 adduser.conf
-rw-r--r--  1 root root        401 мая 29  2017 anacrontab
-rw-r--r--  1 root root        433 окт  2  2017 apg.conf
-rw-r--r--  1 root root        769 апр  4  2018 appstream.conf
-rw-r--r--  1 root root       2319 апр  4  2018 bash.bashrc
-rw-r--r--  1 root root         45 апр  2  2018 bash_completion
-rw-r--r--  1 root root        367 янв 27  2016 bindresvport.blacklist
-rw-r-----  1 root root         33 авг  5 22:04 brlapi.key
-rw-r--r--  1 root root      25341 авг 29  2018 brltty.conf
-rw-r--r--  1 root root       5898 авг  5 21:58 ca-certificates.conf
-rw-r--r--  1 root root        722 ноя 16  2017 crontab
-rw-r--r--  1 root root       2969 фев 28  2018 debconf.conf
-rw-r--r--  1 root root         11 июн 26  2017 debian_version
-rw-r--r--  1 root root        604 авг 13  2017 deluser.conf
-rw-r--r--  1 root root         96 авг  5 21:58 environment
-rw-rw-r--  1 root root        594 дек 12 01:46 fstab
-rw-r--r--  1 root root        280 июн 20  2014 fuse.conf
-rw-r--r--  1 root root       2584 фев  1  2018 gai.conf
-rw-r--r--  1 root root        959 дек 14 04:08 group
-rw-r--r--  1 root root        947 дек 13 23:09 group-
-rw-r-----  1 root shadow      796 дек 14 04:08 gshadow
-rw-r-----  1 root shadow      787 дек 13 23:09 gshadow-
-rw-r--r--  1 root root       4861 фев 22  2018 hdparm.conf
-rw-r--r--  1 root root         92 апр  9  2018 host.conf
-rw-r--r--  1 root root         16 дек 12 01:47 hostname
-rw-r--r--  1 root root        230 дек 12 01:47 hosts
-rw-r--r--  1 root root        411 авг  5 22:04 hosts.allow
-rw-r--r--  1 root root        711 авг  5 22:04 hosts.deny
-rw-r--r--  1 root root       1748 мая 15  2017 inputrc
-rw-r--r--  1 root root         26 авг  5 13:43 issue
-rw-r--r--  1 root root         19 авг  5 13:43 issue.net
-rw-r--r--  1 root root        110 дек 12 01:49 kernel-img.conf
-rw-r--r--  1 root root       1308 дек  2  2017 kerneloops.conf
-rw-r--r--  1 root root      77893 дек 17 00:55 ld.so.cache
-rw-r--r--  1 root root         34 янв 27  2016 ld.so.conf
-rw-r--r--  1 root root        267 апр  9  2018 legal
-rw-r--r--  1 root root         27 янв 18  2018 libao.conf
-rw-r--r--  1 root root        191 фев  8  2018 libaudit.conf
-rw-r--r--  1 root root          0 дек 14 04:13 libuser.conf
-rw-r--r--  1 root root       1417 апр  8  2018 lintianrc
-rw-r--r--  1 root root       2995 апр 16  2018 locale.alias
-rw-r--r--  1 root root       9393 дек 12 01:47 locale.gen
-rw-r--r--  1 root root      10550 янв 25  2018 login.defs
-rw-r--r--  1 root root        703 авг 21  2017 logrotate.conf
-rw-r--r--  1 root root        105 авг  5 13:43 lsb-release
-rw-r--r--  1 root root      14867 окт 13  2016 ltrace.conf
-r--r--r--  1 root root         33 дек 12 01:52 machine-id
-rw-r--r--  1 root root        111 фев 13  2018 magic
-rw-r--r--  1 root root        111 фев 13  2018 magic.mime
-rw-r--r--  1 root root      45069 дек 17 00:55 mailcap
-rw-r--r--  1 root root        449 июл 15  2016 mailcap.order
-rw-r--r--  1 root root       5174 авг  4  2018 manpath.config
-rw-r--r--  1 root root      24301 июл 15  2016 mime.types
-rw-r--r--  1 root root        812 мар 24  2018 mke2fs.conf
-rw-r--r--  1 root root        195 авг  5 21:58 modules
-rw-r--r--  1 root root        624 авг  8  2007 mtools.conf
-rw-r--r--  1 root root       9048 фев 14  2018 nanorc
-rw-r--r--  1 root root         91 апр  9  2018 networks
-rw-r--r--  1 root root        556 авг  5 22:05 nsswitch.conf
-rw-r--r--  1 root root        552 апр  5  2018 pam.conf
-rw-rw-r--  1 root root          3 дек 12 01:49 papersize
-rw-r--r--  1 root root       2550 дек 13 23:09 passwd
-rw-r--r--  1 root root       2550 дек 13 23:09 passwd-
-rw-r--r--  1 root root       7649 авг  5 22:05 pnm2ppa.conf
-rw-rw-r--  1 root root        350 дек 12 01:49 popularity-contest.conf
-rw-r--r--  1 root root        581 апр  9  2018 profile
-rw-r--r--  1 root root       2932 дек 26  2016 protocols
-rw-------  1 root root          0 авг  5 21:58 .pwd.lock
-rwxr-xr-x  1 root root        268 июл 21  2017 rmt
-rw-r--r--  1 root root        887 дек 26  2016 rpc
-rw-r--r--  1 root root       1358 янв 30  2018 rsyslog.conf
-rw-r--r--  1 root root       4141 янв 25  2018 securetty
-rw-r--r--  1 root root      10368 апр  5  2017 sensors3.conf
-rw-r--r--  1 root root      19183 дек 26  2016 services
-rw-r-----  1 root shadow     1446 дек 13 23:09 shadow
-rw-r-----  1 root shadow     1446 дек 13 23:09 shadow-
-rw-r--r--  1 root root         73 авг  5 21:58 shells
-rw-r--r--  1 root root         18 дек 12 01:48 subgid
-rw-r--r--  1 root root          0 авг  5 21:58 subgid-
-rw-r--r--  1 root root         18 дек 12 01:48 subuid
-rw-r--r--  1 root root          0 авг  5 21:58 subuid-
-r--r-----  1 root root        755 янв 18  2018 sudoers
-rw-------  1 root root      12288 дек 12 21:57 .sudoers.swp
-rw-r--r--  1 root root       2683 янв 18  2018 sysctl.conf
-rw-r--r--  1 root root         14 дек 12 01:57 timezone
-rw-r--r--  1 root root       1260 фев 26  2018 ucf.conf
-rw-r--r--  1 root root        403 мар  1  2018 updatedb.conf
-rw-r--r--  1 root root       1523 мар  6  2018 usb_modeswitch.conf
-rw-r--r--  1 root root       4942 апр  8  2019 wgetrc
-rw-r--r--  1 root root         30 дек 17 22:26 whoopsie
-rw-r--r--  1 root root        477 мар 16  2018 zsh_command_not_found
cbpi@cbpi-VirtualBox:~ $ head /etc/bash.bashrc 
# System-wide .bashrc file for interactive bash(1) shells.

# To enable the settings / commands in this file for login shells as well,
# this file has to be sourced in /etc/profile.

# If not running interactively, don't do anything
[ -z "$PS1" ] && return

# check the window size after each command and, if necessary,
# update the values of LINES and COLUMNS.
cbpi@cbpi-VirtualBox:~ $ tail /etc/hdparm.conf 
#	interrupt_unmask = on
#	io32_support = 0
#}

#/dev/hda {
#	mult_sect_io = 16
#	write_cache = off
#	dma = on
#}

cbpi@cbpi-VirtualBox:~ $ cat /etc/fstab
# /etc/fstab: static file system information.
#
# Use 'blkid' to print the universally unique identifier for a
# device; this may be used with UUID= as a more robust way to name devices
# that works even if disks are added and removed. See fstab(5).
#
# <file system> <mount point>   <type>  <options>       <dump>  <pass>
# / was on /dev/sda1 during installation
UUID=ae6da97e-7494-4c6e-93de-f395922cdcba /               ext4    errors=remount-ro 0       1
# swap was on /dev/sda5 during installation
UUID=874b184d-666d-4171-acf2-4cf2b788c1de none            swap    sw              0       0
```
  
</details>

# **task 2,3:**

- Выяснить, для чего предназначена команда **cat**. Используя данную команду, создайте два
файла с данными, а затем объедините их в один. Просмотрите содержимое созданного
файла. Переименуйте файл, дав ему новое имя.

Ознакомление с инструментами:

```ShellSession
~ $ whatis cat
...
~ $ man cat
...
```

```ShellSession
cbpi@cbpi-VirtualBox:~/task2and3 $ cat > file1
File 1 text here
cbpi@cbpi-VirtualBox:~/task2and3 $ cat > file2
File 2 text here
Even more text...
cbpi@cbpi-VirtualBox:~/task2and3 $ cat file{1..2} > combinedfile 
cbpi@cbpi-VirtualBox:~/task2and3 $ cat combinedfile 
File 1 text here
File 2 text here
Even more text...
cbpi@cbpi-VirtualBox:~/task2and3 $ ls
combinedfile  file1  file2
cbpi@cbpi-VirtualBox:~/task2and3 $ mv combinedfile renamedFile
cbpi@cbpi-VirtualBox:~/task2and3 $ ls
file1  file2  renamedFile
cbpi@cbpi-VirtualBox:~/task2and3 $ cat file{2..1} > revCombined
cbpi@cbpi-VirtualBox:~/task2and3 $ cat revCombined 
File 2 text here
Even more text...
File 1 text here
```

- Создать несколько файлов. Создайте директорию, переместите файл туда. Удалите все
созданные в этом и предыдущем задании директории и файлы.

```ShellSession
cbpi@cbpi-VirtualBox:~/task2and3 $ ls
file1  file2  renamedFile  revCombined
cbpi@cbpi-VirtualBox:~/task2and3 $ > newFile
cbpi@cbpi-VirtualBox:~/task2and3 $ ls
file1  file2  newFile  renamedFile  revCombined
cbpi@cbpi-VirtualBox:~/task2and3 $ touch moreFiles{1..5}
cbpi@cbpi-VirtualBox:~/task2and3 $ ls
file1  file2  moreFiles1  moreFiles2  moreFiles3  moreFiles4  moreFiles5  newFile  renamedFile  revCombined
cbpi@cbpi-VirtualBox:~/task2and3 $ mkdir newDir
cbpi@cbpi-VirtualBox:~/task2and3 $ mv newFile newDir/
cbpi@cbpi-VirtualBox:~/task2and3 $ ls newDir/
newFile
cbpi@cbpi-VirtualBox:~/task2and3 $ mv moreFiles{1..5} newDir/
cbpi@cbpi-VirtualBox:~/task2and3 $ ls newDir/
moreFiles1  moreFiles2  moreFiles3  moreFiles4  moreFiles5  newFile
cbpi@cbpi-VirtualBox:~/task2and3 $ ls
file1  file2  newDir  renamedFile  revCombined
cbpi@cbpi-VirtualBox:~/task2and3 $ rm file* re*
cbpi@cbpi-VirtualBox:~/task2and3 $ ls
newDir
cbpi@cbpi-VirtualBox:~/task2and3 $ cd ..
cbpi@cbpi-VirtualBox:~ $ rm -r task2and3/
cbpi@cbpi-VirtualBox:~ $ ls
 bin                file2.txt   photos   SystemdAnalyzePlot.svg   Документы   Изображения   Общедоступные   Шаблоны
 examples.desktop   graph.svf   snap     Видео                    Загрузки    Музыка       'Рабочий стол'
```

# **task 4:**
В ОС Linux скрытыми файлами считаются те, имена которых начинаются с символа **“.”**.
Сколько скрытых файлов в вашем домашнем каталоге? (Использовать конвейер. Подсказка:
для подсчета количества строк можно использовать **wc**).

```ShellSession
~ $ ls -A
...
~ $ ls -d .!(|.)
...
~ $ ls -A | grep "^\."
...
~ $ ls -A | wc -l
46
~ $ ls -d .!(|.) | wc -l
29
~ $ ls -A | grep "^\." | wc -l
29
~ $ ls | wc -l
17
~ $ echo $(ls -A | wc -l) = $(ls -Ad .!(|.) | wc -l) + $(ls | wc -l)
46 = 29 + 17
~ $ echo $(ls -A | wc -l) = $(($(ls -Ad .!(|.) | wc -l) + $(ls | wc -l)))
46 = 46
~ $ ls -ld .!(|.) | grep -v ^d
...
~ $ ls -ld .!(|.) | grep ^- | wc -l
18
```

**Скрытых файлов (включая директории) в домашнем каталоге: 29**

**Скрытых файлов (не включая директории) в домашнем каталоге: 18**

<details>
<summary>Результат вывода команд</summary>
  
```ShellSession
cbpi@cbpi-VirtualBox:~ $ ls -A
 .bash           divider            .landscape         .sudo_as_admin_successful   .vboxclient-draganddrop.pid   Загрузки
 .bash_history   divider.c          .lesshst           .swp                        .vboxclient-seamless.pid      Изображения
 .bash_logout    examples.desktop   .local             SystemdAnalyzePlot.svg      .viminfo                      Музыка
 .bashrc         .gitconfig         .mozilla           task8                       .Xauthority                   Общедоступные
 bin             .gnupg             photos             .thunderbird                .xinputrc                    'Рабочий стол'
 .cache          graph.svf          .profile           tmp                         .xsession-errors              Шаблоны
 .config         .gvfs              .selected_editor   .vboxclient-clipboard.pid   Видео
 .dbus           .ICEauthority      .ssh               .vboxclient-display.pid     Документы
cbpi@cbpi-VirtualBox:~ $ ls -d .!(|.)
.bash          .config     .ICEauthority  .profile                   .thunderbird                 .viminfo
.bash_history  .dbus       .landscape     .selected_editor           .vboxclient-clipboard.pid    .Xauthority
.bash_logout   .gitconfig  .lesshst       .ssh                       .vboxclient-display.pid      .xinputrc
.bashrc        .gnupg      .local         .sudo_as_admin_successful  .vboxclient-draganddrop.pid  .xsession-errors
.cache         .gvfs       .mozilla       .swp                       .vboxclient-seamless.pid
cbpi@cbpi-VirtualBox:~ $ ls -A | grep "^\."
.bash
.bash_history
.bash_logout
.bashrc
.cache
.config
.dbus
.gitconfig
.gnupg
.gvfs
.ICEauthority
.landscape
.lesshst
.local
.mozilla
.profile
.selected_editor
.ssh
.sudo_as_admin_successful
.swp
.thunderbird
.vboxclient-clipboard.pid
.vboxclient-display.pid
.vboxclient-draganddrop.pid
.vboxclient-seamless.pid
.viminfo
.Xauthority
.xinputrc
.xsession-errors
cbpi@cbpi-VirtualBox:~ $ ls -A | wc -l
46
cbpi@cbpi-VirtualBox:~ $ ls -d .!(|.) | wc -l
29
cbpi@cbpi-VirtualBox:~ $ ls -A | grep "^\." | wc -l
29
cbpi@cbpi-VirtualBox:~ $ ls | wc -l
17
cbpi@cbpi-VirtualBox:~ $ echo $(ls -A | wc -l) = $(ls -d .!(|.) | wc -l) + $(ls | wc -l)
46 = 29 + 17
cbpi@cbpi-VirtualBox:~ $ echo $(ls -A | wc -l) = $(($(ls -d .!(|.) | wc -l) + $(ls | wc -l)))
46 = 46
cbpi@cbpi-VirtualBox:~ $ ls -ld .!(|.) | grep -v ^d
-rw-------  1 cbpi cbpi 41438 дек 22 22:31 .bash_history
-rw-r--r--  1 cbpi cbpi   220 дек 12 01:48 .bash_logout
-rw-r--r--  1 cbpi cbpi  4326 дек 15 21:37 .bashrc
-rw-r--r--  1 cbpi cbpi    68 дек 13 16:37 .gitconfig
-rw-------  1 cbpi cbpi 20632 дек 23 11:18 .ICEauthority
-rw-------  1 cbpi cbpi    35 дек 16 16:10 .lesshst
-rw-r--r--  1 cbpi cbpi   807 дек 12 01:48 .profile
-rw-r--r--  1 cbpi cbpi    72 дек 12 20:30 .selected_editor
-rw-r--r--  1 cbpi cbpi     0 дек 12 02:02 .sudo_as_admin_successful
-rw-------  1 cbpi cbpi 36864 дек 18 22:58 .swp
-rw-r-----  1 cbpi cbpi     5 дек 23 11:18 .vboxclient-clipboard.pid
-rw-r-----  1 cbpi cbpi     5 дек 23 11:18 .vboxclient-display.pid
-rw-r-----  1 cbpi cbpi     5 дек 23 11:18 .vboxclient-draganddrop.pid
-rw-r-----  1 cbpi cbpi     5 дек 23 11:18 .vboxclient-seamless.pid
-rw-------  1 root root 10767 дек 21 20:54 .viminfo
-rw-------  1 cbpi cbpi     0 дек 16 18:57 .Xauthority
-rw-rw-r--  1 cbpi cbpi   131 дек 17 17:53 .xinputrc
-rw-------  1 cbpi cbpi     0 дек 16 18:56 .xsession-errors
cbpi@cbpi-VirtualBox:~ $ ls -ld .!(|.) | grep -v ^d | wc -l
18
```
  
</details>

# **task 5:**
Попробовать вывести с помощью **cat** все файлы в директории **/etc**. Направить ошибки в
отдельный файл в вашей домашней директории. Сколько файлов, которые не удалось
посмотреть, оказалось в списке?

- Вариант 1

```ShellSession
cbpi@cbpi-VirtualBox:~ $ cat /etc/* 2> ~/catErrOut | tail
# installation of packages available from the repository

if [[ -x /usr/lib/command-not-found ]] ; then
	if (( ! ${+functions[command_not_found_handler]} )) ; then
		function command_not_found_handler {
			[[ -x /usr/lib/command-not-found ]] || return 1
			/usr/lib/command-not-found --no-failure-msg -- ${1+"$1"} && :
		}
	fi
fi
cbpi@cbpi-VirtualBox:~ $ cat catErrOut | wc -l
138
cbpi@cbpi-VirtualBox:~ $ cat catErrOut | grep каталог | wc -l
132
cbpi@cbpi-VirtualBox:~ $ cat catErrOut | grep -v каталог
cat: /etc/brlapi.key: Отказано в доступе
cat: /etc/gshadow: Отказано в доступе
cat: /etc/gshadow-: Отказано в доступе
cat: /etc/shadow: Отказано в доступе
cat: /etc/shadow-: Отказано в доступе
cat: /etc/sudoers: Отказано в доступе
cbpi@cbpi-VirtualBox:~ $ cat catErrOut | grep -v каталог | wc -l
6
cbpi@cbpi-VirtualBox:~ $ ls -lAd /etc/.*
drwxr-xr-x 131 root root 12288 дек 17 22:26 /etc/.
drwxr-xr-x  24 root root  4096 дек 14 17:21 /etc/..
-rw-------   1 root root     0 авг  5 21:58 /etc/.pwd.lock
-rw-------   1 root root 12288 дек 12 21:57 /etc/.sudoers.swp
cbpi@cbpi-VirtualBox:~ $ ls -Ad /etc/.!(|.)
/etc/.pwd.lock  /etc/.sudoers.swp
cbpi@cbpi-VirtualBox:~ $ ls -Ad /etc/.!(|.) | xargs cat 2> ~/catErrOut2
cbpi@cbpi-VirtualBox:~ $ cat catErrOut2
cat: /etc/.pwd.lock: Отказано в доступе
cat: /etc/.sudoers.swp: Отказано в доступе
cbpi@cbpi-VirtualBox:~ $ cat catErrOut2 | wc -l
2
cbpi@cbpi-VirtualBox:~ $ GLOBIGNORE=/etc/.:/etc/..
cbpi@cbpi-VirtualBox:~ $ cat /etc/.* 2> ~/catErrOut2
cbpi@cbpi-VirtualBox:~ $ cat catErrOut2 
cat: /etc/.pwd.lock: Отказано в доступе
cat: /etc/.sudoers.swp: Отказано в доступе
cbpi@cbpi-VirtualBox:~ $ GLOBIGNORE=
```

**Не удалось прочитать файлов (по причине: "Это каталог"): 132**

**Не удалось прочитать файлов (по причине: "Отказано в доступе"): 6**

**Не удалось прочитать скрытых файлов (по причине: "Отказано в доступе"): 2**

- Вариант 2

```ShellSession
cbpi@cbpi-VirtualBox:~ $ shopt -s dotglob nullglob 
cbpi@cbpi-VirtualBox:~ $ cat /etc/* 2> ~/catErrOut | tail -5
			[[ -x /usr/lib/command-not-found ]] || return 1
			/usr/lib/command-not-found --no-failure-msg -- ${1+"$1"} && :
		}
	fi
fi
cbpi@cbpi-VirtualBox:~ $ cat catErrOut | wc -l
140
cbpi@cbpi-VirtualBox:~ $ cat catErrOut | cut -d" " -f3- | sort | uniq
Отказано в доступе
Это каталог
cbpi@cbpi-VirtualBox:~ $ cat catErrOut | grep каталог | wc -l
132
cbpi@cbpi-VirtualBox:~ $ cat catErrOut | grep доступ | wc -l
8
cbpi@cbpi-VirtualBox:~ $ cat catErrOut | grep "/\."
cat: /etc/.pwd.lock: Отказано в доступе
cat: /etc/.sudoers.swp: Отказано в доступе
cbpi@cbpi-VirtualBox:~ $ shopt -u dotglob nullglob
```

**Не удалось прочитать файлов всего: 140**

**Не удалось прочитать файлов (по причине: "Это каталог"): 132**

**Не удалось прочитать файлов (по причине: "Отказано в доступе"): 8**

**Из них - не удалось прочитать скрытых файлов (по причине: "Отказано в доступе"): 2**

# **task 6:**
Запустить в одном терминале программу, в другом терминале посмотреть **PID** процесса и
остановить с помощью **kill**, посылая разные типы сигналов. Что происходит?

<details>
<summary> **1 (SIGHUP)** </summary>

Первый терминал:

```ShellSession
cbpi@cbpi-VirtualBox:~ $ mc
```

Другой терминал, отправляем сигнал **1 (SIGHUP)**:

```ShellSession
cbpi@cbpi-VirtualBox:~ $ ps aux | grep [m]c
cbpi      2709  0.0  0.0  76492  9856 pts/0    S+   12:57   0:00 mc
cbpi@cbpi-VirtualBox:~ $ kill -1 2709
cbpi@cbpi-VirtualBox:~ $ ps aux | grep [m]c
```

Приложение **mc** было проинформировано о якобы завершении родительского процесса (терминала) и завершило свою работу, в первом терминале отобразилась информация:

```
Обрыв терминальной линии
```

</details>

<details>
<summary> **2 (SIGINT)** </summary>

Первый терминал:

```ShellSession
cbpi@cbpi-VirtualBox:~ $ ls | grep sig
cbpi@cbpi-VirtualBox:~ $ cat > sigintTest
testing SIGINT...
```

Другой терминал, отправляем сигнал **2 (SIGINT)**:

```ShellSession
bpi@cbpi-VirtualBox:~ $ ps aux | grep [b]ash
cbpi      3421  0.0  0.0  30080  5504 pts/0    Ss   13:00   0:00 bash
cbpi      3660  0.1  0.0  29948  5356 pts/1    Ss   13:01   0:00 bash
cbpi@cbpi-VirtualBox:~ $ echo $$
3660
cbpi@cbpi-VirtualBox:~ $ pstree -p 3421
bash(3421)───cat(3648)
cbpi@cbpi-VirtualBox:~ $ kill -2 3648
cbpi@cbpi-VirtualBox:~ $ pstree -p 3421
bash(3421)
```

Дочернему процессу **cat** родительского процеса **bash** был отправлен сигнал остановки, приложение **cat** завершилось, вводимый текст остался на экране, был создан пустой файл **sigintTest**: 

Первый терминал:

```ShellSession
cbpi@cbpi-VirtualBox:~ $ cat > sigintTest
Testing SIGIINT...
cbpi@cbpi-VirtualBox:~ $ Testing SIGIINT...^C
cbpi@cbpi-VirtualBox:~ $ ls | grep sig
sigintTest
```

</details>

<details>
<summary> **8 (SIGFPE)** </summary>

- Вариант 1.

Первый терминал:

```ShellSession
cbpi@cbpi-VirtualBox:~ $ gnome-calculator &
[1] 4618
```

Другой терминал, отправляем сигнал **8 (SIGFPE)**:

```ShellSession
cbpi@cbpi-VirtualBox:~ $ ps aux | grep [g]nome-calc
cbpi      4618  3.4  0.2 798656 40972 pts/0    Sl   13:14   0:00 /snap/gnome-calculator/544/usr/bin/gnome-calculator
cbpi@cbpi-VirtualBox:~ $ ps -o ppid= 6444 | xargs pstree -p
bash(5410)───gnome-calculato(6444)─┬─{gnome-calculato}(6626)
                                   ├─{gnome-calculato}(6627)
                                   └─{gnome-calculato}(6628)
cbpi@cbpi-VirtualBox:~ $ kill -8 6444
cbpi@cbpi-VirtualBox:~ $ pstree -p 5410
bash(5410)
cbpi@cbpi-VirtualBox:~ $ cat /var/log/apport.log
ERROR: apport (pid 6652) Mon Dec 23 15:10:39 2019: host pid 6444 crashed in a separate mount namespace, ignoring
```

Процессу **gnome-calculato** был отправлен сигнал **SIGFPE**, дамп памяти был передан в систему **Apport** и приложение **gnome-calculato** экстренно завершило свою работу, системой **Apport** был проанализирован краш программы, в результате чего было решено не записывать дамп памяти. В первом терминале было отображено сообщение об исключении: 

Первый терминал:

```ShellSession
cbpi@cbpi-VirtualBox:~ $ fg
bash: fg: выполнение задания прервано
[1]+  Исключение в операции с плавающей точкой                                                   (стек памяти сброшен на диск) gnome-calculator
```

- Вариант 2.

Первый терминал:

```ShellSession
cbpi@cbpi-VirtualBox:~ $ bc
bc 1.07.1
Copyright 1991-1994, 1997, 1998, 2000, 2004, 2006, 2008, 2012-2017 Free Software Foundation, Inc.
This is free software with ABSOLUTELY NO WARRANTY.
For details type `warranty'. 
```

Другой терминал, отправляем сигнал **8 (SIGFPE)**:

```ShellSession
cbpi@cbpi-VirtualBox:~ $ ps aux | grep [b]c
cbpi      6697  0.0  0.0  19328  2732 pts/0    S+   15:14   0:00 bc
cbpi@cbpi-VirtualBox:~ $ ps -o ppid= 6697 | xargs pstree -p
bash(5410)───bc(6697)
cbpi@cbpi-VirtualBox:~ $ kill -8 6697
cbpi@cbpi-VirtualBox:~ $ pstree -p 5410
bash(5410)
cbpi@cbpi-VirtualBox:~ $ cat /var/log/apport.log
ERROR: apport (pid 6652) Mon Dec 23 15:10:39 2019: host pid 6444 crashed in a separate mount namespace, ignoring
ERROR: apport (pid 6714) Mon Dec 23 15:16:20 2019: called for pid 6697, signal 8, core limit 0, dump mode 1
ERROR: apport (pid 6714) Mon Dec 23 15:16:20 2019: executable: /usr/bin/bc (command line "bc")
ERROR: apport (pid 6714) Mon Dec 23 15:16:20 2019: debug: session gdbus call: (true,)

ERROR: apport (pid 6714) Mon Dec 23 15:16:20 2019: wrote report /var/crash/_usr_bin_bc.1000.crash
cbpi@cbpi-VirtualBox:~ $ head -10 /var/crash/_usr_bin_bc.1000.crash 
ProblemType: Crash
Architecture: amd64
CurrentDesktop: ubuntu:GNOME
Date: Mon Dec 23 15:16:20 2019
DistroRelease: Ubuntu 18.04
ExecutablePath: /usr/bin/bc
ExecutableTimestamp: 1521735634
ProcCmdline: bc
ProcCwd: /home/cbpi
ProcEnviron:
```

Процессу **bc** был отправлен сигнал **SIGFPE**, дамп памяти был передан в систему **Apport** и приложение **bc** завершило свою работу, системой **Apport** был проанализирован краш программы, в результате чего был записан дамп памяти в **/var/crash/_usr_bin_bc.1000.crash**. В первом терминале было отображено сообщение об исключении: 

Первый терминал:

```ShellSession
For details type `warranty'.
Исключение в операции с плавающей точкой (стек памяти сброшен на диск)
cbpi@cbpi-VirtualBox:~ $ 
```

- Вариант 3.

Напишем небольшую программу на **C** (скрипт bash не подойдет, т.к. там собственный обработчик деления на ноль).

```ShellSession
cbpi@cbpi-VirtualBox:~ $ vim divider.c
```

```C
#include <sys/types.h>
#include <signal.h>
#include <stdio.h>
#include <stdlib.h>

void signal_handler (int signo) {
    switch (signo)
    {
        case SIGFPE:
            printf("Caught FPE\n");
            exit(-1);
            break;
        case SIGSEGV:
            printf("Caught segmentation error\n");
            exit(-1);
            break;
    }
}

int main(int argc, char *argv[])
{
    signal(SIGFPE,(*signal_handler));
    signal(SIGSEGV,(*signal_handler));

    if(argc!=3){
        printf("Two digit arguments must be passed\n");
        return 1;
    }

    int value = atoi(argv[1]);
    int divider = atoi(argv[2]);
    int result = value/divider;

    printf("Divide result is %d\n", result);

    return 0;
}
```

```ShellSession
cbpi@cbpi-VirtualBox:~ $ gcc divider.c -o divider
cbpi@cbpi-VirtualBox:~ $ ./divider 41 41
Divide result is 1
cbpi@cbpi-VirtualBox:~ $ echo $?
0
cbpi@cbpi-VirtualBox:~ $ ./divider 41 0
Caught FPE
cbpi@cbpi-VirtualBox:~ $ echo $?
255
```

При делении на ноль был сгенерирован и обработан сигнал **SIGFPE**. 

</details>

<details>
<summary> **9 (SIGKILL)** </summary>

Первый терминал:

```ShellSession
cbpi@cbpi-VirtualBox:~ $ vim sigKillTest
```

```vim
Testing SIGKILL...
```

Другой терминал, отправляем сигнал **9 (SIGKILL)**:

```ShellSession
bpi@cbpi-VirtualBox:~ $ ps aux | grep [v]im
cbpi      4743  0.1  0.0  62680  9348 pts/0    S+   13:17   0:00 vim sgkillTest
cbpi@cbpi-VirtualBox:~ $ ps -o ppid= 4743 | xargs pstree -p
bash(3421)───vim(4743)
cbpi@cbpi-VirtualBox:~ $ kill -9 4743
cbpi@cbpi-VirtualBox:~ $  pstree -p 3421
bash(3421)
```

Процессу **vim** был отправлен сигнал безусловного завершения работы, приложение **vim** было немедленно завершено, в первом терминале было отображено соответствующее сообщение, однако из-за немедленного завершения (т.к. данный сигнал не обрабатывается программой) остался баг отображения интерфейса **vim** в терминале: 

Первый терминал:

```ShellSession
Testing SIGKILL...Убито
cbpi@cbpi-VirtualBox:~ $
~
~
~
~
```

</details>

<details>
<summary> **11 (SIGSEGV)** </summary>

Первый терминал:

```ShellSession
cbpi@cbpi-VirtualBox:~ $ sol
```

Другой терминал, отправляем сигнал **11 (SIGSEGV)**:

```ShellSession
cbpi@cbpi-VirtualBox:~ $ ps aux | grep [s]ol
systemd+   468  0.0  0.0  71020  6520 ?        Ss   11:17   0:00 /lib/systemd/systemd-resolved
cbpi      7481  0.6  0.2 526372 43204 pts/0    Sl+  16:18   0:00 sol
cbpi@cbpi-VirtualBox:~ $ ps -o ppid= 7481 | xargs pstree -p
bash(5410)───sol(7481)─┬─{sol}(7482)
                       ├─{sol}(7483)
                       ├─{sol}(7484)
                       ├─{sol}(7485)
                       ├─{sol}(7486)
                       └─{sol}(7487)
cbpi@cbpi-VirtualBox:~ $ kill -11 7481
cbpi@cbpi-VirtualBox:~ $ pstree -p 5410
bash(5410)
cbpi@cbpi-VirtualBox:~ $ tail -5 /var/log/apport.log
ERROR: apport (pid 7514) Mon Dec 23 16:19:56 2019: called for pid 7481, signal 11, core limit 0, dump mode 1
ERROR: apport (pid 7514) Mon Dec 23 16:19:56 2019: executable: /usr/games/sol (command line "sol")
ERROR: apport (pid 7514) Mon Dec 23 16:19:56 2019: debug: session gdbus call: (true,)

ERROR: apport (pid 7514) Mon Dec 23 16:19:58 2019: wrote report /var/crash/_usr_games_sol.1000.crash
cbpi@cbpi-VirtualBox:~ $ head -10 /var/crash/_usr_games_sol.1000.crash
ProblemType: Crash
Architecture: amd64
CurrentDesktop: ubuntu:GNOME
Date: Mon Dec 23 16:19:56 2019
DistroRelease: Ubuntu 18.04
ExecutablePath: /usr/games/sol
ExecutableTimestamp: 1520891432
ProcCmdline: sol
ProcCwd: /home/cbpi
ProcEnviron:
```

Процессу **sol** был отправлен сигнал ошибки сегметирования (якобы программа обратилась к не принадлежащей ей
области памяти), дамп памяти был передан в систему **Apport** и приложение **sol** завершило свою работу, системой **Apport** был проанализирован краш программы, в результате чего был записан дамп памяти в **/var/crash/_usr_games_sol.1000.crash**. В первом терминале было отображено соответствующее сообщение: 

Первый терминал:

```ShellSession
cbpi@cbpi-VirtualBox:~ $ sol
Ошибка сегментирования (стек памяти сброшен на диск)
```

Также было отображено сообщение о внезапном закрытии программы и предложение отправить **Отчет об ошибке** разработчикам =)

```
ExecutablePath
  /usr/games/sol
Package
  aicelot 1:3.22.5-1
ProblemType
  Crash
Title
  sol crashed with SEGEGV in_GI__poll()
...
```

</details>

<details>
<summary> **15 (SIGTERM)** </summary>

Первый терминал:

```ShellSession
cbpi@cbpi-VirtualBox:~ $ vim sigKillTest
```

```vim
Testing SIGKILL...
```

Другой терминал, отправляем сигнал **15 (SIGTERM)**:

```ShellSession
cbpi@cbpi-VirtualBox:~ $ ps aux | grep [v]im
cbpi      5820  0.0  0.0  62680  9372 pts/0    S+   13:29   0:00 vim sigkillTest
cbpi@cbpi-VirtualBox:~ $ ps -o ppid= 5820 | xargs pstree -p
bash(3421)───vim(5820)
cbpi@cbpi-VirtualBox:~ $ kill 5820
cbpi@cbpi-VirtualBox:~ $ pstree -p 3421
bash(3421)
```

Процессу **vim** был отправлен сигнал запроса завершения работы, приложение **vim** завершило свою работу в нормальном режиме (благодаря предусмотренному в программе обработчику данного сигнала), в первом терминале было отображено соответствующее сообщение: 

Первый терминал:

```ShellSession
cbpi@cbpi-VirtualBox:~ $ vim sgkillTest
Vim: Caught deadly signal TERM
Vim: preserving files...
Vim: Finished.
Завершено
cbpi@cbpi-VirtualBox:~ $ 
```

</details>

# **task 7:**
Отобразить в **/dev** список устройств, которые в настоящее время используются вашим **UID**
(для этого используется команда **lsof**). Организовать конвейер через **less**, чтобы посмотреть
их должным образом.

Важно, используем **-a**, чтобы все опции команды **lsof** воспринимались в режиме **AND**, а не **OR**.

```ShellSession
cbpi@cbpi-VirtualBox:~ $ man lsof
...
cbpi@cbpi-VirtualBox:~ $ lsof -a +c 15 -u $UID /dev | less
...
cbpi@cbpi-VirtualBox:~ $ lsof -a +c 15 -u $UID /dev | wc -l
142
cbpi@cbpi-VirtualBox:~ $ lsof -a +c 15 -u $UID /dev | tail -5
Web\x20Content  5767 cbpi   52r   CHR    1,9      0t0   11 /dev/urandom
Web\x20Content  5836 cbpi    0r   CHR    1,3      0t0    6 /dev/null
Web\x20Content  5836 cbpi   52r   CHR    1,9      0t0   11 /dev/urandom
Web\x20Content  6150 cbpi    0r   CHR    1,3      0t0    6 /dev/null
Web\x20Content  6150 cbpi   52r   CHR    1,9      0t0   11 /dev/urandom
```

Попробуем задействовать еше одно устройство (**tty2**) как пользователь **cbpi**.
Для этого нажмем **Ctrl+Alt+F2** и авторизуемся как **cbpi**.
Проверим, добавились ли устройства в **/dev**:

```ShellSession
cbpi@cbpi-VirtualBox:~ $ lsof -a +c 15 -u $UID /dev | wc -l
147
cbpi@cbpi-VirtualBox:~ $ lsof -a +c 15 -u $UID /dev | tail -5
Web\x20Content  6150 cbpi   52r   CHR    1,9      0t0   11 /dev/urandom
bash            7624 cbpi    0u   CHR    4,2      0t0   23 /dev/tty2
bash            7624 cbpi    1u   CHR    4,2      0t0   23 /dev/tty2
bash            7624 cbpi    2u   CHR    4,2      0t0   23 /dev/tty2
bash            7624 cbpi  255u   CHR    4,2      0t0   23 /dev/tty2
```

Попробуем задействовать еше одно устройство (**tty4**) как пользователь **root**.
Для этого нажмем **Ctrl+Alt+F4** и авторизуемся как **root**.
Проверим, добавились ли устройства в **/dev**:

```ShellSession
cbpi@cbpi-VirtualBox:~ $ lsof -a +c 15 -u $UID /dev | grep tty
gdm-x-session   1217 cbpi    0u   CHR    4,1      0t0   22 /dev/tty1
Xorg            1219 cbpi    0u   CHR    4,1      0t0   22 /dev/tty1
Xorg            1219 cbpi   11u   CHR    4,1      0t0   22 /dev/tty1
pager           5176 cbpi    3r   CHR    5,0      0t0   13 /dev/tty
bash            7624 cbpi    0u   CHR    4,2      0t0   23 /dev/tty2
bash            7624 cbpi    1u   CHR    4,2      0t0   23 /dev/tty2
bash            7624 cbpi    2u   CHR    4,2      0t0   23 /dev/tty2
bash            7624 cbpi  255u   CHR    4,2      0t0   23 /dev/tty2
cbpi@cbpi-VirtualBox:~ $ sudo lsof -a +c 15 -u root /dev | grep tty4
login           8528 root    0u   CHR    4,4      0t0   25 /dev/tty4
login           8528 root    1u   CHR    4,4      0t0   25 /dev/tty4
login           8528 root    2u   CHR    4,4      0t0   25 /dev/tty4
bash            8543 root    0u   CHR    4,4      0t0   25 /dev/tty4
bash            8543 root    1u   CHR    4,4      0t0   25 /dev/tty4
bash            8543 root    2u   CHR    4,4      0t0   25 /dev/tty4
bash            8543 root  255u   CHR    4,4      0t0   25 /dev/tty4
```

# **task 8,9:**
- Cоздайте директорию для хранения фотографий, в ней должны быть директории по годам,
(например, за последние 5 лет), и в каждой директории года по директории для месяца.

```ShellSession
cbpi@cbpi-VirtualBox:~ $ mkdir task8
cbpi@cbpi-VirtualBox:~ $ cd task8/
cbpi@cbpi-VirtualBox:~/task8 $ ls -a
.  ..
cbpi@cbpi-VirtualBox:~/task8 $ mkdir -p 20{15..19}/{01..12}
cbpi@cbpi-VirtualBox:~/task8 $ ls ./*
./2015:
01  02  03  04  05  06  07  08  09  10  11  12

./2016:
01  02  03  04  05  06  07  08  09  10  11  12

./2017:
01  02  03  04  05  06  07  08  09  10  11  12

./2018:
01  02  03  04  05  06  07  08  09  10  11  12

./2019:
01  02  03  04  05  06  07  08  09  10  11  12
```

- Заполните директории файлами вида **ГГГГММДДНН.txt**. Внутри файла должно быть имя файла. Например **2018011301.txt**, **2018011302.txt**.

Для заполнения директорий файлами напишем скрипт:

```ShellSession
cbpi@cbpi-VirtualBox:~/task8 $ vim filesToDirs.sh
```

- Вариант 1
```sh
#!/bin/bash
for i in $(ls -Ad ./*/ | cut -d/ -f2)
do
    for j in $(ls -Ad ./$i/*/ | cut -d/ -f3)
    do
        days=$(date -d "$j/01/$i +1month -1day" +%d)
        for k in $(seq 1 $days)
        do
            if [ $k -lt 10 ]; then
                day=0$k
            else
                day=$k
            fi
            for l in {01..05}
            do
                echo File $i$j$day$l text here > $i/$j/$i$j$day$l.txt
            done
        done
    done
done
```

- Вариант 2:
```sh
#!/bin/bash
for path in $(find -mindepth 2 -type d)
do
    year=$(echo $path | cut -d/ -f2)
    month=$(echo $path | cut -d/ -f3)
    days=$(date -d "$month/01/$year +1month -1day" +%d)
    for k in $(seq 1 $days)
    do
        if [ $k -lt 10 ]; then
            day=0$k
        else
            day=$k
        fi
        for i in {01..05}
        do
            echo File $year$month$day$i text here > $path/$year$month$day$i.txt
        done
    done
done
```

```ShellSession
cbpi@cbpi-VirtualBox:~/task8 $ chmod 751 filesToDirs.sh
cbpi@cbpi-VirtualBox:~/task8 $ ./filesToDirs.sh
```

<details>
<summary>Проверка заполнения директорий</summary>

```ShellSession
cbpi@cbpi-VirtualBox:~/task8 $ ls 2015/02 2016/02
2015/02:
2015020101.txt  2015020404.txt  2015020802.txt  2015021105.txt  2015021503.txt  2015021901.txt  2015022204.txt  2015022602.txt
2015020102.txt  2015020405.txt  2015020803.txt  2015021201.txt  2015021504.txt  2015021902.txt  2015022205.txt  2015022603.txt
2015020103.txt  2015020501.txt  2015020804.txt  2015021202.txt  2015021505.txt  2015021903.txt  2015022301.txt  2015022604.txt
2015020104.txt  2015020502.txt  2015020805.txt  2015021203.txt  2015021601.txt  2015021904.txt  2015022302.txt  2015022605.txt
2015020105.txt  2015020503.txt  2015020901.txt  2015021204.txt  2015021602.txt  2015021905.txt  2015022303.txt  2015022701.txt
2015020201.txt  2015020504.txt  2015020902.txt  2015021205.txt  2015021603.txt  2015022001.txt  2015022304.txt  2015022702.txt
2015020202.txt  2015020505.txt  2015020903.txt  2015021301.txt  2015021604.txt  2015022002.txt  2015022305.txt  2015022703.txt
2015020203.txt  2015020601.txt  2015020904.txt  2015021302.txt  2015021605.txt  2015022003.txt  2015022401.txt  2015022704.txt
2015020204.txt  2015020602.txt  2015020905.txt  2015021303.txt  2015021701.txt  2015022004.txt  2015022402.txt  2015022705.txt
2015020205.txt  2015020603.txt  2015021001.txt  2015021304.txt  2015021702.txt  2015022005.txt  2015022403.txt  2015022801.txt
2015020301.txt  2015020604.txt  2015021002.txt  2015021305.txt  2015021703.txt  2015022101.txt  2015022404.txt  2015022802.txt
2015020302.txt  2015020605.txt  2015021003.txt  2015021401.txt  2015021704.txt  2015022102.txt  2015022405.txt  2015022803.txt
2015020303.txt  2015020701.txt  2015021004.txt  2015021402.txt  2015021705.txt  2015022103.txt  2015022501.txt  2015022804.txt
2015020304.txt  2015020702.txt  2015021005.txt  2015021403.txt  2015021801.txt  2015022104.txt  2015022502.txt  2015022805.txt
2015020305.txt  2015020703.txt  2015021101.txt  2015021404.txt  2015021802.txt  2015022105.txt  2015022503.txt
2015020401.txt  2015020704.txt  2015021102.txt  2015021405.txt  2015021803.txt  2015022201.txt  2015022504.txt
2015020402.txt  2015020705.txt  2015021103.txt  2015021501.txt  2015021804.txt  2015022202.txt  2015022505.txt
2015020403.txt  2015020801.txt  2015021104.txt  2015021502.txt  2015021805.txt  2015022203.txt  2015022601.txt

2016/02:
2016020101.txt  2016020405.txt  2016020804.txt  2016021203.txt  2016021602.txt  2016022001.txt  2016022305.txt  2016022704.txt
2016020102.txt  2016020501.txt  2016020805.txt  2016021204.txt  2016021603.txt  2016022002.txt  2016022401.txt  2016022705.txt
2016020103.txt  2016020502.txt  2016020901.txt  2016021205.txt  2016021604.txt  2016022003.txt  2016022402.txt  2016022801.txt
2016020104.txt  2016020503.txt  2016020902.txt  2016021301.txt  2016021605.txt  2016022004.txt  2016022403.txt  2016022802.txt
2016020105.txt  2016020504.txt  2016020903.txt  2016021302.txt  2016021701.txt  2016022005.txt  2016022404.txt  2016022803.txt
2016020201.txt  2016020505.txt  2016020904.txt  2016021303.txt  2016021702.txt  2016022101.txt  2016022405.txt  2016022804.txt
2016020202.txt  2016020601.txt  2016020905.txt  2016021304.txt  2016021703.txt  2016022102.txt  2016022501.txt  2016022805.txt
2016020203.txt  2016020602.txt  2016021001.txt  2016021305.txt  2016021704.txt  2016022103.txt  2016022502.txt  2016022901.txt
2016020204.txt  2016020603.txt  2016021002.txt  2016021401.txt  2016021705.txt  2016022104.txt  2016022503.txt  2016022902.txt
2016020205.txt  2016020604.txt  2016021003.txt  2016021402.txt  2016021801.txt  2016022105.txt  2016022504.txt  2016022903.txt
2016020301.txt  2016020605.txt  2016021004.txt  2016021403.txt  2016021802.txt  2016022201.txt  2016022505.txt  2016022904.txt
2016020302.txt  2016020701.txt  2016021005.txt  2016021404.txt  2016021803.txt  2016022202.txt  2016022601.txt  2016022905.txt
2016020303.txt  2016020702.txt  2016021101.txt  2016021405.txt  2016021804.txt  2016022203.txt  2016022602.txt
2016020304.txt  2016020703.txt  2016021102.txt  2016021501.txt  2016021805.txt  2016022204.txt  2016022603.txt
2016020305.txt  2016020704.txt  2016021103.txt  2016021502.txt  2016021901.txt  2016022205.txt  2016022604.txt
2016020401.txt  2016020705.txt  2016021104.txt  2016021503.txt  2016021902.txt  2016022301.txt  2016022605.txt
2016020402.txt  2016020801.txt  2016021105.txt  2016021504.txt  2016021903.txt  2016022302.txt  2016022701.txt
2016020403.txt  2016020802.txt  2016021201.txt  2016021505.txt  2016021904.txt  2016022303.txt  2016022702.txt
2016020404.txt  2016020803.txt  2016021202.txt  2016021601.txt  2016021905.txt  2016022304.txt  2016022703.txt
cbpi@cbpi-VirtualBox:~/task8 $ ls 2015/02 | wc -l
140
cbpi@cbpi-VirtualBox:~/task8 $ ls 2016/02 | wc -l
145
cbpi@cbpi-VirtualBox:~/task8 $ cat 2016/02/2016022903.txt 
File 2016022903 text here
```

</details>

# **task 10:**
Полезное задание на конвейер. Использовать команду **cut** на вывод длинного списка
каталога, чтобы отобразить только права доступа к файлам. Затем отправить в конвейере
этот вывод на **sort** и **uniq**, чтобы отфильтровать все повторяющиеся строки. Потом с помощью
**wc** подсчитать различные типы разрешений в этом каталоге. Самостоятельно решить задачу,
как сделать так, чтобы в подсчет не добавлялись строка **Итого** и файлы **.** и **..** (ссылки на
текущую и родительскую директории)

Для примера возьмём каталог **/etc**.
Для исключения вывода файлов **.** и **..** (ссылки на текущую и родительскую директории)
используем опцию **-A** команды **ls** 
Для исключения вывода строки **Итого** используем **grep -v "^и"** (исключить строки, к-рые начинаются с **и**)

**В какталоге /etc всего 11 уникальных прав доступа к файлам**

```ShellSession
cbpi@cbpi-VirtualBox:~ $ ls -la /etc | head -5
итого 1136
drwxr-xr-x 131 root root      12288 дек 17 22:26 .
drwxr-xr-x  24 root root       4096 дек 14 17:21 ..
drwxr-xr-x   3 root root       4096 авг  5 22:04 acpi
-rw-r--r--   1 root root       3028 авг  5 21:58 adduser.conf
cbpi@cbpi-VirtualBox:~ $ ls -lA /etc | head -3
итого 1120
drwxr-xr-x  3 root root       4096 авг  5 22:04 acpi
-rw-r--r--  1 root root       3028 авг  5 21:58 adduser.conf
cbpi@cbpi-VirtualBox:~ $ ls -lA /etc | grep -v "^и" | head -3
drwxr-xr-x  3 root root       4096 авг  5 22:04 acpi
-rw-r--r--  1 root root       3028 авг  5 21:58 adduser.conf
drwxr-xr-x  2 root root       4096 дек 14 18:01 alternatives
cbpi@cbpi-VirtualBox:~ $ ls -lA /etc | grep -v "^и" | cut -c 1-10 | sort | uniq
drwxr-s---
drwxrwxr-x
drwxr-xr-x
lrwxrwxrwx
-r--r-----
-r--r--r--
-rw-------
-rw-r-----
-rw-r--r--
-rw-rw-r--
-rwxr-xr-x
cbpi@cbpi-VirtualBox:~ $ ls -lA /etc | grep -v "^и" | cut -c 1-10 | sort | uniq | wc -l
11
```

Попробуем вывести аналогичную информацию для каталога **/etc** рекурсивно (включая все подкаталоги), при этом исключив из вывода права доступа к директориям.
Для работы с подкаталогами используем опцию **-R** команды **ls**.
Для фильтра вывода используем **grep "^l\|^-"** - выбираем только строки, которые начинаются с
символов **l** и **-**.

**В какталоге /etc и всех его подкаталогах всего 8 уникальных прав доступа к файлам, исключая директории**

```ShellSession
cbpi@cbpi-VirtualBox:~ $ sudo sudo ls -lAR /etc | grep "^l\|^-" | cut -d" " -f1 | sort | uniq
lrwxrwxrwx
-r--r-----
-r--r--r--
-rw-------
-rw-r-----
-rw-r--r--
-rw-rw-r--
-rwxr-xr-x
cbpi@cbpi-VirtualBox:~ $ sudo sudo ls -lAR /etc | grep "^l\|^-" | cut -d" " -f1 | sort | uniq | wc -l
8
```

