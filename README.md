5. v.memory = 1024	v.cpus = 2
6. config.vm.provider "virtualbox" do |v|
   * v.memory = 2048
   * v.cpus = 4
   * end
7. HISTSIZE 624 ignoreboth "игнорирует" пробелы и дубли
8. line 206 позволяют объединить несколько операторов в один составной, то, что находится между фигурными скобками - выполняется в контексте текущей оболочки.
9. touch file{1..100000} getconf ARG_MAX слишком длинный список аргументов
10. [[ -d /tmp ]] проверяет наличие каталога /tmp
11. 
* vagrant@vagrant:~$ mkdir /tmp/new_path_dir/
* vagrant@vagrant:~$ cp /bin/bash /tmp/new_path_dir/
* vagrant@vagrant:~$ type -a bash
* bash is /usr/bin/bash
* bash is /bin/bash
* vagrant@vagrant:~$ PATH=/tmp/new_path_dir/:$PATH
* vagrant@vagrant:~$ type -a bash
* bash is /tmp/new_path_dir/bash
* bash is /usr/bin/bash
* bash is /bin/bash
12. это утилита командной строки, которая позволяет планировать выполнение команд в определенное время. Задания, созданные с помощью at, выполняются только один раз.
batch или его псевдоним at -b. По умолчанию задания выполняются, когда средняя загрузка системы ниже 1,5.
