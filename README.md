1. start/stop/restart работает, после перезагрузки запускается.
```
vagrant@vagrant:~$ sudo systemctl enable node_exporter
Created symlink /etc/systemd/system/default.target.wants/node_exporter.service → /etc/systemd/system/node_exporter.service.
```
```buildoutcfg
vagrant@vagrant:~$ cat /etc/systemd/system/node_exporter.service
[Unit]
Description=node_exporter

[Service]
ExecStart=/usr/local/bin/node_exporter $MY_OPTYONS
EnvironmentFile=/etc/default/node_exporter

[Install]
WantedBy=default.target
```
2. CPU:
```
node_cpu_seconds_total{cpu="0",mode="idle"} 127918.21
node_cpu_seconds_total{cpu="0",mode="system"} 81.86
node_cpu_seconds_total{cpu="0",mode="user"} 16.48
```
Mem:
```
node_memory_MemAvailable_bytes 1.766096896e+09
node_memory_MemFree_bytes 1.240457216e+09
```
Disk:
```
node_disk_io_time_seconds_total{device="dm-0"} 41.484
node_disk_io_time_seconds_total{device="sda"} 41.9
node_disk_io_time_weighted_seconds_total{device="dm-0"} 102.62
node_disk_io_time_weighted_seconds_total{device="sda"} 44.528
node_disk_read_time_seconds_total{device="dm-0"} 18.712
node_disk_read_time_seconds_total{device="sda"} 14.428
node_disk_write_time_seconds_total{device="dm-0"} 83.908
node_disk_write_time_seconds_total{device="sda"} 60.869
```
Net:
```
node_network_receive_bytes_total{device="eth0"} 354843
node_network_speed_bytes{device="eth0"} 1.25e+08
node_network_transmit_bytes_total{device="eth0"} 334618
```
3. Netdata установлен. По ссылке localhost:19999 перешел.
```
vagrant@vagrant:~$ sudo lsof -i :19999
COMMAND PID    USER   FD   TYPE DEVICE SIZE/OFF NODE NAME
netdata 641 netdata    4u  IPv4  24668      0t0  TCP *:19999 (LISTEN)
netdata 641 netdata   53u  IPv4  31829      0t0  TCP vagrant:19999->_gateway:63474 (ESTABLISHED)
```
4. OS осознает чт запущена в vm ```dmesg | grep virt``` или ```grep vm```
5. 
```
vagrant@vagrant:~$ sysctl fs.nr_open
fs.nr_open = 1048576
```
```
vagrant@vagrant:~$ ulimit -Sn
1024
```
```
vagrant@vagrant:~$ ulimit -Hn
1048576
```
Можно увеличить ```sysctl -w fs.nr_open=10000000```
6.
```buildoutcfg
root@vagrant:~# ps -a |grep sleep
   1768 pts/3    00:00:00 sleep
root@vagrant:~# nsenter --target 1768 --pid --mount
root@vagrant:/# ps
    PID TTY          TIME CMD
      2 pts/4    00:00:00 bash
     13 pts/4    00:00:00 ps
```
7. :(){ :|:& };: Создает функцию и запускае её. Функция запускает два своих экземпляра и каждый запускает ещё по два.
```buildoutcfg
[ 3093.539011] cgroup: fork rejected by pids controller in /user.slice/user-1000.slice/session-1.scope
vagrant@vagrant:~$ ulimit -u
7597
vagrant@vagrant:~$ ulimit -u 50
vagrant@vagrant:~$ ulimit -u
50
```