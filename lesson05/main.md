# **task 1a:**
Написать скрипт, который удаляет из текстового файла пустые строки и заменяет маленькие символы на большие (воспользуйтесь tr или sed).

Для удаления пустых строк и замены маленьких символов на большие используем `sed`: 

```ShellSession
sed '/^$/d; s/\(.*\)/\U\1/' file
```

- Вариант 1:

Создание скрипта:

```ShellSession
cbpi@vault_rpi:~/geekbrains/linux/l5$ vim simple_text_rework.sh
...
cbpi@vault_rpi:~/geekbrains/linux/l5$ chmod +x simple_text_rework.sh
cbpi@vault_rpi:~/geekbrains/linux/l5$ ln -s $(pwd)/simple_text_rework.sh ~/bin/simple_text_rework
```

Скрипт:

```Shell
#!/bin/bash

PATTERN="/^$/d; s/\(.*\)/\U\1/"

sed "$PATTERN" $@
```

<details>
<summary>Проверка</summary>

```ShellSession
cbpi@vault_rpi:~/geekbrains/linux/l5$ cat file1
sTaRt of file1

Text for script1

Some more text

Even more text

end OF FILE1
cbpi@vault_rpi:~/geekbrains/linux/l5$ cat file2
sTaRt of file2

Text for script1

Some more text

Even more text

end OF FILE2
cbpi@vault_rpi:~/geekbrains/linux/l5$ cat file3
sTaRt of file3

Text for script1

Some more text

Even more text

end OF FILE3
```

```ShellSession
cbpi@vault_rpi:~/geekbrains/linux/l5$ simple_text_rework file1
START OF FILE1
TEXT FOR SCRIPT1
SOME MORE TEXT
EVEN MORE TEXT
END OF FILE1
```

```ShellSession
cbpi@vault_rpi:~/geekbrains/linux/l5$ simple_text_rework file*
START OF FILE1
TEXT FOR SCRIPT1
SOME MORE TEXT
EVEN MORE TEXT
END OF FILE1
START OF FILE2
TEXT FOR SCRIPT1
SOME MORE TEXT
EVEN MORE TEXT
END OF FILE2
START OF FILE3
TEXT FOR SCRIPT1
SOME MORE TEXT
EVEN MORE TEXT
END OF FILE3
```

```ShellSession
cbpi@vault_rpi:~/geekbrains/linux/l5$ simple_text_rework file4
sed: can't read file4: No such file or directory
```

> окончание проверки
> ---
</details>

- Вариант 2:

Создание скрипта:

```ShellSession
cbpi@vault_rpi:~/geekbrains/linux/l5$ vim text_rework.sh
...
cbpi@vault_rpi:~/geekbrains/linux/l5$ chmod +x text_rework.sh
cbpi@vault_rpi:~/geekbrains/linux/l5$ ln -s $(pwd)/text_rework.sh ~/bin/text_rework
```

<details>
<summary>Скрипт</summary>

```Shell
#!/bin/bash

ARG1_ERR="Not enough arguments"
ARG2_ERR="No specified files to work with"
PATH_ERR="Can't find path or write to dir: "
F_ERR="Can't find or read file: "

SED_PATTERN="/^$/d; s/\(.*\)/\U\1/"
EXIT_CODE=0

usage(){
    cat <<-EOF
    This script removes empty lines, changes all lowercase letters to uppercase and prints the result to stdout.

    With the option '-o ouput_path' result files with the prefix 'out_'
    will be written to 'output_path' directiry.

    Usage: $0 [file(s)] [-o output path] [--help]
    Options:
        - path to files
        - path to output directory
    Examples:
        $0 --help
        $0 ~/.bashrc i -o .
EOF
}

print_err(){
    >&2 echo $1
    [[ $2 -eq 0 ]] && usage
    [[ $# -gt 2 ]] && exit $3
}

sed_file(){
    if [[ -z "$OUT_PATH"  ]]; then
        [[ ${#FILES[@]} > 1 ]] && echo "$1:"
        sed "$SED_PATTERN" $1
    else
        FILENAME=out_"$(basename -- $1)"
        echo -e "$1 -> $OUT_PATH/$FILENAME ... DONE"
        sed "$SED_PATTERN" $1 > $OUT_PATH/$FILENAME
    fi
}

[[ $# == 0 ]] && print_err "$ARG1_ERR" 0 1

while [[ $# -gt 0 ]]; do
    case $1 in
    --help)
      usage; exit 0
    ;;
    -o)
      shift;
      [[ $# == 0 ]] && print_err "$ARG1_ERR" 0 2
      OUT_PATH=$1; shift
    ;;
    *)
      FILES+=($1); shift
    ;;
    esac
done

[[ ${#FILES[@]} == 0 ]] && print_err "$ARG2_ERR" 0 3
test ! -z "$OUT_PATH" && { [[ -d $OUT_PATH && -w $OUT_PATH ]] ||
print_err "$PATH_ERR$OUT_PATH" 1 4; }

for file in "${FILES[@]}"; do
    { test -h $file || test -f $file; } && test -r $file
    [[ $? == 0 ]] && sed_file $file || { print_err "$F_ERR$file" 1; EXIT_CODE=5; }
done

exit $EXIT_CODE
```

> окончание скрипта
> ---
</details>

<details>
<summary>Проверка</summary>

```ShellSession
cbpi@vault_rpi:~/geekbrains/linux/l5$ cat file1
sTaRt of file1

Text for script1

Some more text

Even more text

end OF FILE1
cbpi@vault_rpi:~/geekbrains/linux/l5$ cat file2
sTaRt of file2

Text for script1

Some more text

Even more text

end OF FILE2
cbpi@vault_rpi:~/geekbrains/linux/l5$ cat file3
sTaRt of file3

Text for script1

Some more text

Even more text

end OF FILE3
```

```ShellSession
cbpi@vault_rpi:~/geekbrains/linux/l5$ text_rework file1
START OF FILE1
TEXT FOR SCRIPT1
SOME MORE TEXT
EVEN MORE TEXT
END OF FILE1
```

```ShellSession
cbpi@vault_rpi:~/geekbrains/linux/l5$ text_rework file{1..2}
file1:
START OF FILE1
TEXT FOR SCRIPT1
SOME MORE TEXT
EVEN MORE TEXT
END OF FILE1
file2:
START OF FILE2
TEXT FOR SCRIPT1
SOME MORE TEXT
EVEN MORE TEXT
END OF FILE2
```

```ShellSession
cbpi@vault_rpi:~/geekbrains/linux/l5$ text_rework file*
file1:
START OF FILE1
TEXT FOR SCRIPT1
SOME MORE TEXT
EVEN MORE TEXT
END OF FILE1
file2:
START OF FILE2
TEXT FOR SCRIPT1
SOME MORE TEXT
EVEN MORE TEXT
END OF FILE2
file3:
START OF FILE3
TEXT FOR SCRIPT1
SOME MORE TEXT
EVEN MORE TEXT
END OF FILE3
```

```ShellSession
cbpi@vault_rpi:~/geekbrains/linux/l5$ mkdir out
cbpi@vault_rpi:~/geekbrains/linux/l5$ text_rework file* -o out
file1 -> out/out_file1 ... DONE
file2 -> out/out_file2 ... DONE
file3 -> out/out_file3 ... DONE
cbpi@vault_rpi:~/geekbrains/linux/l5$ ls -lA out
total 12
-rw-r--r-- 1 cbpi cbpi 75 Jan 22 18:27 out_file1
-rw-r--r-- 1 cbpi cbpi 75 Jan 22 18:27 out_file2
-rw-r--r-- 1 cbpi cbpi 75 Jan 22 18:27 out_file3
cbpi@vault_rpi:~/geekbrains/linux/l5$ cat out/*
START OF FILE1
TEXT FOR SCRIPT1
SOME MORE TEXT
EVEN MORE TEXT
END OF FILE1
START OF FILE2
TEXT FOR SCRIPT1
SOME MORE TEXT
EVEN MORE TEXT
END OF FILE2
START OF FILE3
TEXT FOR SCRIPT1
SOME MORE TEXT
EVEN MORE TEXT
END OF FILE3
```

```ShellSession
cbpi@vault_rpi:~/geekbrains/linux/l5$ text_rework
Not enough arguments
    This script removes empty lines, changes all lowercase letters to uppercase and prints the result to stdout.

    With the option '-o ouput_path' result files with the prefix 'out_'
    will be written to 'output_path' directiry.

    Usage: /home/cbpi/bin/text_rework [file(s)] [-o output path]
    Options:
        - path to files
        - path to output directory
    Examples:
        /home/cbpi/bin/text_rework --help
        /home/cbpi/bin/text_rework ~/.bashrc i -o .
cbpi@vault_rpi:~/geekbrains/linux/l5$ echo $?
1
cbpi@vault_rpi:~/geekbrains/linux/l5$ text_rework --help
    This script removes empty lines, changes all lowercase letters to uppercase and prints the result to stdout.

    With the option '-o ouput_path' result files with the prefix 'out_'
    will be written to 'output_path' directiry.

    Usage: /home/cbpi/bin/text_rework [file(s)] [-o output path]
    Options:
        - path to files
        - path to output directory
    Examples:
        /home/cbpi/bin/text_rework --help
        /home/cbpi/bin/text_rework ~/.bashrc i -o .
```

```ShellSession
cbpi@vault_rpi:~/geekbrains/linux/l5$ text_rework file1 -o
Not enough arguments
    This script removes empty lines, changes all lowercase letters to uppercase and prints the result to stdout.

    With the option '-o ouput_path' result files with the prefix 'out_'
    will be written to 'output_path' directiry.

    Usage: /home/cbpi/bin/text_rework [file(s)] [-o output path]
    Options:
        - path to files
        - path to output directory
    Examples:
        /home/cbpi/bin/text_rework --help
        /home/cbpi/bin/text_rework ~/.bashrc i -o .
cbpi@vault_rpi:~/geekbrains/linux/l5$ echo $?
2
```

```ShellSession
cbpi@vault_rpi:~/geekbrains/linux/l5$ text_rework -o out
No specified files to work with
    This script removes empty lines, changes all lowercase letters to uppercase and prints the result to stdout.

    With the option '-o ouput_path' result files with the prefix 'out_'
    will be written to 'output_path' directiry.

    Usage: /home/cbpi/bin/text_rework [file(s)] [-o output path]
    Options:
        - path to files
        - path to output directory
    Examples:
        /home/cbpi/bin/text_rework --help
        /home/cbpi/bin/text_rework ~/.bashrc i -o .
cbpi@vault_rpi:~/geekbrains/linux/l5$ echo $?
3
```

```ShellSession
cbpi@vault_rpi:~/geekbrains/linux/l5$ text_rework file1 -o out2
Can't find path or write to dir: out2
cbpi@vault_rpi:~/geekbrains/linux/l5$ echo $?
4
```

```ShellSession
cbpi@vault_rpi:~/geekbrains/linux/l5$ text_rework file{1..5} -o out
file1 -> out/out_file1 ... DONE
file2 -> out/out_file2 ... DONE
file3 -> out/out_file3 ... DONE
Can't find or read file: file4
Can't find or read file: file5
cbpi@vault_rpi:~/geekbrains/linux/l5$ echo $?
5
cbpi@vault_rpi:~/geekbrains/linux/l5$ text_rework file* -o out
file1 -> out/out_file1 ... DONE
file2 -> out/out_file2 ... DONE
file3 -> out/out_file3 ... DONE
cbpi@vault_rpi:~/geekbrains/linux/l5$ echo $?
0
```

> окончание проверки
> ---
</details>

# **task 1b:**
Написать скрипт мониторинга лога `/var/log/auth.log`, чтобы он выводил сообщения при попытке неудачной аутентификации пользователя, отслеживая сообщения примерно такого вида:
`May 16 19:45:52 vlamp login[102782]: FAILED LOGIN (1) on '/dev/tty3' FOR 'user', Authentication failure`
Проверить скрипт, сделав корректную и ошибочную аутентификации с реального и виртуального терминала.

Для мониторинга файла `/var/log/auth.log` используем команду `tail -0f /var/log/auth.log`, где `-0f` обозначает вывести 0 последних строк и выводить новые строки по мере появления.

- При неудачной попытке аутентификации в графическом интерфейсе сообщение в `auth.log` имеет вид:

```
Jan 22 19:18:59 cbpi-VirtualBox gdm-password]: pam_unix(gdm-password:auth): authentication failure; logname= uid=0 euid=0 tty=/dev/tty1 ruser= rhost=  user=cbpi
```

- При неудачной попытке аутентификации по `ssh` сообщение в `auth.log` имеет вид:

```
Jan 22 19:20:52 cbpi-VirtualBox sshd[4663]: pam_unix(sshd:auth): authentication failure; logname= uid=0 euid=0 tty=ssh ruser= rhost=192.168.41.2  user=cbpi
Jan 22 19:20:54 cbpi-VirtualBox sshd[4663]: Failed password for cbpi from 192.168.41.2 port 60400 ssh2
```

- При неудачной попытке аутентификации в виртуальном терминале сообщение в `auth.log` имеет вид:

```
Jan 22 19:22:02 cbpi-VirtualBox login[4649]: pam_unix(login:auth): authentication failure; logname=LOGIN uid=0 euid=0 tty=/dev/tty2 ruser= rhost=
Jan 22 19:22:05 cbpi-VirtualBox login[4649]: FAILED LOGIN (1) on '/dev/tty2' FOR 'UNKNOWN', Authentication failure
```

Создание скрипта:

```ShellSession
cbpi@cbpi-VirtualBox:~ $ vim ~/bin/auth_monitor.sh
...
cbpi@cbpi-VirtualBox:~ $ chmod +x ~/bin/auth_monitor.sh
cbpi@cbpi-VirtualBox:~ $ sudo ln -s ~/bin/auth_monitor.sh /usr/sbin/authmon
```

Скрипт:

```Shell
#!/bin/bash

LOG=/var/log/auth.log
AUTH_FAIL="^.*auth.*failure.*tty=.*$"
TTY1="^.*tty=.*tty1.*$"

tail -0f "${LOG}" | while read str
do
    [[ $str =~ $AUTH_FAIL ]] && {
        printf "\n%s\n%s\n" ATTENTION! "$str" &&
        [[ ! $str =~ $TTY1 ]] && read str2 && printf "%s\n" "$str2" ; }
done
```

<details>
<summary>Проверка</summary>

SSH сессия:

```ShellSession
cbpi@cbpi-VirtualBox:~ $ authmon &
[1] 5928
cbpi@cbpi-VirtualBox:~ $
```

- Неудачная попытка аутентификации в графическом интерфейсе. SSH сессия с запущенным скриптом:

```ShellSession
cbpi@cbpi-VirtualBox:~ $
ATTENTION!
Jan 22 21:37:24 cbpi-VirtualBox gdm-password]: pam_unix(gdm-password:auth): authentication failure; logname= uid=0 euid=0 tty=/dev/tty1 ruser= rhost=  user=cbpi

```

- При удачной аутентификации в графическом интерфейсе скрипт ничего не выводит.

- Неудачная попытка аутентификации в виртуальном терминале. SSH сессия с запущенным скриптом:

```ShellSession
cbpi@cbpi-VirtualBox:~ $
ATTENTION!
Jan 22 21:40:20 cbpi-VirtualBox login[5827]: pam_unix(login:auth): authentication failure; logname=LOGIN uid=0 euid=0 tty=/dev/tty2 ruser= rhost=  user=cbpi
Jan 22 21:40:23 cbpi-VirtualBox login[5827]: FAILED LOGIN (1) on '/dev/tty2' FOR 'cbpi', Authentication failure

```

- При удачной аутентификации в виртуальном терминале скрипт ничего не выводит.

- Неудачная попытка аутентификации по ssh. SSH сессия с запущенным скриптом:

```ShellSession
cbpi@cbpi-VirtualBox:~ $
ATTENTION!
Jan 22 21:42:49 cbpi-VirtualBox sshd[6730]: pam_unix(sshd:auth): authentication failure; logname= uid=0 euid=0 tty=ssh ruser= rhost=192.168.41.2  user=cbpi
Jan 22 21:42:50 cbpi-VirtualBox sshd[6730]: Failed password for cbpi from 192.168.41.2 port 53730 ssh2

```

- При удачной аутентификации по ssh скрипт ничего не выводит.

> окончание проверки
> ---
</details>

# **task 1c:**
Создать скрипт, который создаст директории для нескольких годов (2010 — 2017), в них — поддиректории для месяцев (от 01 до 12), и в каждый из них запишет несколько файлов с произвольными записями (например 001.txt, содержащий текст Файл 001, 002.txt с текстом Файл 002) и т.д.

Создание скрипта:

```ShellSession
cbpi@vault_rpi:~/geekbrains/linux/l5$ vim make_dirs.sh
...
cbpi@cbpi-VirtualBox:~ $ chmod +x ~/bin/auth_monitor.sh
cbpi@vault_rpi:~/geekbrains/linux/l5$ ln -s $(pwd)/make_dirs.sh ~/bin/make_dirs
```

<details>
<summary>Скрипт</summary>

```Shell
#!/bin/bash

ARG1_ERR="Not enough arguments"
ARG2_ERR="[End year] must be greater than [start year], both arguments must be positive integer"
ARG3_ERR="Number of files must be positive integer from 1 to 999"
W_ERR="Can't write to the current directory"

usage(){
    cat <<-EOF
    This script creates directories, subdirectories and files in the current directory:
    year{start..end}/{01..12}/{001..num}.txt
        start - first argument of the script;
        end - second argument of the script;
        num - third argument of the script;

    Usage: $0 [start year] [end year] [number of files] [--help]

    Examples:
        $0 --help
        $0 2000 2005 5
EOF
}

print_err(){
    >&2 echo $1
    usage
    exit $2
}

[[ $1 == --help ]] &&  { usage; exit 0; }
[[ $# != 3 ]] && print_err "$ARG1_ERR" 1
[[ $1 -lt $2 && $1 -gt 0 && $2 -gt 0 ]] || print_err "$ARG2_ERR" 2
[[ $3 -gt 1 && $3 -lt 999 ]] || print_err "$ARG3_ERR" 3
[[ -w . ]] || print_err "$W_ERR" 4

echo Processing...
for year in $(seq $1 $2)
do
    mkdir $year
    for month in {01..12}
    do
        mkdir $year/$month
        for file in $(seq -f "%03g" 1 $3)
        do
            echo File $file > $year/$month/${file}.txt
        done
    done
done
echo Done!
exit 0
```

> окончание скрипта
> ---
</details>

<details>
<summary>Проверка</summary>

```ShellSession
cbpi@vault_rpi:~/geekbrains/linux/l5$ mkdir 1c; cd 1c
cbpi@vault_rpi:~/geekbrains/linux/l5/1c$ make_dirs 2010 2017 5
Processing...
Done!
cbpi@vault_rpi:~/geekbrains/linux/l5/1c$ ls ./*
./2010:
01  02  03  04  05  06  07  08  09  10  11  12

./2011:
01  02  03  04  05  06  07  08  09  10  11  12

./2012:
01  02  03  04  05  06  07  08  09  10  11  12

./2013:
01  02  03  04  05  06  07  08  09  10  11  12

./2014:
01  02  03  04  05  06  07  08  09  10  11  12

./2015:
01  02  03  04  05  06  07  08  09  10  11  12

./2016:
01  02  03  04  05  06  07  08  09  10  11  12

./2017:
01  02  03  04  05  06  07  08  09  10  11  12
cbpi@vault_rpi:~/geekbrains/linux/l5/1c$ ls 2010/01
001.txt  002.txt  003.txt  004.txt  005.txt
cbpi@vault_rpi:~/geekbrains/linux/l5/1c$ cat 2010/01/*
File 001
File 002
File 003
File 004
File 005
```

```ShellSession
cbpi@vault_rpi:~/geekbrains/linux/l5/1c$ make_dirs
Not enough arguments
    This script creates directories, subdirectories and files in the current directory:
    year{start..end}/{01..12}/{001..num}.txt
        start - first argument of the script;
        end - second argument of the script;
        num - third argument of the script;

    Usage: /home/cbpi/bin/make_dirs [start year] [end year] [number of files]

    Examples:
        /home/cbpi/bin/make_dirs --help
        /home/cbpi/bin/make_dirs 2000 2005 5
cbpi@vault_rpi:~/geekbrains/linux/l5/1c$ echo $?
1
```

```ShellSession
cbpi@vault_rpi:~/geekbrains/linux/l5/1c$ make_dirs --help
    This script creates directories, subdirectories and files in the current directory:
    year{start..end}/{01..12}/{001..num}.txt
        start - first argument of the script;
        end - second argument of the script;
        num - third argument of the script;

    Usage: /home/cbpi/bin/make_dirs [start year] [end year] [number of files]

    Examples:
        /home/cbpi/bin/make_dirs --help
        /home/cbpi/bin/make_dirs 2000 2005 5
cbpi@vault_rpi:~/geekbrains/linux/l5/1c$ echo $?
0
```

```ShellSession
cbpi@vault_rpi:~/geekbrains/linux/l5/1c$ make_dirs 2017 2010 5
[End year] must be greater than [start year], both arguments must be positive integer
    This script creates directories, subdirectories and files in the current directory:
    year{start..end}/{01..12}/{001..num}.txt
        start - first argument of the script;
        end - second argument of the script;
        num - third argument of the script;

    Usage: /home/cbpi/bin/make_dirs [start year] [end year] [number of files]

    Examples:
        /home/cbpi/bin/make_dirs --help
        /home/cbpi/bin/make_dirs 2000 2005 5
cbpi@vault_rpi:~/geekbrains/linux/l5/1c$ echo $?
2
cbpi@vault_rpi:~/geekbrains/linux/l5/1c$ make_dirs a z 5
[End year] must be greater than [start year], both arguments must be positive integer
    This script creates directories, subdirectories and files in the current directory:
    year{start..end}/{01..12}/{001..num}.txt
        start - first argument of the script;
        end - second argument of the script;
        num - third argument of the script;

    Usage: /home/cbpi/bin/make_dirs [start year] [end year] [number of files]

    Examples:
        /home/cbpi/bin/make_dirs --help
        /home/cbpi/bin/make_dirs 2000 2005 5
cbpi@vault_rpi:~/geekbrains/linux/l5/1c$ echo $?
2
```

```ShellSession
cbpi@vault_rpi:~/geekbrains/linux/l5/1c$ make_dirs 2010 2017 0
Number of files must be positive integer from 1 to 999
    This script creates directories, subdirectories and files in the current directory:
    year{start..end}/{01..12}/{001..num}.txt
        start - first argument of the script;
        end - second argument of the script;
        num - third argument of the script;

    Usage: /home/cbpi/bin/make_dirs [start year] [end year] [number of files]

    Examples:
        /home/cbpi/bin/make_dirs --help
        /home/cbpi/bin/make_dirs 2000 2005 5
cbpi@vault_rpi:~/geekbrains/linux/l5/1c$ echo $?
3
cbpi@vault_rpi:~/geekbrains/linux/l5/1c$ make_dirs 2010 2017 c
Number of files must be positive integer from 1 to 999
    This script creates directories, subdirectories and files in the current directory:
    year{start..end}/{01..12}/{001..num}.txt
        start - first argument of the script;
        end - second argument of the script;
        num - third argument of the script;

    Usage: /home/cbpi/bin/make_dirs [start year] [end year] [number of files]

    Examples:
        /home/cbpi/bin/make_dirs --help
        /home/cbpi/bin/make_dirs 2000 2005 5
cbpi@vault_rpi:~/geekbrains/linux/l5/1c$ echo $?
3
```

```ShellSession
cbpi@vault_rpi:~/geekbrains/linux/l5/1c$ chmod -w .
cbpi@vault_rpi:~/geekbrains/linux/l5/1c$ make_dirs 2010 2017 5
Can't write to the current directory
    This script creates directories, subdirectories and files in the current directory:
    year{start..end}/{01..12}/{001..num}.txt
        start - first argument of the script;
        end - second argument of the script;
        num - third argument of the script;

    Usage: /home/cbpi/bin/make_dirs [start year] [end year] [number of files]

    Examples:
        /home/cbpi/bin/make_dirs --help
        /home/cbpi/bin/make_dirs 2000 2005 5
cbpi@vault_rpi:~/geekbrains/linux/l5/1c$ echo $?
4
```

> окончание проверки
> ---
</details>

# **task 2a:**
Создать файл crontab, который ежедневно регистрирует занятое каждым пользователем дисковое пространство в его домашней директории.

Чтобы каждый пользователь имел возможность проверить занятое дисковое пространство добавим в `.bashrc` следующую строку:

```Shell
read HSIZE <<< $(cat ~/.size)
```

Задача `cron` в свою очередь будет записывать полученное значение занятого пространства в файл `.size` в домашней директории каждого пользователя.

Создадим скрипт для проверки занятого пространства:

```ShellSession
root@cbpi-VirtualBox:~# vim /usr/sbin/get_home_sizes
...
root@cbpi-VirtualBox:~# chmod +x /usr/sbin/get_home_sizes
```

Скрипт:

```Shell
#!/bin/bash

COMMAND='read HSIZE <<< $(cat ~/.size)'
F_SIZE=".size"

for homedir in /home/*
do
    grep -e "$COMMAND" $homedir/.bashrc &> /dev/null
    [[ $? == 0 ]] || echo "$COMMAND" >> $homedir/.bashrc
    du -hs $homedir | awk '{print $1}' > $homedir/$F_SIZE
done
```

Создадим задачу в 'crontab', скрипт `get_home_sizes` будет выполнятся каждый день в `9:00`

```ShellSession
root@cbpi-VirtualBox:~# crontab -e
# Edit this file to introduce tasks to be run by cron.
#
# Each task to run has to be defined through a single line
# indicating with different fields when the task will be run
# and what command to run for the task
#
# To define the time you can provide concrete values for
# minute (m), hour (h), day of month (dom), month (mon),
# and day of week (dow) or use '*' in these fields (for 'any').#
# Notice that tasks will be started based on the cron's system
# daemon's notion of time and timezones.
#
# Output of the crontab jobs (including errors) is sent through
# email to the user the crontab file belongs to (unless redirected).
#
# For example, you can run a backup of all your user accounts
# at 5 a.m every week with:
# 0 5 * * 1 tar -zcf /var/backups/home.tgz /home/
#
# For more information see the manual pages of crontab(5) and cron(8)
#
# m h  dom mon dow   command
0 9 * * * /usr/sbin/get_home_sizes
```

Для проверки было выставлено иное время запуска (`3:00`), скрипт в заданное тестовое время выполнился успешно:

```ShellSession
root@cbpi-VirtualBox:~# ll /home/*/.size
-rw-r--r-- 1 root root 5 янв 23 03:00 /home/cbpi/.size
-rw-r--r-- 1 root root 4 янв 23 03:00 /home/dev1/.size
-rw-r--r-- 1 root root 4 янв 23 03:00 /home/dev2/.size
-rw-r--r-- 1 root root 4 янв 23 03:00 /home/dev3/.size
root@cbpi-VirtualBox:~# su cbpi
Пароль:
cbpi@cbpi-VirtualBox:/root $ echo $HSIZE
1,3G
cbpi@cbpi-VirtualBox:/root $ su dev1
Пароль:
dev1@cbpi-VirtualBox:/root$ echo $HSIZE
48K
dev1@cbpi-VirtualBox:/root$ su dev2
Пароль:
dev2@cbpi-VirtualBox:/root$ echo $HSIZE
40K
dev2@cbpi-VirtualBox:/root$ su dev3
Пароль:
dev3@cbpi-VirtualBox:/root$ echo $HSIZE
40K
```

# **task 2b:**
Создать скрипт ownersort.sh, который в заданной папке копирует файлы в директории, названные по имени владельца каждого файла. Учтите, что файл должен принадлежать соответствующему владельцу 

Создание скрипта:

```ShellSession
root@cbpi-VirtualBox:~# vim /usr/sbin/ownersort.sh
...
root@cbpi-VirtualBox:~# chmod +x /usr/sbin/ownersort.sh
```

Скрипт:

```Shell
#!/bin/bash

usage(){
    cat <<-EOF
    This script copies every file in the current directory to the directory named with the file owner (directory will be created if nessesary)
    
    Usage: $0 [--help] [-p]
    With the option [-p] the script work process will be printed to stdout
	
    Examples:
        $0 --help
        $0 -p
        $0
EOF
}

[[ $1 == "--help" ]] && usage && exit 0
[[ $# > 1 ]] && >&2 echo "Too many arguments" && usage && exit 1
[[ $# == 1 && $1 != "-p" ]] && >&2 echo "Unknown argument" && usage && exit 2

for file in *
do
    if [[ -h $file || -f $file ]]; then
        USER=$(stat -c '%U' $file)
        [[ -d $USER ]] || { mkdir $USER && chown $USER $USER && 
        [[ $1 == "-p" ]] && echo "making directory: $USER ... DONE" ; }
        cp $file $USER/ && chown $USER $USER/$file &&
		[[ $1 == "-p" ]] && echo -e "copying file: $file -> $USER/$file ... DONE"
    fi
done
exit 0
```

<details>
<summary>Проверка</summary>

- Директория для проверки:

```ShellSession
root@cbpi-VirtualBox:/developer# ll
итого 16
drwxrwsr-x+  4 root developer 4096 янв 23 03:41 ./
drwxr-xr-x  25 root root      4096 дек 27 21:48 ../
-rw-rw-r--+  1 cbpi developer    0 янв 23 03:14 cbpiFile1
-rw-rw-r--+  1 cbpi developer    0 янв 23 03:14 cbpiFile2
-rw-rw-r--+  1 cbpi developer    0 янв 23 03:14 cbpiFile3
-rw-rw-r--+  1 cbpi developer    0 янв 23 03:14 cbpiFile4
-rw-rw-r--+  1 cbpi developer    0 янв 23 03:14 cbpiFile5
-rw-rw-r--+  1 dev1 developer    0 янв 23 03:14 dev1file1
-rw-rw-r--+  1 dev1 developer    0 янв 23 03:14 dev1file2
-rw-rw-r--+  1 dev1 developer    0 янв 23 03:14 dev1file3
-rw-rw-r--+  1 dev1 developer    0 янв 23 03:14 dev1file4
-rw-rw-r--+  1 dev1 developer    0 янв 23 03:14 dev1file5
-rw-rw-r--+  1 dev2 developer    0 янв 23 03:14 dev2file1
-rw-rw-r--+  1 dev2 developer    0 янв 23 03:14 dev2file2
-rw-rw-r--+  1 dev2 developer    0 янв 23 03:14 dev2file3
-rw-rw-r--+  1 dev2 developer    0 янв 23 03:14 dev2file4
-rw-rw-r--+  1 dev2 developer    0 янв 23 03:14 dev2file5
-rw-rw-r--+  1 dev3 developer    0 янв 23 03:15 dev3file1
-rw-rw-r--+  1 dev3 developer    0 янв 23 03:15 dev3file2
drwxrws--t+  2 root developer 4096 дек 27 22:08 hiddenFiles/
drwxrwsr-t+  2 root developer 4096 дек 30 00:45 shared/
```

- Проверка:

```ShellSession
root@cbpi-VirtualBox:/developer# ownersort.sh -p
making directory: cbpi ... DONE
copying file: cbpiFile1 -> cbpi/cbpiFile1 ... DONE
copying file: cbpiFile2 -> cbpi/cbpiFile2 ... DONE
copying file: cbpiFile3 -> cbpi/cbpiFile3 ... DONE
copying file: cbpiFile4 -> cbpi/cbpiFile4 ... DONE
copying file: cbpiFile5 -> cbpi/cbpiFile5 ... DONE
making directory: dev1 ... DONE
copying file: dev1file1 -> dev1/dev1file1 ... DONE
copying file: dev1file2 -> dev1/dev1file2 ... DONE
copying file: dev1file3 -> dev1/dev1file3 ... DONE
copying file: dev1file4 -> dev1/dev1file4 ... DONE
copying file: dev1file5 -> dev1/dev1file5 ... DONE
making directory: dev2 ... DONE
copying file: dev2file1 -> dev2/dev2file1 ... DONE
copying file: dev2file2 -> dev2/dev2file2 ... DONE
copying file: dev2file3 -> dev2/dev2file3 ... DONE
copying file: dev2file4 -> dev2/dev2file4 ... DONE
copying file: dev2file5 -> dev2/dev2file5 ... DONE
making directory: dev3 ... DONE
copying file: dev3file1 -> dev3/dev3file1 ... DONE
copying file: dev3file2 -> dev3/dev3file2 ... DONE
root@cbpi-VirtualBox:/developer# ll
итого 32
drwxrwsr-x+  8 root developer 4096 янв 23 04:11 ./
drwxr-xr-x  25 root root      4096 дек 27 21:48 ../
drwxrwsr-x+  2 cbpi developer 4096 янв 23 04:11 cbpi/
-rw-rw-r--+  1 cbpi developer    0 янв 23 03:14 cbpiFile1
-rw-rw-r--+  1 cbpi developer    0 янв 23 03:14 cbpiFile2
-rw-rw-r--+  1 cbpi developer    0 янв 23 03:14 cbpiFile3
-rw-rw-r--+  1 cbpi developer    0 янв 23 03:14 cbpiFile4
-rw-rw-r--+  1 cbpi developer    0 янв 23 03:14 cbpiFile5
drwxrwsr-x+  2 dev1 developer 4096 янв 23 04:11 dev1/
-rw-rw-r--+  1 dev1 developer    0 янв 23 03:14 dev1file1
-rw-rw-r--+  1 dev1 developer    0 янв 23 03:14 dev1file2
-rw-rw-r--+  1 dev1 developer    0 янв 23 03:14 dev1file3
-rw-rw-r--+  1 dev1 developer    0 янв 23 03:14 dev1file4
-rw-rw-r--+  1 dev1 developer    0 янв 23 03:14 dev1file5
drwxrwsr-x+  2 dev2 developer 4096 янв 23 04:11 dev2/
-rw-rw-r--+  1 dev2 developer    0 янв 23 03:14 dev2file1
-rw-rw-r--+  1 dev2 developer    0 янв 23 03:14 dev2file2
-rw-rw-r--+  1 dev2 developer    0 янв 23 03:14 dev2file3
-rw-rw-r--+  1 dev2 developer    0 янв 23 03:14 dev2file4
-rw-rw-r--+  1 dev2 developer    0 янв 23 03:14 dev2file5
drwxrwsr-x+  2 dev3 developer 4096 янв 23 04:11 dev3/
-rw-rw-r--+  1 dev3 developer    0 янв 23 03:15 dev3file1
-rw-rw-r--+  1 dev3 developer    0 янв 23 03:15 dev3file2
drwxrws--t+  2 root developer 4096 дек 27 22:08 hiddenFiles/
drwxrwsr-t+  2 root developer 4096 дек 30 00:45 shared/
root@cbpi-VirtualBox:/developer# ls -lA cbpi/* dev*/*
-rw-rw-r--+ 1 cbpi developer 0 янв 23 04:11 cbpi/cbpiFile1
-rw-rw-r--+ 1 cbpi developer 0 янв 23 04:11 cbpi/cbpiFile2
-rw-rw-r--+ 1 cbpi developer 0 янв 23 04:11 cbpi/cbpiFile3
-rw-rw-r--+ 1 cbpi developer 0 янв 23 04:11 cbpi/cbpiFile4
-rw-rw-r--+ 1 cbpi developer 0 янв 23 04:11 cbpi/cbpiFile5
-rw-rw-r--+ 1 dev1 developer 0 янв 23 04:11 dev1/dev1file1
-rw-rw-r--+ 1 dev1 developer 0 янв 23 04:11 dev1/dev1file2
-rw-rw-r--+ 1 dev1 developer 0 янв 23 04:11 dev1/dev1file3
-rw-rw-r--+ 1 dev1 developer 0 янв 23 04:11 dev1/dev1file4
-rw-rw-r--+ 1 dev1 developer 0 янв 23 04:11 dev1/dev1file5
-rw-rw-r--+ 1 dev2 developer 0 янв 23 04:11 dev2/dev2file1
-rw-rw-r--+ 1 dev2 developer 0 янв 23 04:11 dev2/dev2file2
-rw-rw-r--+ 1 dev2 developer 0 янв 23 04:11 dev2/dev2file3
-rw-rw-r--+ 1 dev2 developer 0 янв 23 04:11 dev2/dev2file4
-rw-rw-r--+ 1 dev2 developer 0 янв 23 04:11 dev2/dev2file5
-rw-rw-r--+ 1 dev3 developer 0 янв 23 04:11 dev3/dev3file1
-rw-rw-r--+ 1 dev3 developer 0 янв 23 04:11 dev3/dev3file2
```

> окончание проверки
> ---
</details>

# **task 2c:**
Написать скрипт rename.sh, аналогичный разобранному, но порядковые номера файлов выравнивать, заполняя слева нуля до ширины максимального значения индекса: newname000.jpg, newname102.jpg (Использовать printf). Дополнительно к 3 добавить проверку на расширение, чтобы не переименовать .sh.

Создание скрипта:

```ShellSession
cbpi@vault_rpi:~/geekbrains/linux/l5$ vim rename.sh
...
cbpi@vault_rpi:~/geekbrains/linux/l5$ chmod +x rename.sh
cbpi@vault_rpi:~/geekbrains/linux/l5$ ln -s $(pwd)/rename.sh ~/bin/renamer
```

Скрипт:

```Shell
#!/bin/bash

EXCLUDE='.*\.sh'

usage(){
    cat <<-EOF
    This script renames all given image files to [prefix]{1..N}.jpg

    Usage: $0 [--help] [prefix] [file(s)]

    Examples:
        $0 --help
        $0 awesome_prefix IMG_00000?.jpg
EOF
}

[[ $1 == "--help" ]] && usage && exit 0
[[ $# -lt 2 ]] && >&2 echo "No specified files to rename" && usage && exit 1

PREFIX=$1; shift
DIGITS=${##}
COUNT=1

echo Processing...
for file in $@
do
    if [[ -h $file || -f $file ]]; then
        [[ $file =~ $EXCLUDE ]] || {
        mv -n "${file}" "${PREFIX}$(printf "%0${DIGITS}d" $COUNT).jpg";
        COUNT=$[COUNT+1] ; }
    fi
done
echo Done!
exit 0
```

<details>
<summary>Проверка</summary>

- Директория для проверки:

```ShellSession
cbpi@vault_rpi:~/geekbrains/linux/l5$ mkdir 2c; cd 2c
cbpi@vault_rpi:~/geekbrains/linux/l5/2c$ touch IMG_{00000..00300}.jpg
cbpi@vault_rpi:~/geekbrains/linux/l5/2c$ touch test.sh
cbpi@vault_rpi:~/geekbrains/linux/l5/2c$ ls
IMG000000.jpg  IMG000038.jpg  IMG000076.jpg  IMG000114.jpg  IMG000152.jpg  IMG000190.jpg  IMG000228.jpg  IMG000266.jpg
IMG000001.jpg  IMG000039.jpg  IMG000077.jpg  IMG000115.jpg  IMG000153.jpg  IMG000191.jpg  IMG000229.jpg  IMG000267.jpg
IMG000002.jpg  IMG000040.jpg  IMG000078.jpg  IMG000116.jpg  IMG000154.jpg  IMG000192.jpg  IMG000230.jpg  IMG000268.jpg
IMG000003.jpg  IMG000041.jpg  IMG000079.jpg  IMG000117.jpg  IMG000155.jpg  IMG000193.jpg  IMG000231.jpg  IMG000269.jpg
IMG000004.jpg  IMG000042.jpg  IMG000080.jpg  IMG000118.jpg  IMG000156.jpg  IMG000194.jpg  IMG000232.jpg  IMG000270.jpg
IMG000005.jpg  IMG000043.jpg  IMG000081.jpg  IMG000119.jpg  IMG000157.jpg  IMG000195.jpg  IMG000233.jpg  IMG000271.jpg
IMG000006.jpg  IMG000044.jpg  IMG000082.jpg  IMG000120.jpg  IMG000158.jpg  IMG000196.jpg  IMG000234.jpg  IMG000272.jpg
IMG000007.jpg  IMG000045.jpg  IMG000083.jpg  IMG000121.jpg  IMG000159.jpg  IMG000197.jpg  IMG000235.jpg  IMG000273.jpg
IMG000008.jpg  IMG000046.jpg  IMG000084.jpg  IMG000122.jpg  IMG000160.jpg  IMG000198.jpg  IMG000236.jpg  IMG000274.jpg
IMG000009.jpg  IMG000047.jpg  IMG000085.jpg  IMG000123.jpg  IMG000161.jpg  IMG000199.jpg  IMG000237.jpg  IMG000275.jpg
IMG000010.jpg  IMG000048.jpg  IMG000086.jpg  IMG000124.jpg  IMG000162.jpg  IMG000200.jpg  IMG000238.jpg  IMG000276.jpg
IMG000011.jpg  IMG000049.jpg  IMG000087.jpg  IMG000125.jpg  IMG000163.jpg  IMG000201.jpg  IMG000239.jpg  IMG000277.jpg
IMG000012.jpg  IMG000050.jpg  IMG000088.jpg  IMG000126.jpg  IMG000164.jpg  IMG000202.jpg  IMG000240.jpg  IMG000278.jpg
IMG000013.jpg  IMG000051.jpg  IMG000089.jpg  IMG000127.jpg  IMG000165.jpg  IMG000203.jpg  IMG000241.jpg  IMG000279.jpg
IMG000014.jpg  IMG000052.jpg  IMG000090.jpg  IMG000128.jpg  IMG000166.jpg  IMG000204.jpg  IMG000242.jpg  IMG000280.jpg
IMG000015.jpg  IMG000053.jpg  IMG000091.jpg  IMG000129.jpg  IMG000167.jpg  IMG000205.jpg  IMG000243.jpg  IMG000281.jpg
IMG000016.jpg  IMG000054.jpg  IMG000092.jpg  IMG000130.jpg  IMG000168.jpg  IMG000206.jpg  IMG000244.jpg  IMG000282.jpg
IMG000017.jpg  IMG000055.jpg  IMG000093.jpg  IMG000131.jpg  IMG000169.jpg  IMG000207.jpg  IMG000245.jpg  IMG000283.jpg
IMG000018.jpg  IMG000056.jpg  IMG000094.jpg  IMG000132.jpg  IMG000170.jpg  IMG000208.jpg  IMG000246.jpg  IMG000284.jpg
IMG000019.jpg  IMG000057.jpg  IMG000095.jpg  IMG000133.jpg  IMG000171.jpg  IMG000209.jpg  IMG000247.jpg  IMG000285.jpg
IMG000020.jpg  IMG000058.jpg  IMG000096.jpg  IMG000134.jpg  IMG000172.jpg  IMG000210.jpg  IMG000248.jpg  IMG000286.jpg
IMG000021.jpg  IMG000059.jpg  IMG000097.jpg  IMG000135.jpg  IMG000173.jpg  IMG000211.jpg  IMG000249.jpg  IMG000287.jpg
IMG000022.jpg  IMG000060.jpg  IMG000098.jpg  IMG000136.jpg  IMG000174.jpg  IMG000212.jpg  IMG000250.jpg  IMG000288.jpg
IMG000023.jpg  IMG000061.jpg  IMG000099.jpg  IMG000137.jpg  IMG000175.jpg  IMG000213.jpg  IMG000251.jpg  IMG000289.jpg
IMG000024.jpg  IMG000062.jpg  IMG000100.jpg  IMG000138.jpg  IMG000176.jpg  IMG000214.jpg  IMG000252.jpg  IMG000290.jpg
IMG000025.jpg  IMG000063.jpg  IMG000101.jpg  IMG000139.jpg  IMG000177.jpg  IMG000215.jpg  IMG000253.jpg  IMG000291.jpg
IMG000026.jpg  IMG000064.jpg  IMG000102.jpg  IMG000140.jpg  IMG000178.jpg  IMG000216.jpg  IMG000254.jpg  IMG000292.jpg
IMG000027.jpg  IMG000065.jpg  IMG000103.jpg  IMG000141.jpg  IMG000179.jpg  IMG000217.jpg  IMG000255.jpg  IMG000293.jpg
IMG000028.jpg  IMG000066.jpg  IMG000104.jpg  IMG000142.jpg  IMG000180.jpg  IMG000218.jpg  IMG000256.jpg  IMG000294.jpg
IMG000029.jpg  IMG000067.jpg  IMG000105.jpg  IMG000143.jpg  IMG000181.jpg  IMG000219.jpg  IMG000257.jpg  IMG000295.jpg
IMG000030.jpg  IMG000068.jpg  IMG000106.jpg  IMG000144.jpg  IMG000182.jpg  IMG000220.jpg  IMG000258.jpg  IMG000296.jpg
IMG000031.jpg  IMG000069.jpg  IMG000107.jpg  IMG000145.jpg  IMG000183.jpg  IMG000221.jpg  IMG000259.jpg  IMG000297.jpg
IMG000032.jpg  IMG000070.jpg  IMG000108.jpg  IMG000146.jpg  IMG000184.jpg  IMG000222.jpg  IMG000260.jpg  IMG000298.jpg
IMG000033.jpg  IMG000071.jpg  IMG000109.jpg  IMG000147.jpg  IMG000185.jpg  IMG000223.jpg  IMG000261.jpg  IMG000299.jpg
IMG000034.jpg  IMG000072.jpg  IMG000110.jpg  IMG000148.jpg  IMG000186.jpg  IMG000224.jpg  IMG000262.jpg  IMG000300.jpg
IMG000035.jpg  IMG000073.jpg  IMG000111.jpg  IMG000149.jpg  IMG000187.jpg  IMG000225.jpg  IMG000263.jpg  test.sh
IMG000036.jpg  IMG000074.jpg  IMG000112.jpg  IMG000150.jpg  IMG000188.jpg  IMG000226.jpg  IMG000264.jpg
IMG000037.jpg  IMG000075.jpg  IMG000113.jpg  IMG000151.jpg  IMG000189.jpg  IMG000227.jpg  IMG000265.jpg
```

- Проверка:

```ShellSession
cbpi@vault_rpi:~/geekbrains/linux/l5/2c$ renamer foo *
Processing...
Done!
cbpi@vault_rpi:~/geekbrains/linux/l5/2c$ ls
foo001.jpg  foo032.jpg  foo063.jpg  foo094.jpg  foo125.jpg  foo156.jpg  foo187.jpg  foo218.jpg  foo249.jpg  foo280.jpg
foo002.jpg  foo033.jpg  foo064.jpg  foo095.jpg  foo126.jpg  foo157.jpg  foo188.jpg  foo219.jpg  foo250.jpg  foo281.jpg
foo003.jpg  foo034.jpg  foo065.jpg  foo096.jpg  foo127.jpg  foo158.jpg  foo189.jpg  foo220.jpg  foo251.jpg  foo282.jpg
foo004.jpg  foo035.jpg  foo066.jpg  foo097.jpg  foo128.jpg  foo159.jpg  foo190.jpg  foo221.jpg  foo252.jpg  foo283.jpg
foo005.jpg  foo036.jpg  foo067.jpg  foo098.jpg  foo129.jpg  foo160.jpg  foo191.jpg  foo222.jpg  foo253.jpg  foo284.jpg
foo006.jpg  foo037.jpg  foo068.jpg  foo099.jpg  foo130.jpg  foo161.jpg  foo192.jpg  foo223.jpg  foo254.jpg  foo285.jpg
foo007.jpg  foo038.jpg  foo069.jpg  foo100.jpg  foo131.jpg  foo162.jpg  foo193.jpg  foo224.jpg  foo255.jpg  foo286.jpg
foo008.jpg  foo039.jpg  foo070.jpg  foo101.jpg  foo132.jpg  foo163.jpg  foo194.jpg  foo225.jpg  foo256.jpg  foo287.jpg
foo009.jpg  foo040.jpg  foo071.jpg  foo102.jpg  foo133.jpg  foo164.jpg  foo195.jpg  foo226.jpg  foo257.jpg  foo288.jpg
foo010.jpg  foo041.jpg  foo072.jpg  foo103.jpg  foo134.jpg  foo165.jpg  foo196.jpg  foo227.jpg  foo258.jpg  foo289.jpg
foo011.jpg  foo042.jpg  foo073.jpg  foo104.jpg  foo135.jpg  foo166.jpg  foo197.jpg  foo228.jpg  foo259.jpg  foo290.jpg
foo012.jpg  foo043.jpg  foo074.jpg  foo105.jpg  foo136.jpg  foo167.jpg  foo198.jpg  foo229.jpg  foo260.jpg  foo291.jpg
foo013.jpg  foo044.jpg  foo075.jpg  foo106.jpg  foo137.jpg  foo168.jpg  foo199.jpg  foo230.jpg  foo261.jpg  foo292.jpg
foo014.jpg  foo045.jpg  foo076.jpg  foo107.jpg  foo138.jpg  foo169.jpg  foo200.jpg  foo231.jpg  foo262.jpg  foo293.jpg
foo015.jpg  foo046.jpg  foo077.jpg  foo108.jpg  foo139.jpg  foo170.jpg  foo201.jpg  foo232.jpg  foo263.jpg  foo294.jpg
foo016.jpg  foo047.jpg  foo078.jpg  foo109.jpg  foo140.jpg  foo171.jpg  foo202.jpg  foo233.jpg  foo264.jpg  foo295.jpg
foo017.jpg  foo048.jpg  foo079.jpg  foo110.jpg  foo141.jpg  foo172.jpg  foo203.jpg  foo234.jpg  foo265.jpg  foo296.jpg
foo018.jpg  foo049.jpg  foo080.jpg  foo111.jpg  foo142.jpg  foo173.jpg  foo204.jpg  foo235.jpg  foo266.jpg  foo297.jpg
foo019.jpg  foo050.jpg  foo081.jpg  foo112.jpg  foo143.jpg  foo174.jpg  foo205.jpg  foo236.jpg  foo267.jpg  foo298.jpg
foo020.jpg  foo051.jpg  foo082.jpg  foo113.jpg  foo144.jpg  foo175.jpg  foo206.jpg  foo237.jpg  foo268.jpg  foo299.jpg
foo021.jpg  foo052.jpg  foo083.jpg  foo114.jpg  foo145.jpg  foo176.jpg  foo207.jpg  foo238.jpg  foo269.jpg  foo300.jpg
foo022.jpg  foo053.jpg  foo084.jpg  foo115.jpg  foo146.jpg  foo177.jpg  foo208.jpg  foo239.jpg  foo270.jpg  foo301.jpg
foo023.jpg  foo054.jpg  foo085.jpg  foo116.jpg  foo147.jpg  foo178.jpg  foo209.jpg  foo240.jpg  foo271.jpg  test.sh
foo024.jpg  foo055.jpg  foo086.jpg  foo117.jpg  foo148.jpg  foo179.jpg  foo210.jpg  foo241.jpg  foo272.jpg
foo025.jpg  foo056.jpg  foo087.jpg  foo118.jpg  foo149.jpg  foo180.jpg  foo211.jpg  foo242.jpg  foo273.jpg
foo026.jpg  foo057.jpg  foo088.jpg  foo119.jpg  foo150.jpg  foo181.jpg  foo212.jpg  foo243.jpg  foo274.jpg
foo027.jpg  foo058.jpg  foo089.jpg  foo120.jpg  foo151.jpg  foo182.jpg  foo213.jpg  foo244.jpg  foo275.jpg
foo028.jpg  foo059.jpg  foo090.jpg  foo121.jpg  foo152.jpg  foo183.jpg  foo214.jpg  foo245.jpg  foo276.jpg
foo029.jpg  foo060.jpg  foo091.jpg  foo122.jpg  foo153.jpg  foo184.jpg  foo215.jpg  foo246.jpg  foo277.jpg
foo030.jpg  foo061.jpg  foo092.jpg  foo123.jpg  foo154.jpg  foo185.jpg  foo216.jpg  foo247.jpg  foo278.jpg
foo031.jpg  foo062.jpg  foo093.jpg  foo124.jpg  foo155.jpg  foo186.jpg  foo217.jpg  foo248.jpg  foo279.jpg
```

```ShellSession
cbpi@vault_rpi:~/geekbrains/linux/l5/2c$ renamer bar foo0{01..99}.jpg
Processing...
Done!
cbpi@vault_rpi:~/geekbrains/linux/l5/2c$ ls
bar01.jpg  bar32.jpg  bar63.jpg  bar94.jpg   foo125.jpg  foo156.jpg  foo187.jpg  foo218.jpg  foo249.jpg  foo280.jpg
bar02.jpg  bar33.jpg  bar64.jpg  bar95.jpg   foo126.jpg  foo157.jpg  foo188.jpg  foo219.jpg  foo250.jpg  foo281.jpg
bar03.jpg  bar34.jpg  bar65.jpg  bar96.jpg   foo127.jpg  foo158.jpg  foo189.jpg  foo220.jpg  foo251.jpg  foo282.jpg
bar04.jpg  bar35.jpg  bar66.jpg  bar97.jpg   foo128.jpg  foo159.jpg  foo190.jpg  foo221.jpg  foo252.jpg  foo283.jpg
bar05.jpg  bar36.jpg  bar67.jpg  bar98.jpg   foo129.jpg  foo160.jpg  foo191.jpg  foo222.jpg  foo253.jpg  foo284.jpg
bar06.jpg  bar37.jpg  bar68.jpg  bar99.jpg   foo130.jpg  foo161.jpg  foo192.jpg  foo223.jpg  foo254.jpg  foo285.jpg
bar07.jpg  bar38.jpg  bar69.jpg  foo100.jpg  foo131.jpg  foo162.jpg  foo193.jpg  foo224.jpg  foo255.jpg  foo286.jpg
bar08.jpg  bar39.jpg  bar70.jpg  foo101.jpg  foo132.jpg  foo163.jpg  foo194.jpg  foo225.jpg  foo256.jpg  foo287.jpg
bar09.jpg  bar40.jpg  bar71.jpg  foo102.jpg  foo133.jpg  foo164.jpg  foo195.jpg  foo226.jpg  foo257.jpg  foo288.jpg
bar10.jpg  bar41.jpg  bar72.jpg  foo103.jpg  foo134.jpg  foo165.jpg  foo196.jpg  foo227.jpg  foo258.jpg  foo289.jpg
bar11.jpg  bar42.jpg  bar73.jpg  foo104.jpg  foo135.jpg  foo166.jpg  foo197.jpg  foo228.jpg  foo259.jpg  foo290.jpg
bar12.jpg  bar43.jpg  bar74.jpg  foo105.jpg  foo136.jpg  foo167.jpg  foo198.jpg  foo229.jpg  foo260.jpg  foo291.jpg
bar13.jpg  bar44.jpg  bar75.jpg  foo106.jpg  foo137.jpg  foo168.jpg  foo199.jpg  foo230.jpg  foo261.jpg  foo292.jpg
bar14.jpg  bar45.jpg  bar76.jpg  foo107.jpg  foo138.jpg  foo169.jpg  foo200.jpg  foo231.jpg  foo262.jpg  foo293.jpg
bar15.jpg  bar46.jpg  bar77.jpg  foo108.jpg  foo139.jpg  foo170.jpg  foo201.jpg  foo232.jpg  foo263.jpg  foo294.jpg
bar16.jpg  bar47.jpg  bar78.jpg  foo109.jpg  foo140.jpg  foo171.jpg  foo202.jpg  foo233.jpg  foo264.jpg  foo295.jpg
bar17.jpg  bar48.jpg  bar79.jpg  foo110.jpg  foo141.jpg  foo172.jpg  foo203.jpg  foo234.jpg  foo265.jpg  foo296.jpg
bar18.jpg  bar49.jpg  bar80.jpg  foo111.jpg  foo142.jpg  foo173.jpg  foo204.jpg  foo235.jpg  foo266.jpg  foo297.jpg
bar19.jpg  bar50.jpg  bar81.jpg  foo112.jpg  foo143.jpg  foo174.jpg  foo205.jpg  foo236.jpg  foo267.jpg  foo298.jpg
bar20.jpg  bar51.jpg  bar82.jpg  foo113.jpg  foo144.jpg  foo175.jpg  foo206.jpg  foo237.jpg  foo268.jpg  foo299.jpg
bar21.jpg  bar52.jpg  bar83.jpg  foo114.jpg  foo145.jpg  foo176.jpg  foo207.jpg  foo238.jpg  foo269.jpg  foo300.jpg
bar22.jpg  bar53.jpg  bar84.jpg  foo115.jpg  foo146.jpg  foo177.jpg  foo208.jpg  foo239.jpg  foo270.jpg  foo301.jpg
bar23.jpg  bar54.jpg  bar85.jpg  foo116.jpg  foo147.jpg  foo178.jpg  foo209.jpg  foo240.jpg  foo271.jpg  test.sh
bar24.jpg  bar55.jpg  bar86.jpg  foo117.jpg  foo148.jpg  foo179.jpg  foo210.jpg  foo241.jpg  foo272.jpg
bar25.jpg  bar56.jpg  bar87.jpg  foo118.jpg  foo149.jpg  foo180.jpg  foo211.jpg  foo242.jpg  foo273.jpg
bar26.jpg  bar57.jpg  bar88.jpg  foo119.jpg  foo150.jpg  foo181.jpg  foo212.jpg  foo243.jpg  foo274.jpg
bar27.jpg  bar58.jpg  bar89.jpg  foo120.jpg  foo151.jpg  foo182.jpg  foo213.jpg  foo244.jpg  foo275.jpg
bar28.jpg  bar59.jpg  bar90.jpg  foo121.jpg  foo152.jpg  foo183.jpg  foo214.jpg  foo245.jpg  foo276.jpg
bar29.jpg  bar60.jpg  bar91.jpg  foo122.jpg  foo153.jpg  foo184.jpg  foo215.jpg  foo246.jpg  foo277.jpg
bar30.jpg  bar61.jpg  bar92.jpg  foo123.jpg  foo154.jpg  foo185.jpg  foo216.jpg  foo247.jpg  foo278.jpg
bar31.jpg  bar62.jpg  bar93.jpg  foo124.jpg  foo155.jpg  foo186.jpg  foo217.jpg  foo248.jpg  foo279.jpg
```

```ShellSession
cbpi@vault_rpi:~/geekbrains/linux/l5/2c$ renamer baz bar{01..09}.jpg
Processing...
Done!
cbpi@vault_rpi:~/geekbrains/linux/l5/2c$ ls
bar10.jpg  bar41.jpg  bar72.jpg  baz4.jpg    foo125.jpg  foo156.jpg  foo187.jpg  foo218.jpg  foo249.jpg  foo280.jpg
bar11.jpg  bar42.jpg  bar73.jpg  baz5.jpg    foo126.jpg  foo157.jpg  foo188.jpg  foo219.jpg  foo250.jpg  foo281.jpg
bar12.jpg  bar43.jpg  bar74.jpg  baz6.jpg    foo127.jpg  foo158.jpg  foo189.jpg  foo220.jpg  foo251.jpg  foo282.jpg
bar13.jpg  bar44.jpg  bar75.jpg  baz7.jpg    foo128.jpg  foo159.jpg  foo190.jpg  foo221.jpg  foo252.jpg  foo283.jpg
bar14.jpg  bar45.jpg  bar76.jpg  baz8.jpg    foo129.jpg  foo160.jpg  foo191.jpg  foo222.jpg  foo253.jpg  foo284.jpg
bar15.jpg  bar46.jpg  bar77.jpg  baz9.jpg    foo130.jpg  foo161.jpg  foo192.jpg  foo223.jpg  foo254.jpg  foo285.jpg
bar16.jpg  bar47.jpg  bar78.jpg  foo100.jpg  foo131.jpg  foo162.jpg  foo193.jpg  foo224.jpg  foo255.jpg  foo286.jpg
bar17.jpg  bar48.jpg  bar79.jpg  foo101.jpg  foo132.jpg  foo163.jpg  foo194.jpg  foo225.jpg  foo256.jpg  foo287.jpg
bar18.jpg  bar49.jpg  bar80.jpg  foo102.jpg  foo133.jpg  foo164.jpg  foo195.jpg  foo226.jpg  foo257.jpg  foo288.jpg
bar19.jpg  bar50.jpg  bar81.jpg  foo103.jpg  foo134.jpg  foo165.jpg  foo196.jpg  foo227.jpg  foo258.jpg  foo289.jpg
bar20.jpg  bar51.jpg  bar82.jpg  foo104.jpg  foo135.jpg  foo166.jpg  foo197.jpg  foo228.jpg  foo259.jpg  foo290.jpg
bar21.jpg  bar52.jpg  bar83.jpg  foo105.jpg  foo136.jpg  foo167.jpg  foo198.jpg  foo229.jpg  foo260.jpg  foo291.jpg
bar22.jpg  bar53.jpg  bar84.jpg  foo106.jpg  foo137.jpg  foo168.jpg  foo199.jpg  foo230.jpg  foo261.jpg  foo292.jpg
bar23.jpg  bar54.jpg  bar85.jpg  foo107.jpg  foo138.jpg  foo169.jpg  foo200.jpg  foo231.jpg  foo262.jpg  foo293.jpg
bar24.jpg  bar55.jpg  bar86.jpg  foo108.jpg  foo139.jpg  foo170.jpg  foo201.jpg  foo232.jpg  foo263.jpg  foo294.jpg
bar25.jpg  bar56.jpg  bar87.jpg  foo109.jpg  foo140.jpg  foo171.jpg  foo202.jpg  foo233.jpg  foo264.jpg  foo295.jpg
bar26.jpg  bar57.jpg  bar88.jpg  foo110.jpg  foo141.jpg  foo172.jpg  foo203.jpg  foo234.jpg  foo265.jpg  foo296.jpg
bar27.jpg  bar58.jpg  bar89.jpg  foo111.jpg  foo142.jpg  foo173.jpg  foo204.jpg  foo235.jpg  foo266.jpg  foo297.jpg
bar28.jpg  bar59.jpg  bar90.jpg  foo112.jpg  foo143.jpg  foo174.jpg  foo205.jpg  foo236.jpg  foo267.jpg  foo298.jpg
bar29.jpg  bar60.jpg  bar91.jpg  foo113.jpg  foo144.jpg  foo175.jpg  foo206.jpg  foo237.jpg  foo268.jpg  foo299.jpg
bar30.jpg  bar61.jpg  bar92.jpg  foo114.jpg  foo145.jpg  foo176.jpg  foo207.jpg  foo238.jpg  foo269.jpg  foo300.jpg
bar31.jpg  bar62.jpg  bar93.jpg  foo115.jpg  foo146.jpg  foo177.jpg  foo208.jpg  foo239.jpg  foo270.jpg  foo301.jpg
bar32.jpg  bar63.jpg  bar94.jpg  foo116.jpg  foo147.jpg  foo178.jpg  foo209.jpg  foo240.jpg  foo271.jpg  test.sh
bar33.jpg  bar64.jpg  bar95.jpg  foo117.jpg  foo148.jpg  foo179.jpg  foo210.jpg  foo241.jpg  foo272.jpg
bar34.jpg  bar65.jpg  bar96.jpg  foo118.jpg  foo149.jpg  foo180.jpg  foo211.jpg  foo242.jpg  foo273.jpg
bar35.jpg  bar66.jpg  bar97.jpg  foo119.jpg  foo150.jpg  foo181.jpg  foo212.jpg  foo243.jpg  foo274.jpg
bar36.jpg  bar67.jpg  bar98.jpg  foo120.jpg  foo151.jpg  foo182.jpg  foo213.jpg  foo244.jpg  foo275.jpg
bar37.jpg  bar68.jpg  bar99.jpg  foo121.jpg  foo152.jpg  foo183.jpg  foo214.jpg  foo245.jpg  foo276.jpg
bar38.jpg  bar69.jpg  baz1.jpg   foo122.jpg  foo153.jpg  foo184.jpg  foo215.jpg  foo246.jpg  foo277.jpg
bar39.jpg  bar70.jpg  baz2.jpg   foo123.jpg  foo154.jpg  foo185.jpg  foo216.jpg  foo247.jpg  foo278.jpg
bar40.jpg  bar71.jpg  baz3.jpg   foo124.jpg  foo155.jpg  foo186.jpg  foo217.jpg  foo248.jpg  foo279.jpg
```

> окончание проверки
> ---
</details>

# **task 2d:**
Написать скрипт резервного копирования по расписанию следующим образом: В первый день месяца помещать копию в backdir/monthly. Бэкап по пятницам хранить в каталоге backdir/weekley. В остальные дни сохранять копии в backdir/daily. Настроить ротацию следующим образом. Ежемесячные копии хранить 180 дней, ежедневные — неделю, еженедельные — 30 дней. Подсказка: для ротации используйте find.

Создание скрипта и необходимых папок:

```ShellSession
cbpi@cbpi-VirtualBox:~ $ vim bin/backup.sh
...
cbpi@cbpi-VirtualBox:~ $ chmod +x bin/backup.sh
```

```ShellSession
cbpi@cbpi-VirtualBox:~ $ mkdir -p backups/backdir
cbpi@cbpi-VirtualBox:~ $ mkdir backups/backdir/monthly
cbpi@cbpi-VirtualBox:~ $ mkdir backups/backdir/weekly
cbpi@cbpi-VirtualBox:~ $ mkdir backups/backdir/daily
```

Расписание для скрипта:

```
# every first day of the month
0 9 1 * * ~/bin/baskup.sh montly
# every friday
0 9 2-31 * 5 ~/bin/backup.sh weekly
# every other day
0 9 2-31 * 1-4,6 ~/bin/backup.sh daily
# every sunday with rotate
0 9 2-31 * 0 ~/bin/backup.sh --rotate daily
```

<details>
<summary>Скрипт</summary>

```Shell
#!/bin/bash

BACKLIST=~/backups/backlist
BACKDIR=~/backups/backdir

saveit() {
    today=$(date +"%s")
    time_passed=$[($today-$1)/86400]

    [[ $time_passed -ge $2 ]] && return 1

    return 0
}

rotate(){
    for period in $(find $BACKDIR -mindepth 1 -type d | awk -F/ '{print $NF}') 
    do
        case $period in
        daily)
          to_keep=7
        ;;
        weekly)
          to_keep=30
        ;;
        monthly)
          to_keep=180
        ;;
        esac
        cd $period
        for file in *
        do
            [ $file = "*" ] && break
            fday=$(date -d `echo $file | cut -c 1-8` +"%s")
            saveit $fday $to_keep || rm $file
        done
        cd ..
    done
}

pushd . &>/dev/null

cd $BACKDIR
[ "$1"  = "--rotate" ] && shift && rotate
subdir=$1

cd $subdir
cat "$BACKLIST" | xargs find | xargs -d '\n' tar -c |  gzip -9c > $(date +"%Y%m%d")backup.tar.gz

popd &>/dev/null
```

> окончание скрипта
> ---
</details>

<details>
<summary>Проверка</summary>

- Создадим заглушки архивов для проверки ротации:

```ShellSession
cbpi@cbpi-VirtualBox:~ $ cd backups/backdir/daily/
cbpi@cbpi-VirtualBox:~/backups/backdir/daily $ ls
20200110backup.tar.qz  20200114backup.tar.qz  20200118backup.tar.qz  20200122backup.tar.qz
20200111backup.tar.qz  20200115backup.tar.qz  20200119backup.tar.qz  20200123backup.tar.qz
20200112backup.tar.qz  20200116backup.tar.qz  20200120backup.tar.qz
20200113backup.tar.qz  20200117backup.tar.qz  20200121backup.tar.qz
```

```ShellSession
cbpi@cbpi-VirtualBox:~/backups/backdir/daily $ cd ../weekly/
cbpi@cbpi-VirtualBox:~/backups/backdir/weekly $ touch 201912{02,09,16,23,30}backup.tar.qz
cbpi@cbpi-VirtualBox:~/backups/backdir/weekly $ ls
20191202backup.tar.qz  20191216backup.tar.qz  20191230backup.tar.qz
20191209backup.tar.qz  20191223backup.tar.qz
```

```ShellSession
cbpi@cbpi-VirtualBox:~/backups/backdir/weekly $ cd ../monthly/
cbpi@cbpi-VirtualBox:~/backups/backdir/monthly $ touch 2019{01..12}01backup.tar.qz 20200101backup.tar.qz
cbpi@cbpi-VirtualBox:~/backups/backdir/monthly $ ls
20190101backup.tar.qz  20190501backup.tar.qz  20190901backup.tar.qz  20200101backup.tar.qz
20190201backup.tar.qz  20190601backup.tar.qz  20191001backup.tar.qz
20190301backup.tar.qz  20190701backup.tar.qz  20191101backup.tar.qz
20190401backup.tar.qz  20190801backup.tar.qz  20191201backup.tar.qz
```

- Проверка ротации и создания архива:

```ShellSession
cbpi@cbpi-VirtualBox:~ $ backup.sh --rotate daily
tar: Удаляется начальный `/' из имен объектов
tar: Удаляются начальные `/' из целей жестких ссылок
cbpi@cbpi-VirtualBox:~ $ cd backups/backdir/
cbpi@cbpi-VirtualBox:~/backups/backdir $ ls -lA daily/ weekly/ monthly/
daily/:
итого 116416
-rw-rw-r-- 1 cbpi cbpi         0 янв 23 07:57 20200117backup.tar.qz
-rw-rw-r-- 1 cbpi cbpi         0 янв 23 07:57 20200118backup.tar.qz
-rw-rw-r-- 1 cbpi cbpi         0 янв 23 07:57 20200119backup.tar.qz
-rw-rw-r-- 1 cbpi cbpi         0 янв 23 07:57 20200120backup.tar.qz
-rw-rw-r-- 1 cbpi cbpi         0 янв 23 07:57 20200121backup.tar.qz
-rw-rw-r-- 1 cbpi cbpi         0 янв 23 07:57 20200122backup.tar.qz
-rw-rw-r-- 1 cbpi cbpi 119207395 янв 23 10:47 20200123backup.tar.gz

monthly/:
итого 0
-rw-rw-r-- 1 cbpi cbpi 0 янв 23 08:05 20190801backup.tar.qz
-rw-rw-r-- 1 cbpi cbpi 0 янв 23 08:05 20190901backup.tar.qz
-rw-rw-r-- 1 cbpi cbpi 0 янв 23 08:05 20191001backup.tar.qz
-rw-rw-r-- 1 cbpi cbpi 0 янв 23 08:05 20191101backup.tar.qz
-rw-rw-r-- 1 cbpi cbpi 0 янв 23 08:05 20191201backup.tar.qz
-rw-rw-r-- 1 cbpi cbpi 0 янв 23 08:05 20200101backup.tar.qz

weekly/:
итого 0
-rw-rw-r-- 1 cbpi cbpi 0 янв 23 08:06 20191230backup.tar.qz
```

- Архив успешно создался, *просроченные* заглушки архивов успещно удалены.

```ShellSession
cbpi@cbpi-VirtualBox:~ $ cat backups/backlist
/home/cbpi/Изображения
/home/cbpi/Документы
cbpi@cbpi-VirtualBox:~ $ tar -tf backups/backdir/daily/20200123backup.tar.gz | head
home/cbpi/Изображения/
home/cbpi/Изображения/Zalevskij75 - 44.jpg
home/cbpi/Изображения/space (49).jpg
home/cbpi/Изображения/Kosmos022.jpg
home/cbpi/Изображения/w_9a0d6399.jpg
home/cbpi/Изображения/space (60).jpg
home/cbpi/Изображения/space (143).jpg
home/cbpi/Изображения/Космос (78).jpeg
home/cbpi/Изображения/ok9M9cI8WRI.jpg
home/cbpi/Изображения/Kosmos003.jpg
```

> окончание проверки
> ---
</details>

