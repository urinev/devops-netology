1. CD втроенная команда bash. CD изменяет текущий каталог среды выполнения оболочки.
2. ```grep <some_string> <some_file> -c``` посчитает количество совпадений в файле
3. ```vagrant@vagrant:~$ pstree -up```
```
systemd(1)─┬─VBoxService(824)─┬─{VBoxService}(825)
           │                  ├─{VBoxService}(826)
           │                  ├─{VBoxService}(827)
           │                  ├─{VBoxService}(828)
           │                  ├─{VBoxService}(829)
           │                  ├─{VBoxService}(830)
           │                  ├─{VBoxService}(831)
           │                  └─{VBoxService}(832)
           ├─accounts-daemon(623)─┬─{accounts-daemon}(627)
           │                      └─{accounts-daemon}(683)
```
4. ```ls 2>/dev/pts1```
5. 
```vagrant@vagrant:~$ ls
file
vagrant@vagrant:~$ cat file
test
vagrant@vagrant:~$ ls
file
vagrant@vagrant:~$ cat file
test
vagrant@vagrant:~$ cat <file>new
vagrant@vagrant:~$ ls
file  new
vagrant@vagrant:~$ cat new
test
```
6. Получится: ```echo hello from pts0 to tty1 >/dev/tty1```
7. ```bash 5>&1``` добавит дескриптор + перенаправит в stdout
    * ```echo netology > /proc/$$/fd/5``` выведет "netology"` т.к. /proc/$$/fd/5 > /proc/$$/fd/1
8. ```5>&2 2>&1 1>&5``` (новый дескриптор(5) в stderr, stderr в stdout, stdout в новый дескриптор(5))
9. ```cat /proc/$$/environ``` отображает переменные окружения. ```env``` ```printenv```
10. ```/proc/<PID>/cmdline``` файл только для чтения, содержит полный путь до процесса, если "зомби" то 0.
```/proc/<PID>/exe``` символическая ссылка, фактический путь к выполняемому файлу. cat-посмотреть содержимое. Если выполнить то запустит копию файла.
11. ```grep sse /proc/cpuinfo```  sse4_2
12. 
```
vagrant@vagrant:~$ ssh -t localhost 'tty'
vagrant@localhost's password:
/dev/pts/1
Connection to localhost closed.
``` 
14. 
```
vagrant@vagrant:~$ sudo sysctl kernel.yama.ptrace_scope=0
kernel.yama.ptrace_scope = 0
vagrant@vagrant:~$ ps -a
    PID TTY          TIME CMD
   1118 tty1     00:00:00 bash
   1164 tty1     00:00:00 top
   1165 pts/0    00:00:00 ps
vagrant@vagrant:~$ reptyr 1164
```
14. команда ```tee``` запущеная с правами su, одновременно выводит и на экран и в фавйл /root/new_file