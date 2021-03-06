# **task 1:**
Написать регулярное выражение, которое проверяет валидный IP-адрес. Например,
**192.168.1.1** подойдет, а **256.300.1.1** — нет.

Каждый октет IP-адреса может принимать значение от **0** до **255**.

Разобьем возможные значения октета на три диапазона (по убыванию):
- от **255**  до **250**, данному диапазону будет соответствовать: `25[0-5]`
- от **249** до **200**, данному диапазону будет соответствовать: `2[0-4][0-9]` или `2[0-4]\d`
- от **199** до **0**, данному диапазону будет соответствовать: `[01]?[0-9]{1,2}` или `[01]?\d{1,2}`

Таким образом получим регулярное выражение для одного октета:

```regex
(25[0-5]|2[0-4][0-9]|[01]?[0-9]{1,2})
```

Однако, данное регулярное выражение будет отмечать такие октеты как: `006`, `06`, а не только `6`
Если нам необходимо отмечать октеты без нулей перед числом, то регулярное выражение для октета примет вид:

```regex
(25[0-5]|2[0-4][0-9]|1?[1-9]?[0-9])
```

- Для указания символа разделителя октетов используем: `\.`
- Для ограничения начала и окончания шаблона используем: `\b`

Получим следующее выражение для IP-адреса (POSIX совместимое):

   С включением нулей перед числом в октете:

```regex
\b((25[0-5]|2[0-4][0-9]|[01]?[0-9]{1,2})\.){3}(25[0-5]|2[0-4][0-9]|[01]?[0-9]{1,2})\b
```

   Без включения нулей перед числом в октете:

```regex
\b((25[0-5]|2[0-4][0-9]|1?[1-9]?[0-9])\.){3}(25[0-5]|2[0-4][0-9]|1?[1-9]?[0-9])\b
```

Можно немного сократить выражение с помощью использования **subpattern** и замены `[0-9]` на `\d` (PCRE):

   С включением нулей перед числом в октете:

```regex
\b(25[0-5]|2[0-4]\d|[01]?\d{1,2})\.(?1)\.(?1)\.(?1)\b
\b(25[0-5]|2[0-4]\d|[01]?\d{1,2})(\.(?1)){3}\b
```

   Без включения нулей перед числом в октете:

```regex
\b(25[0-5]|2[0-4][0-9]|1?[1-9]?\d)\.(?1)\.(?1)\.(?1)\b
\b(25[0-5]|2[0-4][0-9]|1?[1-9]?\d)(\.(?1)){3}\b
```

Можно также использовать именованные группы (PCRE):

   С включением нулей перед числом в октете:

```regex
\b(?<okt>25[0-5]|2[0-4]\d|[01]?\d{1,2})\.(?&okt)\.(?&okt)\.(?&okt)\b
\b(?<okt>25[0-5]|2[0-4]\d|[01]?\d{1,2})(\.(?&okt)){3}\b
\b((?P<okt>(25[0-5]|2[0-4]\d|[01]?\d{1,2}))\.){3}(?P=okt)\b
\b(?P<okt>25[0-5]|2[0-4]\d|[01]?\d{1,2})\.(?P>okt)\.(?P>okt)\.(?P>okt)\b
```

   Без включения нулей перед числом в октете:

```regex
\b(?<okt>25[0-5]|2[0-4][0-9]|1?[1-9]?\d)\.(?&okt)\.(?&okt)\.(?&okt)\b
\b(?<okt>25[0-5]|2[0-4][0-9]|1?[1-9]?\d)(\.(?&okt)){3}\b
\b((?P<okt>(25[0-5]|2[0-4][0-9]|1?[1-9]?\d))\.){3}(?P=okt)\b
\b(?P<okt>25[0-5]|2[0-4][0-9]|1?[1-9]?\d)\.(?P>okt)\.(?P>okt)\.(?P>okt)\b
```

Однако, использование `\b` в шаблоне обеспечит совпадение со следующими, некорретными на мой взгляд значениями:

```
/192.168.2.1
192.168.0.0/24 # конечно тоже IP, но все-таки это обозначение сети, а не хоста
192.168.1.1.1
192.192.168.1.1
```

Т.е. `\b` включает в себя нежелательные символы, например `/` или `.`

Попробуем исключить эти значения из совпадений:

- Для ограничения начала шаблона используем позитивный поиск назад: `(?<=[^\S]|^|[=:])`  
   `([^\S]|^)` - в позитивном поиске назад это тоже самое, как если бы мы использовали негативный поиск назад, т.е. `(?<!\S)`  
   Таким образом мы проверяем шаблон IP адреса только в том случае, если это начало слова (больше ограничений чем `\b`)  
   `([=:])` - указываем в позитивном поиске назад, чтобы позволить наличие символов `=` или `:` перед шаблоном.  
   Для POSIX аналогом выражения `(?<=[^\S]|^|[=:])` в начале шаблона будет: `(^|[\r|\n|\t|\f|\v =:])` однако, в отличие от поиска назад - найденный символ будет включен в шаблон.

- Для ограничения окончания шаблона используем негативный поиск вперед: `(?!\S)`  
   `(\S)` - в негативном поиске вперед это тоже самое, как если бы мы использовали позитивный поиск вперед, т.е. `(?=[^\S]|$)`   
   Однако, в отличие от ограничения начала шаблона, в его окончании нам не требуется включение дополнительных символов, поэтому можем оставить негативный вариант.  
   Для POSIX аналогом выражения `(?!\S)` в конце шаблона будет: `($|[\r\n\t\f\v ])` однако, в отличие от поиска вперед - найденный символ будет включен в шаблон.

Получим итоговый шаблон (PCRE):

   С включением нулей перед числом в октете:

```regex
(?<=[^\S]|^|[=:])(25[0-5]|2[0-4]\d|[01]?\d{1,2})\.(?1)\.(?1)\.(?1)(?!\S)
```

   Без включения нулей перед числом в октете:

```regex
(?<=[^\S]|^|[=:])(25[0-5]|2[0-4][0-9]|1?[1-9]?\d)\.(?1)\.(?1)\.(?1)(?!\S)
```

<details>
  <summary>Проверка</summary>

   - Файл для проверки:

```ShellSession
cbpi@cbpi-VirtualBox:~/l4/t1 $ cat ipCheck 
192.168.1.1
192.168.1.2 and 192.168.1.3

/192.168.2.1
a192.168.2.1
192.168.2.1a
192.168.0.0/24

192.168.1.1.1
192.192.168.1.1
255.250.249.209
256.300.1.1
1192.168.1.1
192.168.1.2550
192.168.1000.1

От 10.0.0.0 до 10.255.255.2
От 172.16.0.0 до 172.31.255.255
От 192.168.0.0 до 192.168.255.255
От 100.64.0.0 до 100.127.255.255

ip=5.35.10.235
ip:87.228.94.66

006.66.189.184
06.66.189.184
000.66.189.184
95.034.255.40
111.05.218.217
111.005.218.217
161.148.053.172
161.148.003.172
161.148.03.172
161.148.153.072
161.148.153.02
161.148.153.002
```

   - Проверка с помощью **sed** (POSIX):

```ShellSession
cbpi@vault_rpi:~/geekbrains/linux/l4$ cat ipCheck | tr ' ' '\n' | sed -nE 's/(^|^.*[=:])(((25[0-5]|2[0-4][0-9]|[01]?[0-9]{1,2})\.){3}(25[0-5]|2[0-4][0-9]|[01]?[0-9]{1,2}))$/\2/p'
192.168.1.1
192.168.1.2
192.168.1.3
255.250.249.209
10.0.0.0
10.255.255.2
172.16.0.0
172.31.255.255
192.168.0.0
192.168.255.255
100.64.0.0
100.127.255.255
5.35.10.235
87.228.94.66
006.66.189.184
06.66.189.184
000.66.189.184
95.034.255.40
111.05.218.217
111.005.218.217
161.148.053.172
161.148.003.172
161.148.03.172
161.148.153.072
161.148.153.02
161.148.153.002
cbpi@vault_rpi:~/geekbrains/linux/l4$ cat ipCheck | tr ' ' '\n' | sed -nE 's/(^|^.*[=:])(((25[0-5]|2[0-4][0-9]|1?[1-9]?[0-9])\.){3}(25[0-5]|2[0-4][0-9]|1?[1-9]?[0-9]))$/\2/p'
192.168.1.1
192.168.1.2
192.168.1.3
255.250.249.209
10.0.0.0
10.255.255.2
172.16.0.0
172.31.255.255
192.168.0.0
192.168.255.255
5.35.10.235
87.228.94.66
```

```ShellSession
cbpi@vault_rpi:~/geekbrains/linux/l4$ okt="(25[0-5]|2[0-4][0-9]|[01]?[0-9]{1,2})"
cbpi@vault_rpi:~/geekbrains/linux/l4$ cat ipCheck | tr ' ' '\n' | sed -nE "s/(^|^.*[=:])((${okt}\.){3}${okt})$/\2/p"         
192.168.1.1
192.168.1.2
192.168.1.3
255.250.249.209
10.0.0.0
10.255.255.2
172.16.0.0
172.31.255.255
192.168.0.0
192.168.255.255
100.64.0.0
100.127.255.255
5.35.10.235
87.228.94.66
006.66.189.184
06.66.189.184
000.66.189.184
95.034.255.40
111.05.218.217
111.005.218.217
161.148.053.172
161.148.003.172
161.148.03.172
161.148.153.072
161.148.153.02
161.148.153.002
cbpi@vault_rpi:~/geekbrains/linux/l4$ okt="(25[0-5]|2[0-4][0-9]|1?[1-9]?[0-9])"
cbpi@vault_rpi:~/geekbrains/linux/l4$ cat ipCheck | tr ' ' '\n' | sed -nE "s/(^|^.*[=:])((${okt}\.){3}${okt})$/\2/p"
192.168.1.1
192.168.1.2
192.168.1.3
255.250.249.209
10.0.0.0
10.255.255.2
172.16.0.0
172.31.255.255
192.168.0.0
192.168.255.255
5.35.10.235
87.228.94.66
```

   - Проверка с помощью **grep** (PCRE):

```ShellSession
cbpi@vault_rpi:~/geekbrains/linux/l4$ grep -oP '(?<=[^\S]|^|[=:])(25[0-5]|2[0-4]\d|[01]?\d{1,2})\.(?1)\.(?1)\.(?1)(?!\S)' ipCheck
192.168.1.1
192.168.1.2
192.168.1.3
255.250.249.209
10.0.0.0
10.255.255.2
172.16.0.0
172.31.255.255
192.168.0.0
192.168.255.255
100.64.0.0
100.127.255.255
5.35.10.235
87.228.94.66
006.66.189.184
06.66.189.184
000.66.189.184
95.034.255.40
111.05.218.217
111.005.218.217
161.148.053.172
161.148.003.172
161.148.03.172
161.148.153.072
161.148.153.02
161.148.153.002
cbpi@vault_rpi:~/geekbrains/linux/l4$ grep -oP '(?<=[^\S]|^|[=:])(25[0-5]|2[0-4]\d|1?[1-9]?\d)\.(?1)\.(?1)\.(?1)(?!\S)' ipCheck
192.168.1.1
192.168.1.2
192.168.1.3
255.250.249.209
10.0.0.0
10.255.255.2
172.16.0.0
172.31.255.255
192.168.0.0
192.168.255.255
5.35.10.235
87.228.94.66
```

```ShellSession
cbpi@vault_rpi:~/geekbrains/linux/l4$ okt="(25[0-5]|2[0-4]\d|[01]?\d{1,2})"
cbpi@vault_rpi:~/geekbrains/linux/l4$ grep -oP '(?<=[^\S]|^|[=:])('$okt'\.){3}'$okt'(?!\S)' ipCheck
192.168.1.1
192.168.1.2
192.168.1.3
255.250.249.209
10.0.0.0
10.255.255.2
172.16.0.0
172.31.255.255
192.168.0.0
192.168.255.255
100.64.0.0
100.127.255.255
5.35.10.235
87.228.94.66
006.66.189.184
06.66.189.184
000.66.189.184
95.034.255.40
111.05.218.217
111.005.218.217
161.148.053.172
161.148.003.172
161.148.03.172
161.148.153.072
161.148.153.02
161.148.153.002
cbpi@vault_rpi:~/geekbrains/linux/l4$ okt="(25[0-5]|2[0-4]\d|1?[1-9]?\d)"
cbpi@vault_rpi:~/geekbrains/linux/l4$ grep -oP '(?<=[^\S]|^|[=:])('$okt'\.){3}'$okt'(?!\S)' ipCheck
192.168.1.1
192.168.1.2
192.168.1.3
255.250.249.209
10.0.0.0
10.255.255.2
172.16.0.0
172.31.255.255
192.168.0.0
192.168.255.255
5.35.10.235
87.228.94.66
```

   - Проверка с помощью **gawk**

```ShellSession
cbpi@vault_rpi:~/geekbrains/linux/l4$ ip_pattern='((25[0-5]|2[0-4][0-9]|[01]?[0-9]{1,2})\.){3}(25[0-5]|2[0-4][0-9]|[01]?[0-9]{1,2})'
cbpi@vault_rpi:~/geekbrains/linux/l4$ cat ipCheck | tr ' ' '\n' | gawk 'match($0,/(^|^.*[=:])('$ip_pattern')$/, arr) {print arr[2]}'
192.168.1.1
192.168.1.2
192.168.1.3
255.250.249.209
10.0.0.0
10.255.255.2
172.16.0.0
172.31.255.255
192.168.0.0
192.168.255.255
100.64.0.0
100.127.255.255
5.35.10.235
87.228.94.66
006.66.189.184
06.66.189.184
000.66.189.184
95.034.255.40
111.05.218.217
111.005.218.217
161.148.053.172
161.148.003.172
161.148.03.172
161.148.153.072
161.148.153.02
161.148.153.002
cbpi@vault_rpi:~/geekbrains/linux/l4$ ip_pattern='((25[0-5]|2[0-4][0-9]|1?[1-9]?[0-9])\.){3}(25[0-5]|2[0-4][0-9]|1?[1-9]?[0-9])'
cbpi@vault_rpi:~/geekbrains/linux/l4$ cat ipCheck | tr ' ' '\n' | gawk 'match($0,/(^|^.*[=:])('$ip_pattern')$/, arr) {print arr[2]}'
192.168.1.1
192.168.1.2
192.168.1.3
255.250.249.209
10.0.0.0
10.255.255.2
172.16.0.0
172.31.255.255
192.168.0.0
192.168.255.255
5.35.10.235
87.228.94.66
```

   - Проверка с помощью **find** (POSIX):

```ShellSession
cbpi@vault_rpi:~/geekbrains/linux/l4$ mkdir findIp; cd findIp
cbpi@vault_rpi:~/geekbrains/linux/l4/findIp$ touch {5,10,192,255,256,900}.1.1.1
cbpi@vault_rpi:~/geekbrains/linux/l4/findIp$ ls
10.1.1.1  192.1.1.1  255.1.1.1  256.1.1.1  5.1.1.1  900.1.1.1
cbpi@vault_rpi:~/geekbrains/linux/l4/findIp$ okt="(25[0-5]|2[0-4][0-9]|[01]?[0-9]{1,2})"
cbpi@vault_rpi:~/geekbrains/linux/l4/findIp$ find . -regextype egrep -regex ".*/(${okt}\.){3}${okt}" | cut -d/ -f2 | sort -n
5.1.1.1
10.1.1.1
192.1.1.1
255.1.1.1
```
> окончание проверки
> ---
</details>

# **task 2:**
Написать регулярное выражение, которое проверяет, является ли указанный файл файлом нужного
типа (на выбор .com,.exe или .jpg,.png,.gif и т.д.). Написать регулярное выражение для
проверки, ведет ли ссылка URL на некоторый файл, и это действительно ссылка на картинку
(например, http://site.com/folder/1.png), а не на любой файл.

- Регулярное выражение, которое проверяет, является ли указанный файл картинкой (**.jpg**,**.jpeg**,**.png**,**.gif**,**.tif**,**.tiff**,**.bmp**)

   Предположим, что имя файла состоит из символов: `a-zA-Z0-9_.-`  
   Тогда шаблон для имени будет: `[\w.-]+`

   Шаблон для расширения файла будет: `\.(jpe?g|png|gif|tiff?|bmp)`

   Для ограничения начала шаблона используем: `\b`  
   Для ограничения окончания шаблона используем: `(?!\S)`

Получим итоговый шаблон:

```regex
\b[\w.-]+\.(jpe?g|png|gif|tiff?|bmp)(?!\S)
```

<details>
  <summary>Проверка</summary>

   - Файл для проверки:

```ShellSession
cbpi@vault_rpi:~/geekbrains/linux/l4$ cat extensionCheck
file.png
file.exe

file.jpeg
file.bin

file.jpg
file.sh

file.bmp
file.inf

file.tif
file.bat

file.tiff
file.run

file.gif
file.out

file.out.png
file.log.bmp

file-1.png and file.1.bmp
file-2..png and file.bmp.bin

file.pngg
file.jpeg1
file.jpg.exe
file.tifff
```

   - Проверка с помощью **sed** (POSIX):

```ShellSession
cbpi@vault_rpi:~/geekbrains/linux/l4$ cat extensionCheck | tr ' ' '\n' | sed -nE '/^(\w|[.-])+\.(jpe?g|png|gif|tiff?|bmp)$/p'
file.png
file.jpeg
file.jpg
file.bmp
file.tif
file.tiff
file.gif
file.out.png
file.log.bmp
file-1.png
file.1.bmp
file-2..png
```

   - Проверка с помощью **grep**:

```ShellSession
cbpi@vault_rpi:~/geekbrains/linux/l4$ grep -oP '\b[\w.-]+\.(jpe?g|png|gif|tiff?|bmp)(?!\S)' extensionCheck
file.png
file.jpeg
file.jpg
file.bmp
file.tif
file.tiff
file.gif
file.out.png
file.log.bmp
file-1.png
file.1.bmp
file-2..png
```

   - Проверка с помощью **awk**:
```ShellSession
cbpi@vault_rpi:~/geekbrains/linux/l4$ cat extensionCheck | tr ' ' '\n' | awk 'BEGIN {print "Files with correct extension:"} /^(\w|[.-])+\.(jpe?g|png|gif|tiff?|bmp)$/ {print $1; ++n} END {print "Totall " n " files"}'
Files with correct extension:
file.png
file.jpeg
file.jpg
file.bmp
file.tif
file.tiff
file.gif
file.out.png
file.log.bmp
file-1.png
file.1.bmp
file-2..png
Totall 12 files
```

   - Проверка с помощью **find**:

```ShellSession
cbpi@vault_rpi:~/geekbrains/linux/l4$ mkdir files; cd files
cbpi@vault_rpi:~/geekbrains/linux/l4/files$ grep -oP '\b[\w.-]+\.\w+(?!\S)' ../extensionCheck | xargs touch
cbpi@vault_rpi:~/geekbrains/linux/l4/files$ ls
file.1.bmp   file.bat  file.bmp.bin  file.inf    file.jpg      file.out      file.pngg  file.tif
file-1.png   file.bin  file.exe      file.jpeg   file.jpg.exe  file.out.png  file.run   file.tiff
file-2..png  file.bmp  file.gif      file.jpeg1  file.log.bmp  file.png      file.sh    file.tifff
cbpi@vault_rpi:~/geekbrains/linux/l4/files$ find . -maxdepth 1 -regextype egrep -regex '\./(\w|[.-])+\.(jpe?g|png|gif|tiff?|bmp)' | cut -d/ -f2 | sort
file.1.bmp
file-1.png
file-2..png
file.bmp
file.gif
file.jpeg
file.jpg
file.log.bmp
file.out.png
file.png
file.tif
file.tiff
```

> окончание проверки
> ---
</details>

- Регулярное выражение для проверки, ведет ли ссылка URL на картинку.

   Прежде всего, в URL адресе может быть **http://** или **https://** шаблон: `(https?:\/\/)?`

   Для **www** и доменов нижних уровней используем шаблон: `([\w-]+\.)+`

   Для доменов верхних уровней используем шаблон: `[a-z]+`

   Для пути к файлу используем шаблон: `(\/[\w.-]+)*\/`

   Для самого файла используем шаблон: `[\w.-]+\.(jpe?g|png|gif|tiff?|bmp)`

   Для ограничения начала шаблона используем: `(?<!\S)`  
   Для ограничения окончания шаблона используем: `(?!\S)`   
   
Получим итоговый шаблон:

```regex
(?<!\S)(https?:\/\/)?([\w-]+\.)+[a-z]+(\/[\w.-]+)*\/[\w.-]+\.(jpe?g|png|gif|tiff?|bmp)(?!\S)
```

<details>
  <summary>Проверка</summary>

   - Файл для проверки:

```ShellSession
cbpi@vault_rpi:~/geekbrains/linux/l4$ cat links
http://site.com/1.png

https://site.com.ru/some-path/2.bmp

www.site-site.more.org/dir1/dir2/3.jpeg

https://www.another_site.ru/.bin.png

bit.ly/00a0sa0sasaxsdd.png

http://bit.ly/00a0sa0sasaxsdd.exe

https://bit.ly/00a0sa0sasaxsdd.gif.not

http://www.terra.es/asasa.bin

hhttps://www.terra.es.net/asasa.gif

https:///www.terra.es.com/dir/dir/picture.jpg

https://www.terra2.es.com/
```

   - Проверка с помощью **grep**:

```ShellSession
cbpi@vault_rpi:~/geekbrains/linux/l4$ grep -oP '(?<!\S)(https?:\/\/)?([\w-]+\.)+[a-z]+(\/[\w.-]+)*\/[\w.-]+\.(jpe?g|png|gif|tiff?|bmp)(?!\S)' links
http://site.com/1.png
https://site.com.ru/some-path/2.bmp
www.site-site.more.org/dir1/dir2/3.jpeg
https://www.another_site.ru/.bin.png
bit.ly/00a0sa0sasaxsdd.png
```

```ShellSession
cbpi@vault_rpi:~/geekbrains/linux/l4$ url='(https?:\/\/)?([\w-]+\.)+[a-z]+(\/[\w.-]+)*\/'
cbpi@vault_rpi:~/geekbrains/linux/l4$ pict='[\w.-]+\.(jpe?g|png|gif|tiff?|bmp)(?!\S)'
cbpi@vault_rpi:~/geekbrains/linux/l4$ grep -oP '(?<!\S)'$url$pict'(?!\S)' links
http://site.com/1.png
https://site.com.ru/some-path/2.bmp
www.site-site.more.org/dir1/dir2/3.jpeg
https://www.another_site.ru/.bin.png
bit.ly/00a0sa0sasaxsdd.png
```

```ShellSession
cbpi@vault_rpi:~/geekbrains/linux/l4$ url='(https?:\/\/)?([\w-]+\.)+[a-z]+(\/[\w.-]+)*\/'
cbpi@vault_rpi:~/geekbrains/linux/l4$ pict='[\w.-]+\.(jpe?g|png|gif|tiff?|bmp)(?!\S)'
cbpi@vault_rpi:~/geekbrains/linux/l4$ grep -oP '(?<!\S)'$url$pict'(?!\S)' links | awk -F/ '{print $NF}'
1.png
2.bmp
3.jpeg
.bin.png
00a0sa0sasaxsdd.png
```

> окончание проверки
> ---
</details>

# **task 3:**
Написать регулярное выражение, которое проверяет, является выведенное значение «белым»  IP-адресом (5.255.255.5 подойдет, а 127.16.0.1 — нет).

Для исключения частных и иных зарезервированных IP адресов используем негативный поиск вперед. 

- Определим какие диапазоны IP адресов следует исключить и составим для них регулярные выражения:

   0.0.0.0/8: Current network - `0\.`  
   10.0.0.0/8: Private network - `10\.`  
   100.64.0.0/10: Shared Address Space - `100\.(12[0-7]|1[01]\d|[7-9]\d|6[4-9])`  
   127.0.0.0/8: Loopback - `127\.`  
   169.254.0.0/16: Link-local - `169\.254`  
   172.16.0.0/12: Private network  - `172\.(3[01]|2\d|1[6-9])`  
   192.0.0.0/24: IETF Protocol Assignments - `192\.0\.0`  
   192.0.2.0/24: TEST-NET-1, documentation and examples - `192\.0\.2`  
   192.88.99.0/24: IPv6 to IPv4 relay - `192\.88\.99`  
   192.168.0.0/16: Private network - `192\.168`  
   198.18.0.0/15: Network benchmark tests - `198\.1[89]`  
   198.51.100.0/24: TEST-NET-2, documentation and examples - `198\.51\.100`  
   203.0.113.0/24: TEST-NET-3, documentation and examples - `203\.0\.113`  
   224.0.0.0/4: IP multicast (former Class D network) - `(22[4-9]|23\d)\.`  
   240.0.0.0/4: Reserved (former Class E network) - `(24\d|25[0-5])\.`  
   255.255.255.255: Broadcast - `255\.`

Источник сведений по частным IP: [Wikipedia](https://www.wikiwand.com/en/IPv4#/Special-use_addresses)

- Объединим некоторые из шаблонов:

   `0\.` и `10\.` и `127\.` и `(22[4-9]|23\d)\.` и `(24\d|25[0-5])\.` и `255\.`:  
   `(25[0-5]|2[34]\d|22[4-9]|127|1?0)\.`

   `192\.0\.0` и `192\.0\.2`: `192\.0\.[02]`

Получим следующее регулярное выражение для исключения определенных диапазонов IP адресов:

```regex
(?!(25[0-5]|2[34]\d|22[4-9]|127|1?0)\.|203\.0\.113|198\.51\.100|198\.1[89]|192\.168|192\.0\.[02]|172\.(3[01]|2\d|1[6-9])|169\.254|100\.(12[0-7]|1[01]\d|[7-9]\d|6[4-9]))
```

В качестве регулярного выражения для IP адреса используем выражение из **task 1**:

```regex
(?<okt>25[0-5]|2[0-4]\d|[01]?\d{1,2})(\.(?&okt)){3}
```

Получим итоговое регулярное выражение для проверки белых IP адресов:

```regex
(?!(25[0-5]|2[34]\d|22[4-9]|127|1?0)\.|203\.0\.113|198\.51\.100|198\.1[89]|192\.168|192\.0\.[02]|172\.(3[01]|2\d|1[6-9])|169\.254|100\.(12[0-7]|1[01]\d|[7-9]\d|6[4-9]))(?<okt>25[0-5]|2[0-4]\d|[01]?\d{1,2})(\.(?&okt)){3}
```
   
<details>
  <summary>Проверка</summary>

   - Файл для проверки:

```ShellSession
cbpi@vault_rpi:~/geekbrains/linux/l4$ cat whiteIpCheck
255.255.255.255
235.255.255.255
224.255.255.255
223.0.0.0

127.10.10.10
126.10.10.10

10.0.0.0
11.0.0.0

0.10.10.10
1.10.10.10

203.0.113.10
203.1.113.10

198.51.100.0
198.51.101.0

198.18.0.0
198.19.0.0
198.20.0.0

192.168.1.1
192.169.1.1

192.0.0.1
192.0.2.1
192.0.1.1

172.31.0.0
172.25.0.0
172.16.0.0
172.15.1.1

169.254.0.0
169.255.1.1

100.64.0.0
100.89.0.0
100.127.0.0
100.128.1.1

127.16.0.1
5.255.255.5
```

   - Проверка с помощью **grep**:

```ShellSession
cbpi@vault_rpi:~/geekbrains/linux/l4$ grep -oP '^(?!(25[0-5]|2[34]\d|22[4-9]|127|1?0)\.|203\.0\.113|198\.51\.100|198\.1[89]|192\.168|192\.0\.[02]|172\.(3[01]|2\d|1[6-9])|169\.254|100\.(12[0-7]|1[01]\d|[7-9]\d|6[4-9]))(?<okt>25[0-5]|2[0-4]\d|[01]?\d{1,2})(\.(?&okt)){3}$' whiteIpCheck
223.0.0.0
126.10.10.10
11.0.0.0
1.10.10.10
203.1.113.10
198.51.101.0
198.20.0.0
192.169.1.1
192.0.1.1
172.15.1.1
169.255.1.1
100.128.1.1
5.255.255.5
```

```ShellSession
cbpi@vault_rpi:~/geekbrains/linux/l4$ not_reserved='(?!(25[0-5]|2[34]\d|22[4-9]|127|1?0)\.|203\.0\.113|198\.51\.100|198\.1[89]|192\.168|192\.0\.[02]|172\.(3[01]|2\d|1[6-9])|169\.254|100\.(12[0-7]|1[01]\d|[7-9]\d|6[4-9]))'
cbpi@vault_rpi:~/geekbrains/linux/l4$ ip_all='(?<okt>25[0-5]|2[0-4]\d|[01]?\d{1,2})(\.(?&okt)){3}'
cbpi@vault_rpi:~/geekbrains/linux/l4$ grep -oP "^${not_reserved}${ip_all}$" whiteIpCheck
223.0.0.0
126.10.10.10
11.0.0.0
1.10.10.10
203.1.113.10
198.51.101.0
198.20.0.0
192.169.1.1
192.0.1.1
172.15.1.1
169.255.1.1
100.128.1.1
5.255.255.5
```

   - Проверка с помощью **awk**:

```ShellSession
cbpi@vault_rpi:~/geekbrains/linux/l4$ not_reserved='(25[0-5]|2[34][0-9]|22[4-9]|127|1?0)\.|203\.0\.113|198\.51\.100|198\.1[89]|192\.168|192\.0\.[02]|172\.(3[01]|2[0-9]|1[6-9])|169\.254|100\.(12[0-7]|1[01][0-9]|[7-9][0-9]|6[4-9])'
cbpi@vault_rpi:~/geekbrains/linux/l4$ ip_pattern='((25[0-5]|2[0-4][0-9]|[01]?[0-9]{1,2})\.){3}(25[0-5]|2[0-4][0-9]|[01]?[0-9]{1,2})'
cbpi@vault_rpi:~/geekbrains/linux/l4$ awk '/'$ip_pattern'/ {if ($1 !~ /^'$not_reserved'.*$/) {print $1}}' whiteIpCheck
223.0.0.0
126.10.10.10
11.0.0.0
1.10.10.10
203.1.113.10
198.51.101.0
198.20.0.0
192.169.1.1
192.0.1.1
172.15.1.1
169.255.1.1
100.128.1.1
5.255.255.5
```

> окончание проверки
> ---
</details>

# **task 4:**
Написать регулярное выражение, которое проверяет, что файл в URL (например, https://site.ru/folder/download/test.docx) не обладает неким расширением (например .exe не пройдет, или .sh — не пройдет. Выбор списка исключенных расширений за вами).

   Для проверки URL используем регулярное выражение из **task 2*:

```regex
(https?:\/\/)?([\w-]+\.)+[a-z]+(\/[\w.-]+)*\/
```

   Модифицируем выражение для включения в путь до файла знаков `=` и `?`:

```regex
(https?:\/\/)?([\w-]+\.)+[a-z]+(\/[\w.=?-]+)*\/
```

   Для проверки наличия файла в URL используем регулярное выражение:

```regex
[\w.+-]+\.\w+
```

   Для исключения списка расширений **exe, bin, sh** используем негативный поиск назад:

```regex
(?<!\.exe|bin|sh)
```

   Для ограничения начала шаблона используем: `(?<!\S)`  
   Для ограничения окончания шаблона используем: `(?!\S)`

Получим итоговый шаблон:

```regex
(?<!\S)(https?:\/\/)?([\w-]+\.)+[a-z]+(\/[\w.=?-]+)*\/[\w.+-]+\.\w+(?<!\.exe|bin|sh)(?!\S)
```

<details>
  <summary>Проверка</summary>

   - Файл для проверки:

```ShellSession
cbpi@vault_rpi:~/geekbrains/linux/l4$ cat > moreLinks
http://site.com/1.png.bin
https://site.com.ru/some-path/2.bmp.exe
www.site-site.more.org/dir1/dir2/3.jpeg.sh
https://www.another_site.ru/4.bin.png
bit.ly/00a0sa0sasaxsdd.png
http://bit.ly/00a0sa0sasaxsdd.docx
https://bit.ly/00a0sa0sasaxsdd.gif.not
http://www.terra.es/asasa.bin.jpg
hhttps://www.terra.es.net/asasa.gif
https:///www.terra.es.com/dir/dir/picture.jpg
https://www.terra2.es.com/
http://example.com/admin/login/?next=/admin/widgets.exe5
http://example.com/admin/login/?next=/admin/animation.jpg
http://example.com/admin/login/?next=/admin/sites/site/change_form.jpg
http://example.com/admin/login/?next=/admin/sites/site/core.jpg
http://example.com/static/admin/img/Open+Sans_400_normal.gif
http://example.com/static/admin/img/Open+Sans_700_normal.gif3
http://example.com/static/admin/js/Open+Sans_700_normal.giff
https://example.com/admin/login/?next=/admin/search.phpr
https://example.com/admin/login/?next=/admin/sorting-icons.php
https://example.com/admin/login/?next=/admin/sites/site/tooltag-add.php
https://example.com/admin/login/?next=/admin/sites/site/widgets.php
https://example.com/Open+Sans_400_normal.gadget
https://example.com/login/Open+Sans_700_normal.gadget3
https://example.com/login/Open+Sans_700_normal.gadgetf
```

   - Проверка с помощью **grep**:

```ShellSession
cbpi@vault_rpi:~/geekbrains/linux/l4$ grep -oP '(?<!\S)(https?:\/\/)?([\w-]+\.)+[a-z]+(\/[\w.=?-]+)*\/[\w.+-]+\.\w+(?<!\.exe|bin|sh)(?!\S)' moreLinks
https://www.another_site.ru/4.bin.png
bit.ly/00a0sa0sasaxsdd.png
http://bit.ly/00a0sa0sasaxsdd.docx
https://bit.ly/00a0sa0sasaxsdd.gif.not
http://www.terra.es/asasa.bin.jpg
http://example.com/admin/login/?next=/admin/widgets.exe5
http://example.com/admin/login/?next=/admin/animation.jpg
http://example.com/admin/login/?next=/admin/sites/site/change_form.jpg
http://example.com/admin/login/?next=/admin/sites/site/core.jpg
http://example.com/static/admin/img/Open+Sans_400_normal.gif
http://example.com/static/admin/img/Open+Sans_700_normal.gif3
http://example.com/static/admin/js/Open+Sans_700_normal.giff
https://example.com/admin/login/?next=/admin/search.phpr
https://example.com/admin/login/?next=/admin/sorting-icons.php
```

```ShellSession
cbpi@vault_rpi:~/geekbrains/linux/l4$ url='(https?:\/\/)?([\w-]+\.)+[a-z]+(\/[\w.=?-]+)*\/[\w.+-]+\.\w+'
cbpi@vault_rpi:~/geekbrains/linux/l4$ grep -oP '(?<!\S)'$url'(?<!\.exe|bin|sh)(?!\S)' moreLinks
https://www.another_site.ru/4.bin.png
bit.ly/00a0sa0sasaxsdd.png
http://bit.ly/00a0sa0sasaxsdd.docx
https://bit.ly/00a0sa0sasaxsdd.gif.not
http://www.terra.es/asasa.bin.jpg
http://example.com/admin/login/?next=/admin/widgets.exe5
http://example.com/admin/login/?next=/admin/animation.jpg
http://example.com/admin/login/?next=/admin/sites/site/change_form.jpg
http://example.com/admin/login/?next=/admin/sites/site/core.jpg
http://example.com/static/admin/img/Open+Sans_400_normal.gif
http://example.com/static/admin/img/Open+Sans_700_normal.gif3
http://example.com/static/admin/js/Open+Sans_700_normal.giff
https://example.com/admin/login/?next=/admin/search.phpr
https://example.com/admin/login/?next=/admin/sorting-icons.php
https://example.com/admin/login/?next=/admin/sites/site/tooltag-add.php
https://example.com/admin/login/?next=/admin/sites/site/widgets.php
https://example.com/Open+Sans_400_normal.gadget
https://example.com/login/Open+Sans_700_normal.gadget3
https://example.com/login/Open+Sans_700_normal.gadgetf
```

```ShellSession
cbpi@vault_rpi:~/geekbrains/linux/l4$ grep -oP '(?<!\S)'$url'(?<!\.exe|bin|sh)(?!\S)' moreLinks | awk -F/ '{print $NF}'
4.bin.png
00a0sa0sasaxsdd.png
00a0sa0sasaxsdd.docx
00a0sa0sasaxsdd.gif.not
asasa.bin.jpg
widgets.exe5
animation.jpg
change_form.jpg
core.jpg
Open+Sans_400_normal.gif
Open+Sans_700_normal.gif3
Open+Sans_700_normal.giff
search.phpr
sorting-icons.php
tooltag-add.php
widgets.php
Open+Sans_400_normal.gadget
Open+Sans_700_normal.gadget3
Open+Sans_700_normal.gadgetf
```

> окончание проверки
> ---
</details>

# **task 5:**
У вас есть лог log.txt, который содержит запросы на загрузку файлов. Один запрос на одной строке. IP адрес во втором столбце. Имя файла может быть в любом столбце. Столбцы разделены одним или несколькими пробелами. Нужно написать выражение в одну строку, которое выведет список всех IP адресов за исключением **loopback** интерфейсов, с которых запрашивался файл /closeio.html, а также количество таких запросов для каждого адреса. Результат должен быть отсортирован по этому значению. Можно использовать стандартные тулы, которые запустятся на большинстве UNIX системах.

   В первую очередь используем шаблон для поиска всех строк, которые содержат запрос на файл: `\/closeio\.html`

   Затем, исключим строки, у которых во втором столбце **loopback**: `(::1|127\.0\.0\.1)`

   Для подсчета и сортировки по количеству запросов используем: `sort|uniq -c|sort -r`
   
<details>
  <summary>Проверка</summary>

   - Файл для проверки:

```ShellSession
cbpi@vault_rpi:~/geekbrains/linux/l4$ cat log.txt
/index.html   189.125.132.232   404 555
/index.html   191.43.29.55
/closeio.html 32.136.34.207   404 555
/bundle.html  91.40.41.91
/index.html   105.204.145.202  404 555
user1    236.185.24.68 /closeio.html
/closeio.html 127.0.0.1     Chrome/79.0.3945.117
/aboutus.html 127.0.0.1     Chrome/79.0.3945.117
/closeio.html 121.26.153.90
/aboutus.html 179.212.146.225
/closeio.html 236.185.24.68
/aboutus.html 21.11.134.84
/closeio.html 121.26.153.90     Chrome/79.0.3945.117
/aboutus.html 179.212.146.225       Chrome/79.0.3945.117
/index.html   214.72.226.119
/index.html   220.56.165.59
/closeio.html 250.127.117.33
ergergresr  236.185.24.68 jjnieer ergwegewg w4gw4 gw4g /closeio.html
/aboutus.html 127.0.0.1
/aboutus.html 21.11.134.84
AppleWebKit/537.36 121.26.153.90 /closeio.html
/aboutus.html 179.212.146.225
/bundle.html  53.21.230.126
/bundle.html  253.21.230.126
/closeio.html 127.0.0.1
AppleWebKit/537.36 253.21.230.126 /aboutus.html
/bundle.html  253.21.230.126
/aboutus.html 202.120.204.224
/index.html   98.57.42.56
AppleWebKit/537.36 50.111.244.24 /bundle.html
/closeio.html ::1
/bundle.html  172.13.183.9
/index.html   247.132.62.22
/aboutus.html 174.7.58.16
/index.html   ::1
/index.html   187.123.93.82
ivan_haski   127.0.0.1             /closeio.html
/closeio.html 163.148.78.78
Jay_navalski   159.158.148.85        /movie.html
```

   - Проверка с помощью **awk**:

```ShellSession
cbpi@vault_rpi:~/geekbrains/linux/l4$ awk '/\/closeio\.html/ {if ($2 !~/(::1|127\.0\.0\.1)/) {print $2|"sort|uniq -c|sort -r"}}' log.txt
      3 236.185.24.68
      3 121.26.153.90
      1 32.136.34.207
      1 250.127.117.33
      1 163.148.78.78
```

> окончание проверки
> ---
</details> 
