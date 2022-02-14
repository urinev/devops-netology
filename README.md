1. 
```buildoutcfg
root@vagrant:~# telnet stackoverflow.com 80
Trying 151.101.129.69...
Connected to stackoverflow.com.
Escape character is '^]'.
GET /questions HTTP/1.0
HOST: stackoverflow.com

HTTP/1.1 301 Moved Permanently
cache-control: no-cache, no-store, must-revalidate
location: https://stackoverflow.com/questions
x-request-guid: e8170ecc-1bc2-4b44-8226-4c1d60c1723f
feature-policy: microphone 'none'; speaker 'none'
content-security-policy: upgrade-insecure-requests; frame-ancestors 'self' https://stackexchange.com
Accept-Ranges: bytes
Date: Thu, 10 Feb 2022 13:44:46 GMT
Via: 1.1 varnish
Connection: close
X-Served-By: cache-bma1669-BMA
X-Cache: MISS
X-Cache-Hits: 0
X-Timer: S1644500687.614633,VS0,VE101
Vary: Fastly-SSL
X-DNS-Prefetch-Control: off
Set-Cookie: prov=4349b6ef-033e-c648-f3d1-56d2917a17a4; domain=.stackoverflow.com; expires=Fri, 01-Jan-2055 00:00:00 GMT; path=/; HttpOnly

Connection closed by foreign host.
```
2. 
```buildoutcfg
HTTP/1.1 307 Internal Redirect
Location: https://stackoverflow.com/
Non-Authoritative-Reason: HSTS
```
3. 79.120.12.68
4. Net By Net Holding LLC AS12714
5. 
```buildoutcfg
2  212.1.254.230 [AS12714]  37.032 ms  4.997 ms  37.002 ms
 3  212.1.251.214 [AS12714]  5.461 ms  6.884 ms  4.962 ms
 4  212.1.251.0 [AS12714]  4.026 ms 212.1.251.113 [AS12714]  3.999 ms 212.1.239.145 [AS12714]  6.098 ms
 5  72.14.215.172 [AS15169]  5.054 ms  5.039 ms  5.039 ms
 6  * * *
 7  108.170.250.33 [AS15169]  5.620 ms 72.14.239.130 [AS15169]  4.491 ms 172.253.69.166 [AS15169]  5.346 ms
 8  108.170.250.34 [AS15169]  5.575 ms 108.170.250.130 [AS15169]  6.188 ms 108.170.250.113 [AS15169]  4.543 ms
 9  172.253.66.116 [AS15169]  21.465 ms * *
10  172.253.66.110 [AS15169]  17.479 ms 74.125.253.109 [AS15169]  18.921 ms 72.14.238.168 [AS15169]  21.908 ms
11  216.239.49.107 [AS15169]  19.228 ms 172.253.51.223 [AS15169]  20.736 ms 172.253.64.57 [AS15169]  23.860 ms
12  * * *
13  * * *
14  * * *
15  * * *
16  * * *
17  * * *
18  * * *
19  * * *
20  * * *
21  8.8.8.8 [AS15169]  20.901 ms  19.332 ms  21.639 ms
```
6. 
```buildoutcfg
AS15169  216.239.57.222                                                   0.0%    83   22.0  21.9  21.4  27.6   0.8
AS15169  172.253.64.53                                                    0.0%    83   20.8  21.2  20.8  25.3   0.6
```
7. 
```buildoutcfg
;; AUTHORITY SECTION:
.                       91056   IN      NS      l.root-servers.net.
.                       91056   IN      NS      c.root-servers.net.
.                       91056   IN      NS      d.root-servers.net.
.                       91056   IN      NS      g.root-servers.net.
.                       91056   IN      NS      m.root-servers.net.
.                       91056   IN      NS      b.root-servers.net.
.                       91056   IN      NS      k.root-servers.net.
.                       91056   IN      NS      h.root-servers.net.
.                       91056   IN      NS      e.root-servers.net.
.                       91056   IN      NS      i.root-servers.net.
.                       91056   IN      NS      a.root-servers.net.
.                       91056   IN      NS      f.root-servers.net.
.                       91056   IN      NS      j.root-servers.net.

;; ADDITIONAL SECTION:
l.root-servers.net.     297342  IN      A       199.7.83.42
c.root-servers.net.     297342  IN      A       192.33.4.12
d.root-servers.net.     297342  IN      A       199.7.91.13
g.root-servers.net.     297342  IN      A       192.112.36.4
m.root-servers.net.     297342  IN      A       202.12.27.33
b.root-servers.net.     297342  IN      A       199.9.14.201
k.root-servers.net.     297342  IN      A       193.0.14.129
h.root-servers.net.     297342  IN      A       198.97.190.53
e.root-servers.net.     297342  IN      A       192.203.230.10
i.root-servers.net.     297342  IN      A       192.36.148.17
a.root-servers.net.     297342  IN      A       198.41.0.4
f.root-servers.net.     297342  IN      A       192.5.5.241
j.root-servers.net.     297342  IN      A       192.58.128.30
```
8. 
```buildoutcfg
debian:~$ dig -x 192.58.128.30 @8.8.8.8

; <<>> DiG 9.16.22-Debian <<>> -x 192.58.128.30 @8.8.8.8
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 33358
;; flags: qr rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 512
;; QUESTION SECTION:
;30.128.58.192.in-addr.arpa.    IN      PTR

;; ANSWER SECTION:
30.128.58.192.in-addr.arpa. 21390 IN    PTR     j.root-servers.net.

;; Query time: 16 msec
;; SERVER: 8.8.8.8#53(8.8.8.8)
;; WHEN: Mon Feb 14 22:43:24 MSK 2022
;; MSG SIZE  rcvd: 87
```