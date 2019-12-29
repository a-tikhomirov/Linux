# **task 1,2:**
- Создать файл **file1** и наполнить его произвольным содержимым. Скопировать его в **file2**.
Создать символическую ссылку **file3** на **file1**. Создать жесткую ссылку **file4** на **file1**. Посмотреть,
какие айноды у файлов. Удалить **file1**. Что стало с остальными созданными файлами?
Попробовать вывести их на экран.

```ShellSession
cbpi@cbpi-VirtualBox:~/l3/t1 $ cat > file1
file1 text
cbpi@cbpi-VirtualBox:~/l3/t1 $ date >> file1
cbpi@cbpi-VirtualBox:~/l3/t1 $ cat file1
file1 text
Пт дек 27 20:15:32 MSK 2019
cbpi@cbpi-VirtualBox:~/l3/t1 $ cp file1 file2
cbpi@cbpi-VirtualBox:~/l3/t1 $ ln -s file1 file3
cbpi@cbpi-VirtualBox:~/l3/t1 $ find . file1 -type l
./file3
cbpi@cbpi-VirtualBox:~/l3/t1 $ ln file1 file4
cbpi@cbpi-VirtualBox:~/l3/t1 $ ls -li
итого 12
270729 -rw-r--r-- 2 cbpi cbpi 44 дек 27 20:15 file1
270732 -rw-r--r-- 1 cbpi cbpi 44 дек 27 20:15 file2
270734 lrwxrwxrwx 1 cbpi cbpi  5 дек 27 20:15 file3 -> file1
270729 -rw-r--r-- 2 cbpi cbpi 44 дек 27 20:15 file4
```

Из вывода команды **ls -li** можно видеть, что **file1** и **file4** имеют одинаковый индексный дескриптор **inode** т.к. **file4** создавался как жесткая ссылка на **file1** Т.е. оба файла (а точнее два различных объекта **dentry**) содержат указатель на один и тот же **inode**. В третьем столбце можно видеть количество жестких ссылок: **2**.

**file2** содержит копию содержимого **file1**, но имеет другой индексный дескриптор **inode**, т.е. объект **dentry** с именем **file2** содержит другой **inode** несмотря на одинаковое содержимое файла.

**file3** как символическая ссылка имеет отличный от **file1** индкесный декскриптор **inode** т.к. содержит указатель на файл **file1** (т.е. у объекта **inode** для символической ссылки, в поле **file-content** содержится путь до файла **file1**, а не содержимое файла). Символическая ссылка имеет обозначение типа файла **l** и размер файла соотвествует размеру пути до файла, на которое ссылается (в данном случае путь относительный: только имя файла **file1** - 5 байт)

```ShellSession
cbpi@cbpi-VirtualBox:~/l3/t1 $ rm file1
cbpi@cbpi-VirtualBox:~/l3/t1 $ ls -li
итого 8
270732 -rw-r--r-- 1 cbpi cbpi 44 дек 27 20:15 file2
270734 lrwxrwxrwx 1 cbpi cbpi  5 дек 27 20:15 file3 -> file1
270729 -rw-r--r-- 1 cbpi cbpi 44 дек 27 20:15 file4
cbpi@cbpi-VirtualBox:~/l3/t1 $ cat file2
file1 text
Пт дек 27 20:15:32 MSK 2019
cbpi@cbpi-VirtualBox:~/l3/t1 $ cat file3
cat: file3: Нет такого файла или каталога
cbpi@cbpi-VirtualBox:~/l3/t1 $ cat file4
file1 text
Пт дек 27 20:15:32 MSK 2019
cbpi@cbpi-VirtualBox:~/l3/t1 $ ls -liL 2> /dev/null
итого 8
270732 -rw-r--r-- 1 cbpi cbpi 44 дек 27 20:15 file2
     ? l????????? ? ?    ?     ?            ? file3
270729 -rw-r--r-- 1 cbpi cbpi 44 дек 27 20:15 file4
```

После удаления файла **file1** количество жестких ссылок на объект файловой системы с **inode = 270729** уменьшилось на 1 (**2 -> 1**), а именно - остался один объект **dentry** с указателем на **inode**: **file4**

С файлом **file2** после удаления **file1** ничего не произошло, т.к. объект **dentry** с именем **file2** имеет указатель на другой **inode**.

Символическая ссылка **file3** стала ссылаться на несуществующий файл.

- Дать созданным файлам другие, произвольные имена. Создать новую символическую ссылку.
Переместить ссылки в другую директорию.

```ShellSession
cbpi@cbpi-VirtualBox:~/l3/t1 $ rm file3
cbpi@cbpi-VirtualBox:~/l3/t1 $ ls -li
итого 8
270732 -rw-r--r-- 1 cbpi cbpi 44 дек 27 20:15 file2
270729 -rw-r--r-- 1 cbpi cbpi 44 дек 27 20:15 file4
cbpi@cbpi-VirtualBox:~/l3/t1 $ mv file2 renamedfile2
cbpi@cbpi-VirtualBox:~/l3/t1 $ mv file4 renamedFile4
cbpi@cbpi-VirtualBox:~/l3/t1 $ ln -s renamedFile2 symLinkRelative
cbpi@cbpi-VirtualBox:~/l3/t1 $ ln -s $(pwd)/renamedFile2 symLinkAbsolute
cbpi@cbpi-VirtualBox:~/l3/t1 $ ls -li
итого 8
270732 -rw-r--r-- 1 cbpi cbpi 44 дек 27 20:15 renamedFile2
270729 -rw-r--r-- 1 cbpi cbpi 44 дек 27 20:15 renamedFile4
270781 lrwxrwxrwx 1 cbpi cbpi 29 дек 27 20:33 symLinkAbsolute -> /home/cbpi/l3/t1/renamedFile2
270734 lrwxrwxrwx 1 cbpi cbpi 12 дек 27 20:32 symLinkRelative -> renamedFile2
cbpi@cbpi-VirtualBox:~/l3/t1 $ ls -liL
итого 16
270732 -rw-r--r-- 1 cbpi cbpi 44 дек 27 20:15 renamedFile2
270729 -rw-r--r-- 1 cbpi cbpi 44 дек 27 20:15 renamedFile4
270732 -rw-r--r-- 1 cbpi cbpi 44 дек 27 20:15 symLinkAbsolute
270732 -rw-r--r-- 1 cbpi cbpi 44 дек 27 20:15 symLinkRelative
cbpi@cbpi-VirtualBox:~/l3/t1 $
cbpi@cbpi-VirtualBox:~/l3/t1 $
cbpi@cbpi-VirtualBox:~/l3/t1 $
cbpi@cbpi-VirtualBox:~/l3/t1 $ mkdir breakSomeLinks
cbpi@cbpi-VirtualBox:~/l3/t1 $ mv sym* breakSomeLinks/
cbpi@cbpi-VirtualBox:~/l3/t1 $ ls -li breakSomeLinks/
итого 0
270781 lrwxrwxrwx 1 cbpi cbpi 29 дек 27 20:33 symLinkAbsolute -> /home/cbpi/l3/t1/renamedFile2
270734 lrwxrwxrwx 1 cbpi cbpi 12 дек 27 20:32 symLinkRelative -> renamedFile2
cbpi@cbpi-VirtualBox:~/l3/t1 $ ls -liL breakSomeLinks/
ls: невозможно получить доступ к 'breakSomeLinks/symLinkRelative': Нет такого файла или каталога
итого 4
270732 -rw-r--r-- 1 cbpi cbpi 44 дек 27 20:15 symLinkAbsolute
     ? l????????? ? ?    ?     ?            ? symLinkRelative
```

После перемещения символических ссылок в другую директорию рабочей осталась только ссылка, созданная с указанием абсолютного пути к файлу.

```ShellSession
cbpi@cbpi-VirtualBox:~/l3/t1 $ mv renamedFile2 breakSomeLinks/
cbpi@cbpi-VirtualBox:~/l3/t1 $ ls -li breakSomeLinks/
итого 4
270732 -rw-r--r-- 1 cbpi cbpi 44 дек 27 20:15 renamedFile2
270781 lrwxrwxrwx 1 cbpi cbpi 29 дек 27 20:33 symLinkAbsolute -> /home/cbpi/l3/t1/renamedFile2
270734 lrwxrwxrwx 1 cbpi cbpi 12 дек 27 20:32 symLinkRelative -> renamedFile2
cbpi@cbpi-VirtualBox:~/l3/t1 $ ls -liL breakSomeLinks/
ls: невозможно получить доступ к 'breakSomeLinks/symLinkAbsolute': Нет такого файла или каталога
итого 8
270732 -rw-r--r-- 1 cbpi cbpi 44 дек 27 20:15 renamedFile2
     ? l????????? ? ?    ?     ?            ? symLinkAbsolute
270732 -rw-r--r-- 1 cbpi cbpi 44 дек 27 20:15 symLinkRelative
```

Если переместить в ту же директорию файл на которые ссылаются символические ссылки, то символическая ссылка созданная с указанием относительно пути снова будет работать (т.к. и сам файл и символическая ссылка на него снова окажутся в одной директории), а вот символическая ссылка созданная с указанием абсолютного пути перестанет работать (т.к. файл, на который она ссылалсь был перемещен).

# **task 3:**
Создать два произвольных файла. Первому присвоить права на чтение, запись для
владельца и группы, только на чтение для всех. Второму присвоить права на чтение, запись
только для владельца. Сделать это в численном и символьном виде.

- Вариант 1

```ShellSession
cbpi@cbpi-VirtualBox:~/l3/t3 $ touch file1 file2
cbpi@cbpi-VirtualBox:~/l3/t3 $ chmod 664 file1
cbpi@cbpi-VirtualBox:~/l3/t3 $ chmod 644 file2
cbpi@cbpi-VirtualBox:~/l3/t3 $ ls -l
итого 0
-rw-rw-r-- 1 cbpi cbpi 0 дек 27 20:50 file1
-rw-r--r-- 1 cbpi cbpi 0 дек 27 20:50 file2
```

- Вариант 2

```ShellSession
cbpi@cbpi-VirtualBox:~/l3/t3 $ rm f*
cbpi@cbpi-VirtualBox:~/l3/t3 $ touch file1 file2
cbpi@cbpi-VirtualBox:~/l3/t3 $ chmod ug=rw,o=r file1
cbpi@cbpi-VirtualBox:~/l3/t3 $ chmod u=rw,go=r file2
cbpi@cbpi-VirtualBox:~/l3/t3 $ ls -l
итого 0
-rw-rw-r-- 1 cbpi cbpi 0 дек 27 20:51 file1
-rw-r--r-- 1 cbpi cbpi 0 дек 27 20:51 file2
```

- Вариант 3

```ShellSession
cbpi@cbpi-VirtualBox:~/l3/t3 $ rm f*
cbpi@cbpi-VirtualBox:~/l3/t3 $ touch file1 file2
cbpi@cbpi-VirtualBox:~/l3/t3 $ ls -l
итого 0
-rw-r--r-- 1 cbpi cbpi 0 дек 27 20:53 file1
-rw-r--r-- 1 cbpi cbpi 0 дек 27 20:53 file2
cbpi@cbpi-VirtualBox:~/l3/t3 $ chmod g+w file1
cbpi@cbpi-VirtualBox:~/l3/t3 $ ls -l
итого 0
-rw-rw-r-- 1 cbpi cbpi 0 дек 27 20:53 file1
-rw-r--r-- 1 cbpi cbpi 0 дек 27 20:53 file2
```

- Вариант 4

На практике, для такого задания - вряд ли имеет смысл. Просто демонстрация того, что общие (**user,group,others**) права можно выставить через **ACL**:

```ShellSession
cbpi@cbpi-VirtualBox:~/l3/t3 $ rm f*
cbpi@cbpi-VirtualBox:~/l3/t3 $ touch file1 file2
cbpi@cbpi-VirtualBox:~/l3/t3 $ chmod a-rwx f*
cbpi@cbpi-VirtualBox:~/l3/t3 $ ls -l
итого 8
---------- 1 cbpi cbpi 11 дек 29 22:49 file1
---------- 1 cbpi cbpi 11 дек 29 22:49 file2
cbpi@cbpi-VirtualBox:~/l3/t3 $ setfacl -m u::rw f*
cbpi@cbpi-VirtualBox:~/l3/t3 $ setfacl -m o::r f*
cbpi@cbpi-VirtualBox:~/l3/t3 $ setfacl -m g::rw file1
cbpi@cbpi-VirtualBox:~/l3/t3 $ setfacl -m g::r file2
cbpi@cbpi-VirtualBox:~/l3/t3 $ getfacl f*
# file: file1
# owner: cbpi
# group: cbpi
user::rw-
group::rw-
other::r--

# file: file2
# owner: cbpi
# group: cbpi
user::rw-
group::r--
other::r--

cbpi@cbpi-VirtualBox:~/l3/t3 $ ls -l
итого 8
-rw-rw-r-- 1 cbpi cbpi 11 дек 29 22:49 file1
-rw-r--r-- 1 cbpi cbpi 11 дек 29 22:49 file2
```

# **task 4:**
Создать пользователя, обладающего возможностью выполнять действия от имени суперпользователя.

- Вариант 1

```ShellSession
cbpi@cbpi-VirtualBox:~ $ sudo adduser suable
[sudo] пароль для cbpi: 
Добавляется пользователь «suable» ...
Добавляется новая группа «suable» (1001) ...
Добавляется новый пользователь «suable» (1001) в группу «suable» ...
Создаётся домашний каталог «/home/suable» ...
Копирование файлов из «/etc/skel» ...
Введите новый пароль UNIX: 
Повторите ввод нового пароля UNIX: 
passwd: пароль успешно обновлён
Изменение информации о пользователе suable
Введите новое значение или нажмите ENTER для выбора значения по умолчанию
	Полное имя []: 
	Номер комнаты []: 
	Рабочий телефон []: 
	Домашний телефон []: 
	Другое []: 
Данная информация корректна? [Y/n] y
bpi@cbpi-VirtualBox:~ $ su suable 
Пароль: 
suable@cbpi-VirtualBox:/home/cbpi$ cd ~
suable@cbpi-VirtualBox:~$ sudo ls -A /root
[sudo] пароль для suable: 
suable отсутствует в файле sudoers. Данное действие будет занесено в журнал.
```

При создании нового пользователя он по умолчанию не добавляется в группу **sudo**, необходимо добавить пользователя в эту группу, чтобы он мог выполнять команды от имени суперпользователя.

```ShellSession
suable@cbpi-VirtualBox:~$ su cbpi
Пароль: 
cbpi@cbpi-VirtualBox:/home/suable $ grep "sudo\|suable" /etc/group
sudo:x:27:cbpi
suable:x:1001:
cbpi@cbpi-VirtualBox:/home/suable $ sudo usermod -aG sudo suable
[sudo] пароль для cbpi: 
cbpi@cbpi-VirtualBox:/home/suable $ grep "sudo\|suable" /etc/group
sudo:x:27:cbpi,suable
suable:x:1001:
cbpi@cbpi-VirtualBox:/home/suable $ su - suable 
Пароль: 
To run a command as administrator (user "root"), use "sudo <command>".
See "man sudo_root" for details.
suable@cbpi-VirtualBox:~$ sudo ls -A /root
[sudo] пароль для suable: 
.bash_history  .bashrc	.cache	.config  .gnupg  .hushlogin  .local  .profile
```

- Вариант 2

При создании пользователя указать дополнительную группу с помощью ключа **-G**

```ShellSession
cbpi@cbpi-VirtualBox:~ $ sudo useradd -G sudo suable
cbpi@cbpi-VirtualBox:~ $ sudo passwd suable
Введите новый пароль UNIX: 
Повторите ввод нового пароля UNIX: 
passwd: пароль успешно обновлён
cbpi@cbpi-VirtualBox:~ $ cat /etc/group | grep "suable\|sudo"
sudo:x:27:cbpi,suable
suable:x:1001:
cbpi@cbpi-VirtualBox:~ $ su - suable 
Пароль: 
Каталог отсутствует или недоступен, вход в систему выполняется с HOME=/
$ sudo ls -A /root
[sudo] пароль для suable: 
.bash_history  .bashrc	.cache	.config  .gnupg  .hushlogin  .local  .profile  .ssh
```

При авторизации как пользователь **suable** мы оказались не в его домашнем каталоге, т.к. таковой не был создан при создании пользователя (не использовался ключ **-m**)

- Вариант 3

Добавить конфиг для пользователя **suable** в **/etc/sudoers.d/**

```ShellSession
root@cbpi-VirtualBox:~# useradd -m -s /bin/bash suable
root@cbpi-VirtualBox:~# passwd suable
Введите новый пароль UNIX: 
Повторите ввод нового пароля UNIX: 
passwd: пароль успешно обновлён
root@cbpi-VirtualBox:~# cat > /etc/sudoers.d/suable
suable ALL=ALL
root@cbpi-VirtualBox:~# su - suable
suable@cbpi-VirtualBox:~$ sudo ls -A /root
[sudo] пароль для suable: 
.bash_history  .bashrc	.cache	.config  .gnupg  .hushlogin  .local  .profile  .ssh  .viminfo
suable@cbpi-VirtualBox:~$ grep su.* /etc/group
sudo:x:27:cbpi
suable:x:1005:
```

# **task 5:**
Создать группу **developer**, несколько пользователей, входящих в эту группу. Создать
директорию для совместной работы. Сделать так, чтобы созданные одними пользователями
файлы могли изменять другие пользователи этой группы.

- Создание группы и добавление пользователей:

```ShellSession
root@cbpi-VirtualBox:~# groupadd developer
root@cbpi-VirtualBox:~# useradd -m -G developer -s /bin/bash dev1
root@cbpi-VirtualBox:~# useradd -m -G developer -s /bin/bash dev2
root@cbpi-VirtualBox:~# useradd -m -G developer -s /bin/bash dev3
root@cbpi-VirtualBox:~# passwd dev1
Введите новый пароль UNIX: 
Повторите ввод нового пароля UNIX: 
passwd: пароль успешно обновлён
root@cbpi-VirtualBox:~# passwd dev2
Введите новый пароль UNIX: 
Повторите ввод нового пароля UNIX: 
passwd: пароль успешно обновлён
root@cbpi-VirtualBox:~# passwd dev3
Введите новый пароль UNIX: 
Повторите ввод нового пароля UNIX: 
passwd: пароль успешно обновлён
root@cbpi-VirtualBox:~# cat /etc/group | grep "dev\w"
developer:x:1001:dev1,dev2,dev3
dev1:x:1002:
dev2:x:1003:
dev3:x:1004:
```

- Создание директории и установка прав доступа. Для обеспечения совместной работы в директории назначим для нее группу и используем бит **SGID** или **g+s** или **2000**:

```ShellSession
root@cbpi-VirtualBox:~# mkdir /developer
root@cbpi-VirtualBox:~# chgrp developer /developer
root@cbpi-VirtualBox:~# chmod 2775 /developer
root@cbpi-VirtualBox:~# ls -ld /developer
drwxrwsr-x 2 root developer 4096 дек 27 21:48 /developer
```

- Проверка:

Пользователь **dev1**:

```ShellSession
root@cbpi-VirtualBbpi@cbpi-VirtualBox:~ $ su - dev1
Пароль: 
dev1@cbpi-VirtualBox:~$ cd /developers/
dev1@cbpi-VirtualBox:/developers$ echo dev1 file > dev1file
```

Пользователь **dev2**:

```ShellSession
cbpi@cbpi-VirtualBox:~ $ su - dev2
Пароль: 
dev2@cbpi-VirtualBox:~$ cd /developer/
dev2@cbpi-VirtualBox:/developer$ ls -l
итого 4
-rw-rw-r-- 1 dev1 developer 10 дек 27 21:50 dev1file
dev2@cbpi-VirtualBox:/developer$ echo more info from dev2 >> dev1file
dev2@cbpi-VirtualBox:/developer$ cat dev1file 
dev1 file
more info from dev2
```

Пользователь не из группы **developer**:

```ShellSession
cbpi@cbpi-VirtualBox:~ $ cd /developer/
cbpi@cbpi-VirtualBox:/developer $ ls -l
итого 4
-rw-rw-r-- 1 dev1 developer 30 дек 27 21:51 dev1file
cbpi@cbpi-VirtualBox:~ $ touch /developer/cbpiFile
touch: невозможно выполнить touch для '/developer/cbpiFile': Отказано в доступе
cbpi@cbpi-VirtualBox:/developer $ echo some more info >> dev1file 
bash: dev1file: Отказано в доступе
cbpi@cbpi-VirtualBox:/developer $ cat dev1file 
dev1 file
more info from dev2
```

# **task 6:**
Создать в директории для совместной работы поддиректорию для обмена файлами, но
чтобы удалять файлы могли только их создатели.

Группа, пользователи и директория для совместной работы созданы в **task 5**

- Создание поддиректории для обмена файлами. Для обеспечения возможности удаления файла только создателем используем **sticky bit** или **+t** или **1000**:

```ShellSession
root@cbpi-VirtualBox:~# cd /developer
root@cbpi-VirtualBox:/developer# mkdir shared
root@cbpi-VirtualBox:/developer# ls -l | grep ^d
drwxr-sr-x 2 root developer 4096 дек 27 21:53 shared
root@cbpi-VirtualBox:/developer# chmod g+w,+t shared/
root@cbpi-VirtualBox:/developer# ls -ld shared/
drwxrwsr-t 2 root developer 4096 дек 27 21:53 shared/
```

- Проверка:

Пользователь **dev2**:

```ShellSession
cbpi@cbpi-VirtualBox:~ $ su - dev2
Пароль: 
dev2@cbpi-VirtualBox:~$ cd /developer/shared/
dev2@cbpi-VirtualBox:/developer/shared$ echo dev2 file > dev2file
dev2@cbpi-VirtualBox:/developer/shared$ ls -l
итого 4
-rw-rw-r-- 1 dev2 developer 10 дек 27 21:55 dev2file
```

Пользователь **dev3**:

```ShellSession
cbpi@cbpi-VirtualBox:~ $ su - dev3
Пароль: 
dev3@cbpi-VirtualBox:~$ cd /developer/shared/
dev3@cbpi-VirtualBox:/developer/shared$ ls -l
итого 4
-rw-rw-r-- 1 dev2 developer 10 дек 27 21:55 dev2file
dev3@cbpi-VirtualBox:/developer/shared$ rm dev2file
rm: невозможно удалить 'dev2file': Операция не позволена
dev3@cbpi-VirtualBox:/developer/shared$ echo dev3 file > dev3file
```

Пользователь **dev2**:

```ShellSession
dev2@cbpi-VirtualBox:/developer/shared$ ls -l
итого 8
-rw-rw-r-- 1 dev2 developer 10 дек 27 21:55 dev2file
-rw-rw-r-- 1 dev3 developer 10 дек 27 21:57 dev3file
dev2@cbpi-VirtualBox:/developer/shared$ rm dev3file 
rm: невозможно удалить 'dev3file': Операция не позволена
dev2@cbpi-VirtualBox:/developer/shared$ rm dev2file 
dev2@cbpi-VirtualBox:/developer/shared$ ls -l
итого 4
-rw-rw-r-- 1 dev3 developer 10 дек 27 21:57 dev3file
```

# **task 7:**
Создать директорию, в которой есть несколько файлов. Сделать так, чтобы открыть файлы
можно только, зная имя файла, а через **ls** список файлов посмотреть нельзя.

Единственный вариант решения задачи, который я нашел - это убрать разрешение на чтение для директории.

```ShellSession
root@cbpi-VirtualBox:~# cd /developer
root@cbpi-VirtualBox:/developer# mkdir hiddenFiles
root@cbpi-VirtualBox:/developer# ls -ld hiddenFiles/
drwxr-sr-x 2 root developer 4096 дек 27 22:02 hiddenFiles/
root@cbpi-VirtualBox:/developer# chmod 3731 hiddenFiles/
root@cbpi-VirtualBox:/developer# ls -ld hiddenFiles/
drwx-ws--t 2 root developer 4096 дек 27 22:02 hiddenFiles/
```

Оставим возможность просматривать содержимое директории для пользователя **root** =)
Если необходимо "скрыть" содержимое вообще от всех, то нужно назначить права: **chmod a-r hiddenFiles/**

- Проверка:

Пользователь **dev1**:

```ShellSession
cbpi@cbpi-VirtualBox:~ $ su - dev1
Пароль: 
dev1@cbpi-VirtualBox:~$ cd /developer/hiddenFiles/
dev1@cbpi-VirtualBox:/developer/hiddenFiles$ ls
ls: невозможно открыть каталог '.': Отказано в доступе
dev1@cbpi-VirtualBox:/developer/hiddenFiles$ echo dev1 file > dev1file
dev1@cbpi-VirtualBox:/developer/hiddenFiles$ cat dev1file
dev1 file
```

Пользователь **dev2**:

```ShellSession
cbpi@cbpi-VirtualBox:~ $ su - dev2
Пароль: 
dev2@cbpi-VirtualBox:~$ cd /developer/hiddenFiles/
dev2@cbpi-VirtualBox:/developer/hiddenFiles$ ls -la
ls: невозможно открыть каталог '.': Отказано в доступе
dev2@cbpi-VirtualBox:/developer/hiddenFiles$ cat dev1file
dev1 file
dev2@cbpi-VirtualBox:/developer/hiddenFiles$ echo dev2 file > dev2file
dev2@cbpi-VirtualBox:/developer/hiddenFiles$ cat dev2file
dev2 file
```

Пользователи не из группы **developer**:

```ShellSession
cbpi@cbpi-VirtualBox:~ $ ls -l /developer/
итого 12
-rw-rw-r-- 1 dev1 developer   30 дек 27 21:51 dev1file
drwx-ws--t 2 root developer 4096 дек 27 22:08 hiddenFiles
drwxrwsr-t 2 root developer 4096 дек 27 21:57 shared
cbpi@cbpi-VirtualBox:~ $ ls -l /developer/hiddenFiles/dev*
ls: невозможно получить доступ к '/developer/hiddenFiles/dev*': Нет такого файла или каталога
cbpi@cbpi-VirtualBox:~ $ ls -l /developer/hiddenFiles
ls: невозможно открыть каталог '/developer/hiddenFiles': Отказано в доступе
cbpi@cbpi-VirtualBox:~ $ cat /developer/hiddenFiles/dev*
cat: '/developer/hiddenFiles/dev*': Нет такого файла или каталога
cbpi@cbpi-VirtualBox:~ $ cat /developer/hiddenFiles/dev1file
dev1 file
cbpi@cbpi-VirtualBox:~ $ cat /developer/hiddenFiles/dev{1..2}file
dev1 file
dev2 file
cbpi@cbpi-VirtualBox:~ $ su
Пароль: 
root@cbpi-VirtualBox:/home/cbpi# ls -l /developer/hiddenFiles/
итого 8
-rw-rw-r-- 1 dev1 developer 10 дек 27 22:07 dev1file
-rw-rw-r-- 1 dev2 developer 10 дек 27 22:08 dev2file
root@cbpi-VirtualBox:/home/cbpi# cat /developer/hiddenFiles/dev*
dev1 file
dev2 file
```

# **Дополнительно. ACL**

Предположим, что у нас есть группа **cbpi** в которой есть пользователи, которым также необходимо предоставить полный доступ к общей папке группы **developer** (например группа тим-лидов/проект-менеджеров и т.п.). Использование обычных прав доступа в этом случае не всегда удобно/корректно - придется всех пользователей группы **cbpi** добавить в группу **developer**. Предполагаю, что такой вариант не очень хорош с точки зрения структуры групп и пользователей. В этом случае на помощь приходит **ACL**:

```ShellSession
root@cbpi-VirtualBox:~# setfacl -R -m d:g:cbpi:rwx,g:cbpi:rwx /developer
root@cbpi-VirtualBox:~# getfacl /developer/
getfacl: Удаление начальных '/' из абсолютных путей
# file: developer/
# owner: root
# group: developer
# flags: -s-
user::rwx
group::rwx
group:cbpi:rwx
mask::rwx
other::r-x
default:user::rwx
default:group::rwx
default:group:cbpi:rwx
default:mask::rwx
default:other::r-x

root@cbpi-VirtualBox:~# exit
exit
cbpi@cbpi-VirtualBox:~ $ cd /developer/
cbpi@cbpi-VirtualBox:/developer $ ls
dev1file  hiddenFiles  shared
cbpi@cbpi-VirtualBox:/developer $ echo comment from cbpi >> dev1file
cbpi@cbpi-VirtualBox:/developer $ cat dev1file 
dev1 file
more info from dev2
comment from cbpi
cbpi@cbpi-VirtualBox:/developer $ su - dev3
Пароль: 
dev3@cbpi-VirtualBox:~$ cd /developer/
dev3@cbpi-VirtualBox:/developer$ echo dev3 file > dev3file
dev3@cbpi-VirtualBox:/developer$ exit
выход
cbpi@cbpi-VirtualBox:/developer $ echo comment for dev3 >> dev3file
cbpi@cbpi-VirtualBox:/developer $ cat dev3file 
dev3 file
comment for dev3
```

