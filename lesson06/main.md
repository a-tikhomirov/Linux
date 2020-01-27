# **task 1:**
## **task 1.1:**
Найдите информацию о том, как в Ubuntu открыть порт 80,443. Укажите как.

---
Проверим текущие сетевые правила:

```ShellSession
cbpi@cbpi-VirtualBox:~ $ sudo iptables -L
[sudo] пароль для cbpi:
Chain INPUT (policy ACCEPT)
target     prot opt source               destination

Chain FORWARD (policy ACCEPT)
target     prot opt source               destination

Chain OUTPUT (policy ACCEPT)
target     prot opt source               destination
```

Как мы видим, в данный момент подключение по всем портам разрешено: `Chain INPUT (policy ACCEPT)`

Проверим порты с другой машины:

```ShellSession
cbpi@vault_rpi:~$ nmap 192.168.41.250

Starting Nmap 7.60 ( https://nmap.org ) at 2020-01-24 17:29 MSK
Nmap scan report for 192.168.41.250
Host is up (0.00043s latency).
Not shown: 997 closed ports
PORT    STATE SERVICE
22/tcp  open  ssh
139/tcp open  netbios-ssn
445/tcp open  microsoft-ds

Nmap done: 1 IP address (1 host up) scanned in 0.29 seconds
```

1. Добавим правила разрешающие обмен данными между любыми портами на локальном интерфейсе `lo` (чтобы избежать системных ошибок):  
  `sudo iptables -A INPUT -i lo -j ACCEPT`  
  `sudo iptables -A OUTPUT -o lo -j ACCEPT`
2. Добавим правила разрешающие все пакеты с состоянием `ESTABLISHED` и `RELATED` (запрещать только новые соединения, а пакеты для уже открытых разрешать):  
  `sudo iptables -A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT`
3. Откроем порты `22`, `80` и `443` для протокола TCP:  
  `sudo iptables -A INPUT -p tcp --dport 22 -j ACCEPT`  
  `sudo iptables -A INPUT -p tcp --dport 80 -j ACCEPT`  
  `sudo iptables -A INPUT -p tcp --dport 443 -j ACCEPT`
4. Поменяем политку по умолчанию на `DROP`:  
  `sudo iptables -P INPUT DROP`

Проверим что получилось в итоге:

```ShellSession
cbpi@cbpi-VirtualBox:~ $ sudo iptables -L
Chain INPUT (policy DROP)
target     prot opt source               destination
ACCEPT     all  --  anywhere             anywhere
ACCEPT     all  --  anywhere             anywhere             state RELATED,ESTABLISHED
ACCEPT     tcp  --  anywhere             anywhere             tcp dpt:ssh
ACCEPT     tcp  --  anywhere             anywhere             tcp dpt:http
ACCEPT     tcp  --  anywhere             anywhere             tcp dpt:https

Chain FORWARD (policy ACCEPT)
target     prot opt source               destination

Chain OUTPUT (policy ACCEPT)
target     prot opt source               destination
ACCEPT     all  --  anywhere             anywhere
cbpi@cbpi-VirtualBox:~ $ sudo iptables -nvL
Chain INPUT (policy DROP 65 packets, 2208 bytes)
 pkts bytes target     prot opt in     out     source               destination
    8   752 ACCEPT     all  --  lo     *       0.0.0.0/0            0.0.0.0/0
  217 15483 ACCEPT     all  --  *      *       0.0.0.0/0            0.0.0.0/0            state RELATED,ESTABLISHED
    0     0 ACCEPT     tcp  --  *      *       0.0.0.0/0            0.0.0.0/0            tcp dpt:22
    0     0 ACCEPT     tcp  --  *      *       0.0.0.0/0            0.0.0.0/0            tcp dpt:80
    0     0 ACCEPT     tcp  --  *      *       0.0.0.0/0            0.0.0.0/0            tcp dpt:443

Chain FORWARD (policy ACCEPT 0 packets, 0 bytes)
 pkts bytes target     prot opt in     out     source               destination

Chain OUTPUT (policy ACCEPT 12 packets, 1152 bytes)
 pkts bytes target     prot opt in     out     source               destination
    4   376 ACCEPT     all  --  *      lo      0.0.0.0/0            0.0.0.0/0
```

Проверка с другого хоста:

```ShellSession
cbpi@vault_rpi:~$ nmap 192.168.41.250

Starting Nmap 7.60 ( https://nmap.org ) at 2020-01-24 17:44 MSK
Nmap scan report for 192.168.41.250
Host is up (0.00062s latency).
Not shown: 997 filtered ports
PORT    STATE  SERVICE
22/tcp  open   ssh
80/tcp  closed http
443/tcp closed https

Nmap done: 1 IP address (1 host up) scanned in 4.95 seconds
```

Как мы видим, `997 closed ports` изменилось на `997 filtered ports` и есть потенциально открытые порты `80` и `443`.

## **task 1.2 & 1.3:**
Установить **nginx**, сконфигурировать свой виртуальный хост, используя порт `80`, так, чтобы:
- на запрос клиента с указанным и непустым хидером `User`. отправлять код `200` с текстом `"Hi $user!"`, где `$user` - это значение хидера `User`.
- иначе отправлять код `404` с текстом `"Page not found"`.

---

1. Добавим репозиторий для установки новой стабильной версии:  
  `apt-add-repository ppa:nginx/stable`  
  `apt update`
2. Установим **nginx**:  
  `apt install nginx`
3. Добавим программу в автозагрузку, чтобы она запускалась автоматически:  
  `systemctl enable nginx`
4. Создадим конфигурационный файл для страницы `localhost`:  
  `vim /etc/nginx/sites-available/localhost`  
  `ln -s /etc/nginx/sites-available/localhost /etc/nginx/sites-enabled/simple_test`

```
server{
    listen 80;
    server_name 192.168.41.250 127.0.0.1 localhost;

    add_header User-detected $user_detected always;

    set $user $http_user;

    location / {
        if ($user != ""){
            set $user_detected "True";
            return 200 "Hi, $user!\n";
        }
        if ($user = "") {
            set $user_detected "False";
            return 404 "Page not found\n";
        }
    }
}
```

5. Проверим корректность конфига и перезапустим сервис:  
  `nginx -t`  
  `service nginx reload`

<details>
<summary>Проверка</summary>

- Проверка с другого хоста:

```ShellSession
cbpi@vault_rpi:~$ curl -D - http://192.168.41.250 -H 'User: test'
HTTP/1.1 200 OK
Server: nginx/1.16.1
Date: Fri, 24 Jan 2020 19:33:24 GMT
Content-Type: application/octet-stream
Content-Length: 9
Connection: keep-alive
User-detected: True

Hi, test!
```

```ShellSession
cbpi@vault_rpi:~$ curl -D - http://192.168.41.250 -H 'Host: localhost'
HTTP/1.1 404 Not Found
Server: nginx/1.16.1
Date: Fri, 24 Jan 2020 19:33:41 GMT
Content-Type: application/octet-stream
Content-Length: 15
Connection: keep-alive
User-detected: False

Page not found
```

```ShellSession
cbpi@vault_rpi:~$ curl -D - http://192.168.41.250 -H 'Host: 127.0.0.1' -H 'User: Nameless one'
HTTP/1.1 200 OK
Server: nginx/1.16.1
Date: Fri, 24 Jan 2020 19:37:20 GMT
Content-Type: application/octet-stream
Content-Length: 17
Connection: keep-alive
User-detected: True

Hi, Nameless one!
```

> окончание проверки
> ---
</details>

# **task 2:**
## **task 2.1:**
Найти информацию о том, что такое самоподписанные сертификаты и сгенерировать такой для своего веб-сервера. Написать своими словами, что это такое и как сгенерить.

---

Самоподписанный SSL сертификат - это сертификат созданный и подписанный тем же лицом, которое этот сертификат идентифицирует. 
Например: у нас есть сайт и мы хотим обеспечить его безопасность, т.е. сделать так, чтобы пользователи были уверены, что зашли именно на наш сайт и что передача данных между нашим сервисом и клиентом должным образом зашифрована. Для этих целей мы создаем пару ключей (публичный и частный). Частный ключ обеспечит подпись информации, передаваемой от нашего сервиса и расшифровку данных от клиента. Публичный ключ вместе с информацией о нас (своего рода паспорт компании/организации) в виде сертификата будет предоставляться при попытке открыть сайт.

Запрос SSL сертификата:  

```ShellSession
cbpi@ubuntu-vbox:~/ssl $ openssl req -x509 -new -nodes -key rootCA.key -sha256 -days 1024 -out rootCA.pem
Can't load /home/cbpi/.rnd into RNG
139999366058432:error:2406F079:random number generator:RAND_load_file:Cannot open file:../crypto/rand/randfile.c:88:Filename=/home/cbpi/.rnd
You are about to be asked to enter information that will be incorporated
into your certificate request.
What you are about to enter is what is called a Distinguished Name or a DN.
There are quite a few fields but you can leave some blank
For some fields there will be a default value,
If you enter '.', the field will be left blank.
-----
Country Name (2 letter code) [AU]:RU
State or Province Name (full name) [Some-State]:Moscow oblast
Locality Name (eg, city) []:Korolev
Organization Name (eg, company) [Internet Widgits Pty Ltd]:Nameless
Organizational Unit Name (eg, section) []:Vault
Common Name (e.g. server FQDN or YOUR name) []:GeekTest
Email Address []:
```

В итоге получим два файла:

- `rootCA.pem` - файл сертификата;
- `rootCA.key` - закрытый ключ;

Копируем полученные ключи в директорию `nginx`:  
  `mkdir /etc/nginx/ssl`  
  `cp rootCA* /etc/nginx/ssl/`

## **task 2.2:**
Добавить SSL соединение для дефолтного виртуального хоста nginx, используя порт 443. Прикрепить конфиги nginx.

---

- Добавим конфигурацию для тестирования SSL соединения:  
  `cp /etc/nginx/sites-available/default /etc/nginx/sites-available/ssl-test`  
  `ln -s /etc/nginx/sites-available/ssl-test /etc/nginx/sites-enabled/ssl-test`  
  `vim /etc/nginx/sites-available/ssl-test`

```
server {
        listen 80;
        listen [::]:80;

        server_name ubuntu-vbox;

        return 301 https://$server_name;
}

server {
        listen 443 ssl;
        listen [::]:443 ssl;

        ssl_certificate         /etc/nginx/ssl/rootCA.pem;
        ssl_certificate_key     /etc/nginx/ssl/rootCA.key;

        server_name ubuntu-vbox;
        root /var/www/html;
        index index.html index.htm index.nginx-debian.html;

        location / {
                # First attempt to serve request as file, then
                # as directory, then fall back to displaying a 404.
                try_files $uri $uri/ =404;
        }
}
```

- Проверим корректность конфига и перезапустим сервис:  
  `nginx -t`  
  `service nginx reload`

## **task 2.3:**
Откройте в браузере страницу хоста и посмотрите, как браузер реагирует на самоподписанные сертификаты. Напишите, что видите.

---

Откроем на локальной машине `https://ubuntu-vbox/`.
При открытии страницы в Firefox появляется предупреждение:

```
Предупреждение: Вероятная угроза безопасности

Firefox обнаружил вероятную угрозу безопасности и не стал открывать 5.35.14.33. Если вы посетите этот сайт, нападавшие могут попытаться похитить вашу информацию, такую как пароли, адреса электронной почты или данные банковских карт.

Подробнее…

Веб-сайты подтверждают свою подлинность с помощью сертификатов. Firefox не доверяет этому сайту, потому что он использует сертификат, недействительный для ubuntu-vbox.
 
Код ошибки: MOZILLA_PKIX_ERROR_SELF_SIGNED_CERT
 
Просмотреть сертификат
```

## **task 2.4:**
Для Firefox сделайте так, чтобы эта ошибка исчезла (Firefox позволяет манипулировать root сертификатами). Напишите, как это сделать.

---

На основе ключей, полученных в **task 2.3** (т.е. используем их как корневой сертификат) выпустим новый сертификат, который можно будет сделать доверенным для браузера.

Используем следующий скрипт:  
  `vim make_cert_for_domain.sh`
  
```Shell
#!/bin/bash

if [ -z "$1" ]
then
  echo "Please enter a subdomain to create a certificate for";
  echo "e.g. mysite.localhost"
  exit;
fi

if [ -f device.key ]; then
  KEY_OPT="-key"
else
  KEY_OPT="-keyout"
fi

DOMAIN=$1
COMMON_NAME=${2:-$1}

SUBJECT="/C=CA/ST=None/L=NB/O=None/CN=$COMMON_NAME"
NUM_OF_DAYS=999

openssl req -new -newkey rsa:2048 -sha256 -nodes $KEY_OPT device.key -subj "$SUBJECT" -out device.csr

cat cert_options.ext | sed s/%%DOMAIN%%/$COMMON_NAME/g > /tmp/__mk_cert.ext

openssl x509 -req -in device.csr -CA rootCA.pem -CAkey rootCA.key -CAcreateserial -out device.crt -days $NUM_OF_DAYS -sha256 -extfile /tmp/__mk_cert.ext

mv device.csr $DOMAIN.csr
cp device.crt $DOMAIN.crt

rm -f device.crt;
```

Файл `cert_options.ext`:

```Shell
authorityKeyIdentifier=keyid,issuer
basicConstraints=CA:FALSE
keyUsage = digitalSignature, nonRepudiation, keyEncipherment, dataEncipherment
subjectAltName = @alt_names

[alt_names]
DNS.1 = %%DOMAIN%%
DNS.2 = *.%%DOMAIN%%
```

- Выпуск сертификата и настройка `nginx`:

1. Создадим сертификат на основе корневого для домена `ubuntu-vbox.localhost`:  
  `sudo ./make_cert_for_domain.sh ubuntu-vbox.localhost`
2. Копируем полученные ключи в каталог `nginx`:  
  `cp ubuntu-vbox.localhost.crt device.key /etc/nginx/ssl/`
3. Модифицируем конфиг чтобы иметь возможность получить корневой сертификат:

```
server {
        listen 80;
        listen [::]:80;

        server_name ubuntu-vbox;

        return 301 https://$server_name;
}

server {
        listen 443 ssl;
        listen [::]:443 ssl;

        ssl_certificate         /etc/nginx/ssl/rootCA.pem;
        ssl_certificate_key     /etc/nginx/ssl/rootCA.key;

        server_name ubuntu-vbox;
}

server {
        listen 80;
        listen [::]:80;

        server_name ubuntu-vbox.localhost;
        return 301 https://$server_name$request_uri;
}

server {
        listen 443 ssl;
        listen [::]:443 ssl;

        ssl_certificate         /etc/nginx/ssl/ubuntu-vbox.localhost.crt;
        ssl_certificate_key     /etc/nginx/ssl/device.key;

        server_name ubuntu-vbox.localhost;

        root /var/www/html;
        index index.html index.htm index.nginx-debian.html;

        location / {
                # First attempt to serve request as file, then
                # as directory, then fall back to displaying a 404.
                try_files $uri $uri/ =404;
        }
}
```

4. Проверим корректность конфига и перезапустим сервис:  
  `nginx -t`  
  `service nginx reload`
5. Добавим запись в файл `hosts`:  
  `echo "127.0.0.1 ubuntu-vbox.localhost" >> /etc/hosts`

- Как добавить корневой сертификат в список доверенных:

1. Открыть в Firefox страницу `https://ubuntu-vbox/`
2. Нажать `Доролнительно`
3. Нажать `Просмотреть сертификат`
4. Скачать файл `PEM (сертификат)`
5. Закрыть страницу `https://ubuntu-vbox/`
6. Открыть в Firefox страницу `about:preferences#privacy`
7. Открыть `Просмотор сертификатов`
8. Во вкладке `Центры сертификации` нажать кнопку `Импортировать`
9. Выбрать скачанный в п.4 файл `Geektest.pem`
10. Отметить чекбокс `Этот сертификат может служить для идентификации веб-сайтов` и нажать `OK`
11. Нажать `OK` в окне управления сертификатами, закрыть страницу настроек
12. Открыть в Firefox страницу `https://ubuntu-vbox.localhost`

Готово =)

## **task 2.5:**
Мы говорили о необходимости шифровать симметрично. При этом проблем с получением ассиметричных сертификатов нет. Зачем такая сложная схема для установления SSL соединения? Почему бы не шифровать ассиметрично? Своими словами.

---

У обоих видов шифрования есть как преимущества, так и недостатки, а именно:  
- Симметричное шифрование:  
  + Преимущество: процесс шифрования намного быстрее и менее ресуроемкий по сравнению с ассиметричным.  
  - Недостаток: для шифрования и для дешифрования используется один и тот же ключ, что создает определенные риски - при перехвате ключа данные могут быть расшифрованы злоумышленником.
- Ассиметричное шифрование:  
  + Преимущество: надежность - при перехвате открытого ключа данные не могут быть расшифрованы.  
  - Недостаток: более ресурсоемкий, медленнее по сравнению с симметричным.

Как раз благодаря схеме установки SSL соединения и обеспечивается надежность и скорость передачи данных.  
Благодаря сертификату обеспечивается уверенность в том, что соединение было установлено именно с тем, с кем нужно.  
Для того чтобы перехват передаваемого ключа не давал возможность расшифровать данные ключ шифруется ассиметричным методом и для каждой сессии связи устанавливается новый ключ.  
Для скорости обмена данными и приемлемного потребления ресурсов - сами данные шифруются уже симметричным методом, с помощью сессионного ключа.

# **task 3:**
## **task 3.1 - 3.3:**
Cейчас почти все сетевые провайдеры предоставляют клиентам белые адреса по умолчанию. Если у вас белый адрес, то нужно сделать ваш дефолтный виртуальный хост nginx доступным извне. Разберитесь и сделайте проброс портов на вашем роутере (с публичного белого адреса на внутренний адрес компьютера) В настройках Virtual Box / VMWare cделайте проброс портов с вашего компьютера на виртуальную машину, либо переведите сетевой интерфейс виртуальный машины в режим bridged и сделайте правки к конфигурации роутера.

---

Тип подключения в настройках сети для виртуальной машины: `Сетевой мост`

1. Изменим порт доступа к веб-конфигуротору роутера:  
  `(config)> ip http port 8080`
2. Добавим перенаправление при обращении к портам `80` и `443` на машину с настроенным **nginx**:  
  `(config)> ip static tcp PPPoE0 80 nginx_machine_MAC !nginx 80`  
  `(config)> ip static tcp PPPoE0 443 nginx_machine_MAC !nginx 443`

## **task 3.4:**
Разберитесь с тем, что такое файл /etc/hosts (в Windows c:\Windows\System32\Drivers\etc\hosts) и сделайте свой сайт доступным по адресу https://geekbrains-linux.ru

---

Для доступа по адресу `https://geekbrains-linux.ru` нужно добавить в файл `hosts` строку:  
`5.35.14.33	geekbrains-linux.ru`

Однако, вот инструкция как установить защищенное соединение по адресу `https://ubuntu-vbox.localhost/`:

1. Добавить в файл `hosts` строку: `5.35.14.33	ubuntu-vbox.localhost`
2. Открыть в Firefox страницу `https://5.35.14.33`
3. Нажать кнопку `Дополнительно`
4. Нажать `Просмотреть сертификат`
5. Скачать файл `PEM (сертификат)`, закрыть страницу просмотра сертификата
6. Закрыть страницу `https://5.35.14.33`
7. Открыть в Firefox страницу `about:preferences#privacy`
8. Открыть `Просмотор сертификатов`
9. Во вкладке `Центры сертификации` нажать кнопку `Импортировать`
10. Выбрать скачанный в п.4 файл `Geektest.pem`
11. Отметить чекбокс `Этот сертификат может служить для идентификации веб-сайтов` или `Доверять при идентификации веб-сайтов` и нажать `OK`
12. Нажать `OK` в окне управления сертификатами, закрыть страницу настроек
13. Открыть в Firefox страницу `https://ubuntu-vbox.localhost` или `http://ubuntu-vbox.localhost`
