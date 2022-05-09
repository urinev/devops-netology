# 5.2. Применение принципов IaaC в работе с виртуальными машинами

## 1. Преимущества применения IaaC
- Одинаковая инфраструктура у всей команды. Использование шаблонов увеличивает скорость и эффективность развертывания инфраструктуры, как следствие сокращается стоимость обслуживания. Использование систем контроля управления версиями уменьшают вероятность появления ошибок.  
- Основной принцип iac это описание инфраструктуры кодом не прибегая к ручному конфигурированию.

## 2. Ansible
- Ansible использует SSH инфраструктуру.
- На мой взгляд более надежный метод push т.к. главную машину можно не делать общедоступной.

## 3. Установить на личный компьютер:
- VirtualBOX
```
.\VBoxManage.exe --version
6.1.32r149290
```
- Vagrant
```
vagrant.exe --version
Vagrant 2.2.19
```
- Ansible
```
$ ansible --version
ansible 2.9.6
  config file = /etc/ansible/ansible.cfg
  configured module search path = ['/home/been/.ansible/plugins/modules', '/usr/share/ansible/plugins/modules']
  ansible python module location = /usr/lib/python3/dist-packages/ansible
  executable location = /usr/bin/ansible
  python version = 3.8.10 (default, Mar 15 2022, 12:22:08) [GCC 9.4.0]
```
## 4. Воспроизвести практическую часть лекции самостоятельно.