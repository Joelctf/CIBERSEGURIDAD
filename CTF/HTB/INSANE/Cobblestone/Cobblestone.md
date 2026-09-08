
``` bash

❯ ip="10.129.232.170"


```


``` bash

❯ recon $ip
Starting Nmap 7.98 ( https://nmap.org ) at 2026-09-08 19:31 +0200
Nmap scan report for 10.129.232.170
Host is up (0.036s latency).
Not shown: 65533 closed tcp ports (reset)
PORT   STATE SERVICE
22/tcp open  ssh
80/tcp open  http

Nmap done: 1 IP address (1 host up) scanned in 13.84 seconds
[*] First script done
[*] Open ports = '22,80'
Starting Nmap 7.98 ( https://nmap.org ) at 2026-09-08 19:31 +0200
Nmap scan report for 10.129.232.170
Host is up (0.036s latency).

PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 9.2p1 Debian 2+deb12u7 (protocol 2.0)
| ssh-hostkey:
|   256 50:ef:5f:db:82:03:36:51:27:6c:6b:a6:fc:3f:5a:9f (ECDSA)
|_  256 e2:1d:f3:e9:6a:ce:fb:e0:13:9b:07:91:28:38:ec:5d (ED25519)
80/tcp open  http    Apache httpd 2.4.62
|_http-server-header: Apache/2.4.62 (Debian)
|_http-title: Did not follow redirect to http://cobblestone.htb/
Service Info: Host: 127.0.0.1; OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 8.65 seconds
[*] Done
╭─ ~/hacking/ctf/htb/insane/Cobblestone/recon                                                                ✔ │ 29s ─╮
╰─                                                                                                                   ─╯

```

``` bash

❯ echo "$ip cobblestone.htb" | sudo tee -a /etc/hosts
10.129.232.170 cobblestone.htb
╭─ ~/hacking/ctf/htb/insane/Cobblestone/recon                                                                      ✔ ─╮
╰─                                                                                                                   ─╯

```
