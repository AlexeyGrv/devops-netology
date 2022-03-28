#### 1. На лекции мы познакомились с node_exporter. В демонстрации его исполняемый файл запускался в background. Этого достаточно для демо, но не для настоящей production-системы, где процессы должны находиться под внешним управлением. 

##### Используя знания из лекции по systemd, создайте самостоятельно простой unit-файл для node_exporter:
~$sudo vi /etc/systemd/system/node_exporter.service

    [Unit]
    Description=Node Exporter Service
    After=network.target
    
    [Service]
    User=nodeusr
    Group=nodeusr
    Type=simple
    ExecStart=/usr/local/bin/node_exporter
    
    [Install]
    WantedBy=multi-user.target
    
##### поместите его в автозагрузку,
    systemctl enable node_exporter

##### предусмотрите возможность добавления опций к запускаемому процессу через внешний файл (посмотрите, например, на systemctl cat cron),

    sudo touch /usr/local/bin/node_exporter.env
    echo "EXTRA_OPTS=\"--log.level=info\"" | sudo tee /usr/local/bin/node_exporter.env

изменить unit-файл:

    [Service]
    EnvironmentFile=/usr/local/bin/node_exporter.env
    ExecStart=/usr/local/bin/node_exporter $EXTRA_OPTS

##### Удостоверьтесь, что с помощью systemctl процесс корректно стартует, завершается, а после перезагрузки автоматически поднимается.

    ~$ sudo systemctl start node_exporter
    ~$ sudo systemctl status node_exporter
    ● node_exporter.service - Node Exporter Service
     Loaded: loaded (/etc/systemd/system/node_exporter.service; enabled; vendor preset: enabled)
     Active: active (running) since Mon 2022-03-28 16:41:42 MSK; 3h 25min ago
     Main PID: 1138 (node_exporter)
     Tasks: 15 (limit: 9326)
     Memory: 21.6M
     CGroup: /system.slice/node_exporter.service
             └─1138 /usr/local/bin/node_exporter --collector.systemd --collector.systemd.unit-whitelist=(chronyd|mariadb|nginx).service
             
    ~$ sudo systemctl stop node_exporter
    ~$ sudo systemctl status  node_exporter
    ● node_exporter.service - Node Exporter Service
     Loaded: loaded (/etc/systemd/system/node_exporter.service; enabled; vendor preset: e>
     Active: inactive (dead) since Mon 2022-03-28 20:42:02 MSK; 35s ago
    Process: 18655 ExecStart=/usr/local/bin/node_exporter --collector.systemd --collector>
    Main PID: 18655 (code=killed, signal=TERM)
             
    ~$ sudo systemctl restart node_exporter
    ~$ sudo systemctl status  node_exporter
    ● node_exporter.service - Node Exporter Service
     Loaded: loaded (/etc/systemd/system/node_exporter.service; enabled; vendor preset: e>
     Active: active (running) since Mon 2022-03-28 20:43:27 MSK; 3s ago
     Main PID: 20565 (node_exporter)
     Tasks: 8 (limit: 9326)
     Memory: 7.2M
     CGroup: /system.slice/node_exporter.service
             └─20565 /usr/local/bin/node_exporter --collector.systemd --collector.systemd>

#### 2. Ознакомьтесь с опциями node_exporter и выводом /metrics по-умолчанию. Приведите несколько опций, которые вы бы выбрали для базового мониторинга хоста по CPU, памяти, диску и сети.

    curl http://localhost:9100/metrics | grep cpu

CPU: user — время выполнения обычных процессов, которые выполняются в режиме пользователя (в user mode, userland);
nice — время выполнения процессов с приоритетом nice, которые выполняются в режиме пользователя;
system — время выполнения процессов, которые выполняются в режиме ядра (kernel mode);
idle — время простоя, CPU ничем не занят;
iowait — время ожидания I/O операций;
irq и softirq — время обработки аппаратных и программных прерываний;
steal — время, которое используют другие операционные системы (при виртуализации);
guest — время выполнения «гостевых» процессов (при виртуализации).

    node_cpu_seconds_total{cpu="0",mode="idle"} 1351.39
    node_cpu_seconds_total{cpu="0",mode="iowait"} 8.89
    node_cpu_seconds_total{cpu="0",mode="irq"} 0
    node_cpu_seconds_total{cpu="0",mode="nice"} 1.39
    node_cpu_seconds_total{cpu="0",mode="softirq"} 3.12
    node_cpu_seconds_total{cpu="0",mode="steal"} 0
    node_cpu_seconds_total{cpu="0",mode="system"} 54.69
    node_cpu_seconds_total{cpu="0",mode="user"} 126.57

MEM: MemTotal - количество памяти; MemFree и MemAvailable - сводобная и доступная (включая кеш) память; SwapTotal, SwapFree, SwapCached - своп, если слишком много занято -- памяти не хватает.

    node_memory_MemAvailable_bytes 7.7056e+08
    node_memory_MemFree_bytes 7.11176192e+08
    node_memory_MemTotal_bytes 1.028694016e+09
    node_memory_SwapCached_bytes 0
    node_memory_SwapFree_bytes 1.027600384e+09
    node_memory_SwapTotal_bytes 1.027600384e+09

DISK: size_bytes и avail_bytes покажут объём и свободное место; readonly=1 может говорить о проблемах ФС, из-за чего она перешла в режим только для чтения; io_now - интенсивность работы с диском в текущий момент.

    node_filesystem_avail_bytes{device="/dev/mapper/vgvagrant-root",fstype="ext4",mountpoint="/"} 6.0591689728e+10
    node_filesystem_readonly{device="/dev/mapper/vgvagrant-root",fstype="ext4",mountpoint="/"} 0
    node_filesystem_size_bytes{device="/dev/mapper/vgvagrant-root",fstype="ext4",mountpoint="/"} 6.5827115008e+10
    node_disk_io_now{device="sda"} 0

NET: carrier_down, carrier_up - если много, значит что-то с кабелем; info - общая информация по нитерфейсу; mtu_bytes - может быть важно для диагностики потерь или если трафик хостов не проходит через маршрутизатор; receive_errs_total, transmit_errs_total, receive_packets_total, transmit_packets_total - ошибки передачи, в зависимости от объёма если больше процента, вероятно какие-то проблемы сети или с хостом

    node_network_carrier_down_changes_total{device="eth0"} 1
    node_network_carrier_up_changes_total{device="eth0"} 1
    node_network_info{address="08:00:27:73:60:cf",broadcast="ff:ff:ff:ff:ff:ff",device="eth0",duplex="full",ifalias="",operstate="up"} 1
    node_network_mtu_bytes{device="eth0"} 1500
    node_network_receive_errs_total{device="eth0"} 0
    node_network_receive_packets_total{device="eth0"} 4079
    node_network_transmit_errs_total{device="eth0"} 0
    node_network_transmit_packets_total{device="eth0"} 3567

#### 3. Установите в свою виртуальную машину Netdata
в конфигурационном файле /etc/netdata/netdata.conf в секции [web] замените значение с localhost на bind to = 0.0.0.0

    19999 (guest) => 19999 (host)

    vagrant cannot forward the specified ports on this VM, since they
    would collide with some other application that is already listening
    on these ports. The forwarded port to 19999 is already in use
    on the host machine.

    To fix this, modify your current project's Vagrantfile to use another
    port. Example, where '1234' would be replaced by a unique host port:

    config.vm.network :forwarded_port, guest: 19999, host: 1234
  
    19999 (guest) => 19998 (host)

После успешной перезагрузки в браузере на своем ПК (не в виртуальной машине) вы должны суметь зайти на localhost:19999.

 
#### 4. Можно ли по выводу dmesg понять, осознает ли ОС, что загружена не на настоящем оборудовании, а на системе виртуализации?
    vagrant@vagrant:~$ sudo dmesg | grep "Hypervisor detected"
    [    0.000000] Hypervisor detected: KVM

#### 5. Как настроен sysctl fs.nr_open на системе по-умолчанию? Узнайте, что означает этот параметр. Какой другой существующий лимит не позволит достичь такого числа (ulimit --help)?
    vagrant@vagrant:~$ sysctl fs.nr_open
    fs.nr_open = 1048576

Это означает максимальное количество файловых дескрипторов, которые может выполнять процесс.
выделить. Значение по умолчанию - 1024 * 1024 (1048576), что должно хватит на большинство машин. 
Фактический лимит зависит от RLIMIT_NOFILE ограничение ресурса.
ulimit --help

Значения указаны с шагом 1024 байта, за исключением -t, который задается в секундах,
     -p с шагом 512 байт и -u немасштабируемый
     количество процессов.
     
#### 6. Запустите любой долгоживущий процесс (не ls, который отработает мгновенно, а, например, sleep 1h) в отдельном неймспейсе процессов; покажите, что ваш процесс работает под PID 1 через nsenter. Для простоты работайте в данном задании под root (sudo -i). Под обычным пользователем требуются дополнительные опции (--map-root-user) и т.д.

    #ps aux | grep sleep
    root        1231  0.0  0.4  11864  4628 pts/0    S+   19:50   0:00 sudo unshare -f --pid --mount-proc sleep 1h
    root        1232  0.0  0.0   8080   528 pts/0    S+   19:50   0:00 unshare -f --pid --mount-proc sleep 1h
    root        1233  0.0  0.0   8076   592 pts/0    S+   19:50   0:00 sleep 1h
    vagrant     1286  0.0  0.0   8900   736 pts/1    S+   19:53   0:00 grep --color=auto sleep

    # sudo nsenter -t 1233  -p -r ps -ef
    UID          PID    PPID  C STIME TTY          TIME CMD
    root           1       0  0 19:50 pts/0    00:00:00 sleep 1h
    root           2       0  0 19:55 pts/1    00:00:00 ps -ef

#### 7. Найдите информацию о том, что такое :(){ :|:& };:. Запустите эту команду в своей виртуальной машине, после чего (минуты) – ОС должна стабилизироваться. Вызов dmesg расскажет, какой механизм помог автоматической стабилизации. Как настроен этот механизм по-умолчанию, и как изменить число процессов, которое можно создать в сессии?

+ fork-бомба порождает большое количество собственных копий и тем самым пытается заполнить свободное место в списке активных процессов операционной системы. После заполнения списка процессов становится невозможным старт полезной программы. Даже если какой-либо другой процесс прекратит работу и место в списке процессов освободится, то старт полезной программы маловероятен, так как множество других
копий fork-бомбы уже ждут возможности запустить свою очередную копию.

Установленные Hard лимиты

    vagrant@vagrant:~$ ulimit -Hn
    1048576

Установленные Soft лимиты

    vagrant@vagrant:~$ ulimit -Sn
    1024

    vagrant@vagrant:~$ dmesg | grep fork
    [   33.928954] cgroup: fork rejected by pids controller in /user.slice/user-1000.slice/session-4.scope
    vagrant@vagrant:~$ dmesg | grep cgroup
    [    0.061481] *** VALIDATE cgroup1 ***
    [    0.061482] *** VALIDATE cgroup2 ***
    [   33.928954] cgroup: fork rejected by pids controller in /user.slice/user-1000.slice/session-4.scope

