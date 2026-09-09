
``` bash

❯ ip="10.129.92.31"

```

``` bash

❯ recon $ip
Starting Nmap 7.98 ( https://nmap.org ) at 2026-09-09 03:17 +0200
Nmap scan report for silenium.htb (10.129.92.31)
Host is up (0.036s latency).
Not shown: 65533 closed tcp ports (reset)
PORT   STATE SERVICE
22/tcp open  ssh
80/tcp open  http

Nmap done: 1 IP address (1 host up) scanned in 10.86 seconds
[*] First script done
[*] Open ports = '22,80'
Starting Nmap 7.98 ( https://nmap.org ) at 2026-09-09 03:17 +0200
Nmap scan report for silenium.htb (10.129.92.31)
Host is up (0.036s latency).

PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 9.6p1 Ubuntu 3ubuntu13.15 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   256 0c:4b:d2:76:ab:10:06:92:05:dc:f7:55:94:7f:18:df (ECDSA)
|_  256 2d:6d:4a:4c:ee:2e:11:b6:c8:90:e6:83:e9:df:38:b0 (ED25519)
80/tcp open  http    nginx 1.24.0 (Ubuntu)
|_http-title: Did not follow redirect to http://silentium.htb/
|_http-server-header: nginx/1.24.0 (Ubuntu)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 8.14 seconds
[*] Done
╭─ ~/hacking/ctf/htb/easy/silenium/recon                                                                     ✔ │ 25s ─╮
╰─                                                                                                                   ─╯

```
