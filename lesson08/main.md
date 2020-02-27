## **task 1:**
��� ����������� ������ ���������� �� �����������?

---
- ����������� ������ `VIRT` - ���  ����� ���������� ������, ������� �������� ���������� ��������� � ������ ������ �������.  
  `VIRT = DATA + CODE + SWAP + SHR` ����� �������� � ���� ��������, ������� ���� �������� ��������, �� �� ������������.  
  - `DATA` - ����������� ������, ���������� ��� ���, ������� �� �������� �����������;  
  - `CODE` - ����������� ������, ���������� ��� ����������� ���;  
  - `SWAP` - ������, ������� �� �������� �����������, �� �������� � ������� ��������. ��� ������, ������� ��������� � SWAP, �� ����� ��������� �������������� ������������� ������. `SWAP = VIRT - RES`;  
  - `SHR` - ���������� ����������� ������, ������� ������������ ���������. ���������� ���������� ������, ������� ������������ ����� ���� ��������� � ������� ����������. Shared, ���������� ����� ���������� �� ������� VIRT ���������� ��������� (������� ��� ������������).  

- ����������� ������ `RES` - ��� ����� ���������� ������, ������� ���������� �������.  
  ��� ��������, ����� ������ �������� `VIRT`, ��� ��� ����������� �������� ������� �� ����������� `C library`.

�.�. ����� ������� ��� ������� � ���, ��� ����������� ������ - ��� ������� ���������� ������ ������� ������� ����������, � ����������� - ��� ������� �������� ���� �������� �������� (������� ����� ������������ ������������).

��������:  
[������� TOP: ������� ����� VIRT, RES � SHR ���������](http://profhelp.com.ua/content/%D0%BE%D1%82%D0%BB%D0%B8%D1%87%D0%B8%D0%B5-%D0%BC%D0%B5%D0%B6%D0%B4%D1%83-virt-res-%D0%B8-shr-%D0%B2-%D0%B2%D1%8B%D0%B2%D0%BE%D0%B4%D0%B5-%D0%BA%D0%BE%D0%BC%D0%BC%D0%B0%D0%BD%D0%B4%D1%8B-top)


## **task 2:**
��� ����� ����������� �������� ������� `/proc` ?

---
- ���� Linux ������������� �������� ������� � ����� ���������� ���������� � ��������� �������� ��������� ���� �� ����� ������ �� ����������� ������� `/proc`  
- �������� ������� `/proc` �������� ���������� ��� ���� � ��� �������, ����������� �������� ���������� ���������.  
- �������� ������� `/proc` ������������� � ������.  
- ����������� �� `/proc` ������������ ����������� ������ � ����������� ����������� ����, ��������� �������� ���������� � ��������� � ��������� ��������� (����� ��������� ����) �� ����.

���������:  
[������� �������� ������� proc](https://www.dkws.org.ua/article.php?id=65)  
[�������� ������� proc � Linux](https://losst.ru/fajlovaya-sistema-proc-v-linux)

## **task 3:**
��� ��������� ���� ���������� � �������?

---
���������� ���� ���������� � ������� �������������� ������� �������� `0` � �������������� ���� � VFS `/sys`:  
`/sys/devices/system/cpu/cpuN/online`

������:

- �������� ���������:

```ShellSession
root@ubuntu-vbox:~# lscpu | grep CPU.*list
On-line CPU(s) list:     0-3
```

- ���������� ����:

```ShellSession
root@ubuntu-vbox:~# cat /sys/devices/system/cpu/cpu3/online
1
root@ubuntu-vbox:~# echo 0 > /sys/devices/system/cpu/cpu3/online
```

- ��������:

```ShellSession
root@ubuntu-vbox:~# lscpu | grep CPU.*list
On-line CPU(s) list:     0-2
Off-line CPU(s) list:    3
```

���������:  
[���������� ���� ���������� � Linux ](https://blog.tataranovich.com/2013/02/linux.html)

## **task 4:**
����� ���� ���� �����������?

---
� ���� Linux ������������ ��� ���� �����������:
- *nice* ���������:  
 �������� ����� ���������� ���������� **����� ���������� ������**, ������� ���������� ����� ������������ ������������� *Completely Fair Scheduler (CFS)*.  
 �������� nice-���������� ����� ������ � ��������� �� `-20` �� `19`, �� ��������� ������������ �������� `0`. �������� `�20` ������������� �������� �������� ����������.  
- *real-time* ���������:  
  �������� ����� ���������� ���������� **������� ���������� �����**, ����� ���������� ������ �� ������������ �� ��� ���, ���� ��� �� ����� ��������� *real-time* ������� � ������� ����������� (��������� ��� *SCHED_FIFO*, ��� *SCHED_RR* ����� ����� ���� ����������). �������� �������� ����������� ��������� ������� ���������� �� `1` �� `99`.

*real-time* ������ (������, � ������� ��������� �������� *SCHED_FIFO/SCHED_RR*) �� ����� ���� ��������� *nice* �������� (������, � ������� ��������� �������� *SCHED_NORMAL*), �.�. ����� ������� ��������� ��� *nice* ��������.

���������:  
[��������� �������� � Linux](https://life-prog.ru/view_linux.php?id=15)  
[A difference between nice priorities and RT priorities in Linux](https://medium.com/@karadaharu/a-difference-between-nice-priorities-and-rt-priorities-in-linux-7c299f8ea790)  
[Priorities and policies](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_MRG/1.3/html/Realtime_Reference_Guide/chap-Realtime_Reference_Guide-Priorities_and_policies.html)

## **task 5:**
��������� ��� ��������, ������� ���������� ��� ����� ���������� ����� �������, ����� ������ �� ��� ���� ������ ������ ������������� �������

---
- �������������� �������� ��� ������������ ���� ����� ������ (��� �����������):

```ShellSession
cbpi@ubuntu-vbox:~/bin $ lscpu | grep CPU.*list
On-line CPU(s) list:     0-3
cbpi@ubuntu-vbox:~/bin $ su
������:
root@ubuntu-vbox:/home/cbpi/bin# echo 0 > /sys/devices/system/cpu/cpu1/online
root@ubuntu-vbox:/home/cbpi/bin# echo 0 > /sys/devices/system/cpu/cpu2/online
root@ubuntu-vbox:/home/cbpi/bin# echo 0 > /sys/devices/system/cpu/cpu3/online
root@ubuntu-vbox:/home/cbpi/bin# exit
exit
cbpi@ubuntu-vbox:~/bin $ lscpu | grep CPU.*list
On-line CPU(s) list:     0
Off-line CPU(s) list:    1-3
```

- ������ ������� ��������:

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

- ������ ������� ��������:

```ShellSession
cbpi@ubuntu-vbox:~/bin $ simple_proc.sh &
[8] 32235
cbpi@ubuntu-vbox:~/bin $ top
  PID USER      PR  NI    VIRT    RES    SHR S %CPU %MEM     TIME+ COMMAND
32238 cbpi      20   0   14612    928    864 R 50,0  0,0   0:07.27 dd
31837 cbpi      20   0   14612    896    832 R 43,8  0,0   1:10.99 dd
```

- ��������� ���������� ��� ������� ��������:

```ShellSession
cbpi@ubuntu-vbox:~/bin $ renice 10 -p 32238
32238 (process ID) ������ ��������� 0, ����� ��������� 10
cbpi@ubuntu-vbox:~/bin $ top
PID USER      PR  NI    VIRT    RES    SHR S %CPU %MEM     TIME+ COMMAND
31837 cbpi      20   0   14612    896    832 R 87,8  0,0   1:47.64 dd
32238 cbpi      30  10   14612    928    864 R  9,6  0,0   0:30.95 dd
```

��� ������ �������� ����������� ��������� ������������ �����.

## **task 6:**
��� �������� overcommit'��? ����� ���������� �������� ��������� ������?

---
*Overcommit* - ��������� ��������� ������, ����� ������������ ������� ��������� ����������� �������� ������ ����������� ������, ��� �������� � �������.

��������� ����������� � `/proc/sys/vm/`, ������������ ��������: `sysctl vm.parameter_name=value`

���������:  
- `overcommit_memory` - ���������� ������� ���������� � ������ �������� ������� ������� ������. ��������� ��������:  
  - 0 (�� ���������) � ������������� ������ � ������������� ������. � ��� ���������� ������� ������, ������� ����� �������. �� � swap/res �������� ������ �� ��������, ������� ������������ ���� ���������  
  - 1 � ���� �� ������������ ���������� ������, �.�. ������ ������������� ����� ������� �� ��������� ������.  
  - 2 � ����� ��������� ��������, ������������� ������, ������ ������� ��������� ��������� ������ ������ swap � RAM � ������������ � `overcommit_ratio`. ���������� ����� ������������ ������ ����� `total_swap + alowed`, ��� `alowed = total_ram * overcommit_ratio / 100`  
- `overcommit_ratio` - ���� `overcommit_memory` ����� 2, ���������� ���������� ����� ���������� ������. �� ��������� ����� `50`. 
- `nr_hugepages` - ����� ������� �������� �������. �� ��������� ����� `0`. ��������� ������� ������� �������� ������ ��� ������� ������������ ����� ��������� ���� �� ������ ��������� �������. ����������������� ���� ���������� �������� �� ����� �������������� ��� ������ �����.
	
���������:  
[��������� ������������������ � /proc](https://access.redhat.com/documentation/ru-ru/red_hat_enterprise_linux/6/html/performance_tuning_guide/s-memory-captun)  
[����������� ��������� ������ (overcommit)](http://tolstiyman.blogspot.com/2013/08/overcommit.html)  
[��� ������: overcommit memory](http://catap.ru/blog/2009/05/05/about-memory-overcommit-memory/)  
[��� Linux �������� � �������](https://habr.com/ru/company/yandex/blog/250753/)

## **task 7:**
����� ����� ��������� �������� ��� ������� overcommit'a?

---
�������� `overcommit` ��. � **task 6** ��������� - `overcommit_memory`.

��������� ���� ��������� ������, ������� `swap`, ����� �������� � `kernel_pani�` (����� �������� �������� ��� �������� `overcommit_memory` = `0` ��� `1`).

�������� ��������� ������� `out of memory`:  
- �������������� ������������ ������ (����������� �������) - � ���� ������ ������� ����������� �������� `oom_killer` ��������� �������� ������ ������� ��� ������������ ������. ���������:  
  - `vm.panic_on_oom` - `0` ��� ��������� (�� ���������), `1` - ��� ����������;  
  - `/proc/$PID/oom_adj` - � ���� ����� ��� ������� �������� ������� ��������, ������� ����� �������� �� ����� �������� ��� "�������". �������� � ��������� �� `-16` �� `15` ���������� `oom_score` ��������. ��� ������ `oom_score`, ��� ������ ����������� ����, ��� `oom_killer` ��������� �������. �������� `-17` ��������� `oom_killer` ��� ��������.  
  ��������, ����� �������� ������������ ������� ��������� �������: `echo -17 > /proc/$PID/oom_adj`
- �������������� ������������ ������� - ���������� �������� `kernel.panic` ������ ����.  
  ��������, `sysctl kernel.panic=60` ��������� �������������� ������������ ������� ����� 60 ������ ��� `kernel_pani�`.  
  ����� ���� �������� ����� ������������ ������� ���������� ������������� ���������� ��� ������������� � ����� `/etc/sysctl.conf`  

[�������������� ������������ linux ��� kernel panic](https://i-notes.org/avtomaticheskaya-perezagruzka-linux-pri-kernel-panic/)  
[Linux Out-of-Memory Killer (OOM)](http://geckich.blogspot.com/2013/12/linux-out-of-memory-killer-oom.html)  
[����������� Out-Of-Memory Killer � Linux ��� PostgreSQL](https://habr.com/ru/company/southbridge/blog/464245/)  
[��� ������: OOM Killer](http://catap.ru/blog/2009/05/03/about-memory-oom-killer/)  
[How to Configure the Linux Out-of-Memory Killer](https://www.oracle.com/technical-resources/articles/it-infrastructure/dev-oom-killer.html)

## **task 8:**
������� ������� 5, ��������� ����������� ������ � �������� cpushares - ��������� ��� ��������, ������� ���������� ��� ����� ���������� ����� �������, ����� ������ �� ��� ���� ������ ������ ������������� �������

---
������� � ������ ������ `taskset -cp 0 $$` ��� ���������� ������� �������� �� ������������ ���� `0`.

- ������ ������� ��������:

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

- ������ ������� ��������:

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

- �������� `cgroup` � ����������� ���� ������� ��������:

```ShellSession
root@ubuntu-vbox:/home/cbpi/bin# mkdir /sys/fs/cgroup/cpu/cgroup_test
root@ubuntu-vbox:/home/cbpi/bin# echo 3688 > /sys/fs/cgroup/cpu/cgroup_test/tasks
```

- ��������� ��������� `cpu.shares` � ��������:

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

��� ���������� ��������� `cpu.shares` ����������� � ������������ �����, ���������� �������� ������� ��� �������� �� ��� `cgroup`

[Starting a Process in a Control Group](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/6/html/resource_management_guide/starting_a_process)  
[Assigning Processes to CPU Cores](https://serverfault.com/questions/345687/assigning-processes-to-cpu-cores)  
