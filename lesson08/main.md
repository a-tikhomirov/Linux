## **task 1:**
Чем виртуальная память отличается от резидентной?

---
- Вирутальная память `VIRT` - это  общее количество памяти, которое способна адресовать программа в данный момент времени.  
  `VIRT = DATA + CODE + SWAP + SHR` Также включает в себя страницы, которые были выделены системой, но не использованы.  
  - `DATA` - виртуальная память, отведенная под код, который не является исполняемым;  
  - `CODE` - виртуальная памятя, отведенная под исполняемый код;  
  - `SWAP` - память, которая не является резидентной, но доступна в текущем процессе. Это память, которая выгружена в SWAP, но может содержать дополнительную нерезидентную память. `SWAP = VIRT - RES`;  
  - `SHR` - Количество разделяемой памяти, которое используется процессом. Отображает количество памяти, которая потенциально может быть разделена с другими процессами. Shared, отображает какое количество от размера VIRT фактически разделено (памятью или библиотеками).  

- Резидентная память `RES` - это объем физической памяти, которую использует процесс.  
  Это значение, будет меньше значения `VIRT`, так как большинство программ зависят от разделяемой `C library`.

Т.е. можно сказать что отличие в том, что резидентная память - это сколько физической памяти процесс реально использует, а виртуальная - это сколько процессу было выделено системой (сколько может использовать потенциально).

Источник:  
[Команда TOP: Отличие между VIRT, RES и SHR столбцами](http://profhelp.com.ua/content/%D0%BE%D1%82%D0%BB%D0%B8%D1%87%D0%B8%D0%B5-%D0%BC%D0%B5%D0%B6%D0%B4%D1%83-virt-res-%D0%B8-shr-%D0%B2-%D0%B2%D1%8B%D0%B2%D0%BE%D0%B4%D0%B5-%D0%BA%D0%BE%D0%BC%D0%BC%D0%B0%D0%BD%D0%B4%D1%8B-top)


## **task 2:**
Что такое виртуальная файловая система `/proc` ?

---
- Ядро Linux предоставляет механизм доступа к своим внутренним структурам и позволяет изменять установки ядра во время работы ОС посредством системы `/proc`  
- Файловая система `/proc` является механизмом для ядра и его модулей, позволяющим посылать информацию процессам.  
- Файловая система `/proc` располагается в памяти.  
- Виртуальная ФС `/proc` обеспечивает возможность работы с внутренними структурами ядра, получения полезной информации о процессах и изменения установок (меняя параметры ядра) на лету.

Источники:  
[Изучаем файловую систему proc](https://www.dkws.org.ua/article.php?id=65)  
[Файловая система proc в Linux](https://losst.ru/fajlovaya-sistema-proc-v-linux)

## **task 3:**
Как отключить ядро процессора в системе?

---
Отключение ядра процессора в системе обеспечивается записью значения `0` в соотвествующий файл в VFS `/sys`:  
`/sys/devices/system/cpu/cpuN/online`

Пример:

- Исходное состояние:

```ShellSession
root@ubuntu-vbox:~# lscpu | grep CPU.*list
On-line CPU(s) list:     0-3
```

- Отключение ядра:

```ShellSession
root@ubuntu-vbox:~# cat /sys/devices/system/cpu/cpu3/online
1
root@ubuntu-vbox:~# echo 0 > /sys/devices/system/cpu/cpu3/online
```

- Проверка:

```ShellSession
root@ubuntu-vbox:~# lscpu | grep CPU.*list
On-line CPU(s) list:     0-2
Off-line CPU(s) list:    3
```

Источники:  
[Отключение ядер процессора в Linux ](https://blog.tataranovich.com/2013/02/linux.html)

## **task 4:**
Какие есть типы приоритетов?

---
В ядре Linux используется два типа приоритетов:
- *nice* приоритет:  
 Значение этого приоритета определяет **время исполнения задачи**, порядок выполнения задач регулируется планировщиком *Completely Fair Scheduler (CFS)*.  
 Значение nice-приоритета может лежать в диапазоне от `-20` до `19`, по умолчанию используется значение `0`. Значение `–20` соответствует наиболее высокому приоритету.  
- *real-time* приоритет:  
  Значение этого приоритета определяет **порядок выполнения задач**, время выполнения задачи не лимитировано до тех пор, пока она не будет вытеснена *real-time* задачей с большим приоритетом (актуально для *SCHED_FIFO*, для *SCHED_RR* время может быть ограничено). Диапазон значений приоритетов реального времени составляет от `1` до `99`.

*real-time* задачи (задачи, к которым применена политика *SCHED_FIFO/SCHED_RR*) не могут быть вытеснены *nice* задачами (задачи, к которым применена политика *SCHED_NORMAL*), т.е. имеют больший приоритет над *nice* задачами.

Источники:  
[Приоритет процесса в Linux](https://life-prog.ru/view_linux.php?id=15)  
[A difference between nice priorities and RT priorities in Linux](https://medium.com/@karadaharu/a-difference-between-nice-priorities-and-rt-priorities-in-linux-7c299f8ea790)  
[Priorities and policies](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_MRG/1.3/html/Realtime_Reference_Guide/chap-Realtime_Reference_Guide-Priorities_and_policies.html)

## **task 5:**
Запустить два процесса, которые потребляют все время процессора таким образом, чтобы одному из них ядро давала больше процессорного времени

---
- Предварительно отключим все процессорные ядра кроме одного (для наглядности):

```ShellSession
cbpi@ubuntu-vbox:~/bin $ lscpu | grep CPU.*list
On-line CPU(s) list:     0-3
cbpi@ubuntu-vbox:~/bin $ su
Пароль:
root@ubuntu-vbox:/home/cbpi/bin# echo 0 > /sys/devices/system/cpu/cpu1/online
root@ubuntu-vbox:/home/cbpi/bin# echo 0 > /sys/devices/system/cpu/cpu2/online
root@ubuntu-vbox:/home/cbpi/bin# echo 0 > /sys/devices/system/cpu/cpu3/online
root@ubuntu-vbox:/home/cbpi/bin# exit
exit
cbpi@ubuntu-vbox:~/bin $ lscpu | grep CPU.*list
On-line CPU(s) list:     0
Off-line CPU(s) list:    1-3
```

- Запуск первого процесса:

```ShellSession
cbpi@ubuntu-vbox:~/bin $ cat simple_proc.sh
#!/bin/bash
dd if=/dev/zero of=/dev/null
cbpi@ubuntu-vbox:~/bin $ simple_proc.sh &
[7] 31834
cbpi@ubuntu-vbox:~/bin $ top
 PID USER      PR  NI    VIRT    RES    SHR S %CPU %MEM     TIME+ COMMAND
31837 cbpi      20   0   14612    896    832 R 97,4  0,0   0:37.92 dd
```

- Запуск второго процесса:

```ShellSession
cbpi@ubuntu-vbox:~/bin $ simple_proc.sh &
[8] 32235
cbpi@ubuntu-vbox:~/bin $ top
  PID USER      PR  NI    VIRT    RES    SHR S %CPU %MEM     TIME+ COMMAND
32238 cbpi      20   0   14612    928    864 R 50,0  0,0   0:07.27 dd
31837 cbpi      20   0   14612    896    832 R 43,8  0,0   1:10.99 dd
```

- Понижение приоритета для второго процесса:

```ShellSession
cbpi@ubuntu-vbox:~/bin $ renice 10 -p 32238
32238 (process ID) старый приоритет 0, новый приоритет 10
cbpi@ubuntu-vbox:~/bin $ top
PID USER      PR  NI    VIRT    RES    SHR S %CPU %MEM     TIME+ COMMAND
31837 cbpi      20   0   14612    896    832 R 87,8  0,0   1:47.64 dd
32238 cbpi      30  10   14612    928    864 R  9,6  0,0   0:30.95 dd
```

Для второг процесса уменьшилось выдялемое процессорное время.

## **task 6:**
Что называют overcommit'ом? Какие существуют политики выделения памяти?

---
*Overcommit* - стратегия выделения памяти, когда операционная система разрешает приложениям занимать больше виртуальной памяти, чем доступно в системе.

Параметры расположены в `/proc/sys/vm/`, регулируются командой: `sysctl vm.parameter_name=value`

Параметры:  
- `overcommit_memory` - определяет условия разрешения и отказа запросов больших объемов памяти. Возможные значения:  
  - 0 (по умолчанию) — эвристический подход к распределению памяти. В нем выделяется столько памяти, сколько хочет процесс. Но в swap/res попадает только те страницы, которые используются этим процессом  
  - 1 — ядро не обрабатывает перерасход памяти, т.е. всегда удовлетворяет любые запросы на выделение памяти.  
  - 2 — отказ обработки запросов, запрашивающих память, размер которой превышает суммарный размер памяти swap и RAM в соответствии с `overcommit_ratio`. Допустимый объем пространства памяти будет `total_swap + alowed`, где `alowed = total_ram * overcommit_ratio / 100`  
- `overcommit_ratio` - если `overcommit_memory` равен 2, определяет процентную часть физической памяти. По умолчанию равен `50`. 
- `nr_hugepages` - число страниц большого размера. По умолчанию равно `0`. Выделение больших страниц возможно только при наличии достаточного числа следующих друг за другом свободных страниц. Зарезервированные этим параметром страницы не могут использоваться для других целей.
	
Источники:  
[Параметры производительности в /proc](https://access.redhat.com/documentation/ru-ru/red_hat_enterprise_linux/6/html/performance_tuning_guide/s-memory-captun)  
[Черезмерное выделение памяти (overcommit)](http://tolstiyman.blogspot.com/2013/08/overcommit.html)  
[Про память: overcommit memory](http://catap.ru/blog/2009/05/05/about-memory-overcommit-memory/)  
[Как Linux работает с памятью](https://habr.com/ru/company/yandex/blog/250753/)

## **task 7:**
Какие можно выставить политики при событии overcommit'a?

---
Политики `overcommit` см. в **task 6** Параметры - `overcommit_memory`.

Выделение всей доступной памяти, включая `swap`, может привести к `kernel_paniс` (такая ситуация возможна при значении `overcommit_memory` = `0` или `1`).

Варинаты обработки события `out of memory`:  
- Автоматическое освобождение памяти (стандартная реакция) - в этом случае система посредством процесса `oom_killer` завершает наименее важный процесс для освобождения памяти. Параметры:  
  - `vm.panic_on_oom` - `0` для включения (по умолчанию), `1` - для выключения;  
  - `/proc/$PID/oom_adj` - в этом файле для каждого процесса записан параметр, который может повлиять на выбор процесса для "убиения". Значение в диапазоне от `-16` до `15` определяет `oom_score` процесса. Чем больше `oom_score`, тем больше вероятность того, что `oom_killer` остановит процесс. Значение `-17` отключает `oom_killer` для процесса.  
  Например, можно защитить определенный процесс следующим образом: `echo -17 > /proc/$PID/oom_adj`
- Автоматическая перезагрузка системы - установить параметр `kernel.panic` больше нуля.  
  Например, `sysctl kernel.panic=60` установит автоматическую перезагрузку системы через 60 секунд при `kernel_paniс`.  
  Чтобы этот параметр после перезагрузки системы применялся автоматически необходимо его зафиксировать в файле `/etc/sysctl.conf`  

[Автоматическая перезагрузка linux при kernel panic](https://i-notes.org/avtomaticheskaya-perezagruzka-linux-pri-kernel-panic/)  
[Linux Out-of-Memory Killer (OOM)](http://geckich.blogspot.com/2013/12/linux-out-of-memory-killer-oom.html)  
[Настраиваем Out-Of-Memory Killer в Linux для PostgreSQL](https://habr.com/ru/company/southbridge/blog/464245/)  
[Про память: OOM Killer](http://catap.ru/blog/2009/05/03/about-memory-oom-killer/)  
[How to Configure the Linux Out-of-Memory Killer](https://www.oracle.com/technical-resources/articles/it-infrastructure/dev-oom-killer.html)

## **task 8:**
Сделать задание 5, используя контрольные группы и параметр cpushares - Запустить два процесса, которые потребляют все время процессора таким образом, чтобы одному из них ядро давала больше процессорного времени

---
Добавим в скрипт строку `taskset -cp 0 $$` для назначения запуска процесса на процессорном ядре `0`.

- Запуск первого процесса:

```ShellSession
cbpi@ubuntu-vbox:~/bin $ cat proc.sh
#!/bin/bash
taskset -cp 0 $$
dd if=/dev/zero of=/dev/null
cbpi@ubuntu-vbox:~/bin $ proc.sh &
[7] 3407
pid 3407's current affinity list: 0-3
pid 3407's new affinity list: 0
cbpi@ubuntu-vbox:~/bin $ top
  PID USER      PR  NI    VIRT    RES    SHR S  %CPU %MEM     TIME+ COMMAND
 3411 cbpi      20   0   14612    896    832 R 100,0  0,0   0:28.25 dd
```

- Запуск второго процесса:

```ShellSession
cbpi@ubuntu-vbox:~/bin $ proc.sh &
[8] 3684
pid 3684's current affinity list: 0-3
pid 3684's new affinity list: 0
cbpi@ubuntu-vbox:~/bin $ top
  PID USER      PR  NI    VIRT    RES    SHR S  %CPU %MEM     TIME+ COMMAND
 3688 cbpi      20   0   14612    880    816 R  53,3  0,0   0:05.15 dd
 3411 cbpi      20   0   14612    896    832 R  46,7  0,0   0:47.43 dd
```

- Создание `cgroup` и перемещение туда второго процесса:

```ShellSession
root@ubuntu-vbox:/home/cbpi/bin# mkdir /sys/fs/cgroup/cpu/cgroup_test
root@ubuntu-vbox:/home/cbpi/bin# echo 3688 > /sys/fs/cgroup/cpu/cgroup_test/tasks
```

- Изменение параметра `cpu.shares` и проверка:

```ShellSession
root@ubuntu-vbox:/home/cbpi/bin# cat /sys/fs/cgroup/cpu/cgroup_test/cpu.shares
1024
root@ubuntu-vbox:/home/cbpi/bin# echo 512 > /sys/fs/cgroup/cpu/cgroup_test/cpu.shares
root@ubuntu-vbox:/home/cbpi/bin# top
 PID USER      PR  NI    VIRT    RES    SHR S  %CPU %MEM     TIME+ COMMAND
 3411 cbpi      20   0   14612    896    832 R  62,1  0,0   2:39.45 dd
 3688 cbpi      20   0   14612    880    816 R  37,9  0,0   1:56.77 dd
root@ubuntu-vbox:/home/cbpi/bin# echo 256 > /sys/fs/cgroup/cpu/cgroup_test/cpu.shares
root@ubuntu-vbox:/home/cbpi/bin# top
 PID USER      PR  NI    VIRT    RES    SHR S  %CPU %MEM     TIME+ COMMAND
 3411 cbpi      20   0   14612    896    832 R  75,7  0,0   4:11.33 dd
 3688 cbpi      20   0   14612    880    816 R  23,9  0,0   2:32.72 dd
```

При уменьшении параметра `cpu.shares` уменьшается и процессорное время, выделяемое процессу который был назначен на эту `cgroup`

[Starting a Process in a Control Group](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/6/html/resource_management_guide/starting_a_process)  
[Assigning Processes to CPU Cores](https://serverfault.com/questions/345687/assigning-processes-to-cpu-cores)  
