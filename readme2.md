#### 1. Какой системный вызов делает команда cd?
	alexeygrv@alexeygrv-G771JW:~$ strace /bin/bash -c 'cd /tmp' > cd.log 2>&1 
	alexeygrv@alexeygrv-G771JW:~$ cat cd.log | grep /tmp
	execve("/bin/bash", ["/bin/bash", "-c", "cd /tmp"], 0x7ffc1c596aa0 /* 48 vars */) = 0
	stat("/tmp", {st_mode=S_IFDIR|S_ISVTX|0777, st_size=4096, ...}) = 0
	chdir("/tmp")                           = 0

##### chdir("/tmp") = 0

#### 2. Попробуйте использовать команду file на объекты разных типов на файловой системе.
#### Используя strace выясните, где находится база данных file на основании которой она делает свои догадки.

	strace file > openat.log 2>&1
	cat openat.log | grep openat
	openat(AT_FDCWD, "/etc/ld.so.cache", O_RDONLY|O_CLOEXEC) = 3
	openat(AT_FDCWD, "/lib/x86_64-linux-gnu/libmagic.so.1", O_RDONLY|O_CLOEXEC) = 3
	openat(AT_FDCWD, "/lib/x86_64-linux-gnu/libc.so.6", O_RDONLY|O_CLOEXEC) = 3
	openat(AT_FDCWD, "/lib/x86_64-linux-gnu/liblzma.so.5", O_RDONLY|O_CLOEXEC) = 3
	openat(AT_FDCWD, "/lib/x86_64-linux-gnu/libbz2.so.1.0", O_RDONLY|O_CLOEXEC) = 3
	openat(AT_FDCWD, "/lib/x86_64-linux-gnu/libz.so.1", O_RDONLY|O_CLOEXEC) = 3
	openat(AT_FDCWD, "/lib/x86_64-linux-gnu/libpthread.so.0", O_RDONLY|O_CLOEXEC) = 3
	openat(AT_FDCWD, "/usr/lib/locale/locale-archive", O_RDONLY|O_CLOEXEC) = 3
	openat(AT_FDCWD, "/etc/magic.mgc", O_RDONLY) = -1 ENOENT (Нет такого файла или каталога)
	openat(AT_FDCWD, "/etc/magic", O_RDONLY) = 3
	openat(AT_FDCWD, "/usr/share/misc/magic.mgc", O_RDONLY) = 3
#####/usr/share/misc/magic.mgc
####3. Предположим, приложение пишет лог в текстовый файл. Этот файл оказался удален (deleted в lsof), однако возможности сигналом сказать приложению переоткрыть файлы или просто перезапустить приложение – нет. Так как приложение продолжает писать в удаленный файл, место на диске постепенно заканчивается. Основываясь на знаниях о перенаправлении потоков предложите способ обнуления открытого удаленного файла (чтобы освободить место на файловой системе).
	ping -c 1000 10.0.2.15 > ping.log 2>&1
	vi ping.log
	rm .ping.log.swp 
	sudo lsof | grep deleted
	vi        1351                       vagrant    4u      REG              253,0    16384     131088 /home/vagrant/.ping.log.swp (deleted)
	>/proc/1351/fd/4
	sudo lsof | grep deleted
	vi        1351                       vagrant    4u      REG              253,0        0     131088 /home/vagrant/.ping.log.swp (deleted)
#####>/proc/1351/fd/4
####4. Занимают ли зомби-процессы какие-то ресурсы в ОС (CPU, RAM, IO)?
#####Зомби не потребляют никаких ресурсов, память и файловые дескрипторы таких процессов уже освобождены. Остается только запись в таблице процессов, которая занимает несколько десятков байт памяти

	ps aux | grep Z
	USER         PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
	vagrant     1491  0.0  0.0   8900   736 pts/2    R+   14:47   0:00 grep --color=auto Z
####5. На какие файлы вы увидели вызовы группы open за первую секунду работы утилиты?

	dpkg -L bpfcc-tools | grep sbin/opensnoop
	/usr/sbin/opensnoop-bpfcc

	sudo /usr/sbin/opensnoop-bpfcc
	PID    COMM               FD ERR PATH
	593    irqbalance          6   0 /proc/interrupts
	593    irqbalance          6   0 /proc/stat
	593    irqbalance          6   0 /proc/irq/20/smp_affinity
	593    irqbalance          6   0 /proc/irq/0/smp_affinity
	593    irqbalance          6   0 /proc/irq/1/smp_affinity
	593    irqbalance          6   0 /proc/irq/8/smp_affinity
	593    irqbalance          6   0 /proc/irq/12/smp_affinity
	593    irqbalance          6   0 /proc/irq/14/smp_affinity
	593    irqbalance          6   0 /proc/irq/15/smp_affinity
	784    vminfo              5   0 /var/run/utmp
	581    dbus-daemon        -1   2 /usr/local/share/dbus-1/system-services
	581    dbus-daemon        19   0 /usr/share/dbus-1/system-services
	581    dbus-daemon        -1   2 /lib/dbus-1/system-services
	581    dbus-daemon        19   0 /var/lib/snapd/dbus-1/system-services/
	1      systemd            12   0 /proc/404/cgroup
	784    vminfo              5   0 /var/run/utmp
####6. Какой системный вызов использует uname -a? Приведите цитату из man по этому системному вызову, где описывается альтернативное местоположение в /proc, где можно узнать версию ядра и релиз ОС.
#####системный вызов uname()
	vagrant@vagrant:~$ strace uname -a > uname-a.log 2>&1
	vagrant@vagrant:~$ cat uname-a.log | grep uname
	execve("/usr/bin/uname", ["uname", "-a"], 0x7ffcc1405788 /* 24 vars */) = 0
	uname({sysname="Linux", nodename="vagrant", ...}) = 0
	uname({sysname="Linux", nodename="vagrant", ...}) = 0
	uname({sysname="Linux", nodename="vagrant", ...}) = 0
man 2 uname

UNAME(2)  
- Part of the utsname information is also  accessible  via  /proc/sys/kernel/{os‐type, hostname, osrelease, version, domainname}.

#### 7. Чем отличается последовательность команд через ; и через && в bash?

#####Двойной амперсанд (&&) 
Командная оболочка будет интерпретировать последовательность символов && как логический оператор "И". При использовании оператора && вторая команда будет исполняться только в том случае, если исполнение первой команды успешно завершится (будет возвращен нулевой код завершения).

	vagrant@vagrant:~$ echo первая команда && echo вторая команда
	первая команда
	вторая команда
	vagrant@vagrant:~$ zecho первая команда && echo вторая команда
	-bash: zecho: команда не найдена...

#####Точка с запятой (;)
Вы можете разместить две и более команд в одной и той же строке, разделив эти команды с помощью символа точки с запятой ;. Все команды с наборами аргументов будут выполнены последовательно, причем командная оболочка будет ожидать завершения исполнения каждой из команд перед исполнением следующей команды.

	vagrant@vagrant:~$ echo Hello ; echo World
	Hello
	World
	vagrant@vagrant:~$ zcho Hello ; echo World
	-bash: zcho: command not found
	World

#####set -e - прерывает сессию при любом ненулевом значении исполняемых команд в конвеере кроме последней.
в случае &&  вместе с set -e- вероятно не имеет смысла, так как при ошибке , выполнение команд прекратится.

####8. Из каких опций состоит режим bash set -euxo pipefail и почему его хорошо было бы использовать в сценариях? 
> - -e  немедленный выход, если команда завершается с ненулевым статусом.
- -u  неустановленные/не заданные параметры и переменные считаются как ошибки, с выводом в stderr текста ошибки и выполнит завершение неинтерактивного вызова.
- -x  с помощью него bash печатает в стандартный вывод все команды перед их исполнением.
- -o  если нужно убедиться, что все команды в пайпах завершились успешно, нужно использовать -o pipefail

#####так безопасней

####9. Используя -o stat для ps, определите, какой наиболее часто встречающийся статус у процессов в системе. В man ps ознакомьтесь (/PROCESS STATE CODES) что значат дополнительные к основной заглавной буквы статуса процессов. Его можно не учитывать при расчете (считать S, Ss или Ssl равнозначными) 
Самые частые
> S - процессы ожидающие завершения (спящие с прерыванием "сна")
I - фоновые(бездействующие) процессы ядра

- доп символы - это доп характеристики, например приоритет.
