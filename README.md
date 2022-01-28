1. 
```chdir("/tmp")```
2. 
```
openat(AT_FDCWD, "/usr/share/misc/magic.mgc", O_RDONLY) = 3
vagrant@vagrant:~$ file /usr/share/misc/magic.mgc
/usr/share/misc/magic.mgc: symbolic link to ../../lib/file/magic.mgc
vagrant@vagrant:`$ file /lib/file/magic.mgc
/lib/file/magic.mgc: magic binary file for file(1) cmd (version 14) (little endian)
```
3. 
```
vagrant@vagrant:~$ rm /home/vagrant/.file.swp
vagrant@vagrant:~$ lsof | grep deleted
vi        2358                        vagrant    4u      REG              253,0    12288    1048595 /home/vagrant/.file.swp (deleted)
vagrant@vagrant:~$ ls -l /proc/2358/fd | grep "/home/vagrant/.file.swp"
lrwx------ 1 vagrant vagrant 64 Jan 25 19:01 4 -> /home/vagrant/.file.swp (deleted)
vagrant@vagrant:~$ truncate -s 0 /proc/2358/fd/4
```
4. Зомби процессы ресурсов не занимают, но по прежнему имеют запись в таблице процессов.
5. 
```
root@vagrant:~# dpkg -L bpfcc-tools | grep sbin/opensnoop
/usr/sbin/opensnoop-bpfcc
root@vagrant:~# /usr/sbin/opensnoop-bpfcc
PID    COMM               FD ERR PATH
874    vminfo              6   0 /var/run/utmp
630    dbus-daemon        -1   2 /usr/local/share/dbus-1/system-services
630    dbus-daemon        20   0 /usr/share/dbus-1/system-services
630    dbus-daemon        -1   2 /lib/dbus-1/system-services
630    dbus-daemon        20   0 /var/lib/snapd/dbus-1/system-services/
```
6.
```
Part of the utsname information is also accessible via /proc/sys/kernel/{ostype, hostname, osrelease, version, domainname}.
```
7. && - выполняет следующую команду только после успешного завершения предъидущей  
; - просто разделяет последовательные команды  
set -e прерывает выполненение команды с не нулевым статусом  
&& и set -e прервут выполнение команд если предыдущая завершилась ошибкой
8. ```set -euxo pipefail```  
-e прервет выполнение при ошибке команды (кроме последней)  
-u прервет выполнение если переменная не объявлена  
-x все выполняемые команды выводятся в терминал  
-o pipefail При сбое команды в конвейере этот код возврата будет использоваться как код возврата для всего конвейера.
Интерактивный, расширенный режим "логирования" выполнения сценария.
9. S*  прерываемое ожидание. Процесс ждет наступления события.  
I* процесс бездействует.  
< — процесс с высоким приоритетом;  
N — процесс с низким приоритетом;  
l — многопоточный процесс;  
\+ — фоновый процесс;  
\+ s — лидер сессии.