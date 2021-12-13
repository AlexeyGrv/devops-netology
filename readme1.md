1 Это встроенная команда Bash и меняет текущую папку только для оболочки, в которой выполняется. Если бы эта команда была внешней, то было бы просто очень неудобно

2 grep <some_string> <some_file> -c

3 systemd(1)─┬─VBoxService(786)

4 ls /abrvalg 2>/dev/pts/1 
  ls /abrvalg &>/dev/pts/1 
  ls: невозможно получить доступ к '/abrvalg': Нет такого файла или каталога

5 
  ~/test$ cat test
  line
  new line
  second line
  ~/test$ cat < test > out
  ~/test$ cat out
  line
  new line
  second line

6 ~$ tty
  /dev/tty3
  ~$ echo netology 1>/dev/pts/0
  ~$ tty
  /dev/pts/0
  ~$ netology

7 bash 5>&1 создали файловый дескриптор т.е открытый файл, который является идентификатором потока ввода-вывода и перенаправили в stdout
  lexeygrv@alexeygrv-G771JW:~/vg-project$ ls -l /proc/$$/fd/
  итого 0
  lrwx------ 1 alexeygrv alexeygrv 64 дек  5 22:59 0 -> /dev/pts/0
  lrwx------ 1 alexeygrv alexeygrv 64 дек  5 22:59 1 -> /dev/pts/0
  lrwx------ 1 alexeygrv alexeygrv 64 дек  5 22:59 2 -> /dev/pts/0
  lrwx------ 1 alexeygrv alexeygrv 64 дек  5 22:59 255 -> /dev/pts/0
  lrwx------ 1 alexeygrv alexeygrv 64 дек  5 22:59 5 -> /dev/pts/0
  
  Что будет, если вы выполните echo netology > /proc/$$/fd/5
  
  alexeygrv@alexeygrv-G771JW:~/vg-project$ echo netology > /proc/$$/fd/5
  netology
  
8 vagrant@vagrant:~$ ls -l /proc/$$/fd/
  total 0
  lrwx------ 1 vagrant vagrant 64 Dec  7 17:17 0 -> /dev/pts/0
  lrwx------ 1 vagrant vagrant 64 Dec  7 17:17 1 -> /dev/pts/0
  lrwx------ 1 vagrant vagrant 64 Dec  7 17:17 2 -> /dev/pts/0
  lrwx------ 1 vagrant vagrant 64 Dec  7 17:17 255 -> /dev/pts/0
  lrwx------ 1 vagrant vagrant 64 Dec  7 17:17 4 -> /dev/pts/0
 
  vagrant@vagrant:~$ ls -l /home 4>&2 2>&1 1>&4 |grep vg* -c
  total 4
  drwxr-xr-x 4 vagrant vagrant 4096 Nov 29 20:37 vagrant
  0

9 cat /proc/$$/environ переменные окружения для данного процесса
  возможно printenv
  
10 /proc/<PID>/cmdline полный путь до исполняемого процесса 
   /proc/<PID>/exe является символьной ссылкой, содержащую фактический путь к выполняемой команде

11 Узнайте, какую наиболее старшую версию набора инструкций SSE поддерживает ваш процессор с помощью /proc/cpuinfo
   grep sse /proc/cpuinfo
   sse 4.2
   
12 vagrant@netology1:~$ ssh localhost 'tty'
   not a tty
   возможно после vagrant ssh tty уже определены для текущего пользователя как для localhost /dev/pts/0 так и виртуальной машины /dev/pts/ 1 
   и нужно подключаться под другим пользователем
   * -t команда исполняется c принудительным созданием псевдотерминала
13
   редактируем файл /etc/sysctl.d/10-ptrace.conf kernel.yama.ptrace_scope = 0
   sudo reptyr -T <PID_NUMBER>

14  команда tee делает вывод одновременно и в файл, указаный в качестве параметра, и в stdout, 
    в данном примере команда получает вывод из stdin, перенаправленный через pipe от stdout команды echo
    и так как команда запущена от sudo , соотвественно имеет права на запись в файл

  


