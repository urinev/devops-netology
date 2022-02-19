1. 
```netsh interface show interface``` ```ipconfig /all``` ```Get-NetIPConfiguration -Detailed -All``` ```Get-NetAdapter```  
```ipaddr``` ```ip link``` ```ip -s link``` ```cat /proc/net/dev``` ```cat /proc/net/dev``` ```cat /proc/net/dev```   
2. Address Resolution Protocol ```ip neigh show``` ```ip neigh add 192.168.0.5 lladdr aa:aa:aa:aa:aa:aa dev enp6s0``` ``` ip neigh del dev enp6s0 192.168.0.5```  
3. IEEE 802.1Q  
```buildoutcfg
auto enp6s0.55
iface enp6s0.55 inet static
    address 192.168.22.10
    netmask 255.255.255.255
    gateway 172.20.22.1
    dns-nameservers 8.8.8.8
```
4. Round Robin, Active Backup, XOR [exclusive OR], Broadcast, Dynamic Link Aggregation, Transmit Load Balancing (TLB), Adaptive Load Balancing (ALB)  
lacp_rate, xmit_hash_policy, miimon
```buildoutcfg
auto enp2s0
    iface enp2s0 inet manual

auto enp2s1
    iface enp2s1 inet manual

auto bond0
    iface bond0 inet static
    address 10.10.10.1/24
    gateway 10.10.10.254
    dns-nameservers 10.10.10.10 10.10.10.11
    slaves enp2s0 enp2s1
    bond-mode 802.3ad
    bond-lacp-rate slow
```
5. 10.10.10.0/29 8 IP, 10.10.10.0 адрес сети, 10.10.10.7 широковещательный, 10.10.10.1-6 IP хостов  
10.10.10.0/24 32 сети /29
6. 100.64.0.0/10 на 40-50 хостов минимально /26 255.255.255.192
7. Windows: ```arp -a``` ```arp -d``` ```ipconfig /flushdns```  ```netsh interface ip delete arpcashe``` Linux: ```ip neigh del 192.168.0.1 dev eth0``` ```ip -s -s neigh flush all```
