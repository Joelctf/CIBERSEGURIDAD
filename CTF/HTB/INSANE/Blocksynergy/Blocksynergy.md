
``` bash
❯ recon 10.129.56.169
Starting Nmap 7.98 ( https://nmap.org ) at 2026-09-01 13:33 +0200
Nmap scan report for blocksynergy.htb (10.129.56.169)
Host is up (0.036s latency).
Not shown: 65533 closed tcp ports (reset)
PORT     STATE SERVICE
22/tcp   open  ssh
8080/tcp open  http-proxy

Nmap done: 1 IP address (1 host up) scanned in 11.84 seconds
[*] First script done
[*] Open ports = '22,8080'
Starting Nmap 7.98 ( https://nmap.org ) at 2026-09-01 13:34 +0200
Nmap scan report for blocksynergy.htb (10.129.56.169)
Host is up (0.036s latency).

PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 9.6p1 Ubuntu 3ubuntu13.18 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   256 0b:93:57:66:c8:a4:f0:85:6a:d2:e1:a4:d5:f4:52:81 (ECDSA)
|_  256 aa:38:b7:38:85:1d:21:1e:db:0a:15:8b:c8:a4:03:92 (ED25519)
8080/tcp open  http    Werkzeug httpd 3.1.3 (Python 3.12.3)
|_http-server-header: Werkzeug/3.1.3 Python/3.12.3
|_http-title: BlockSynergy \xE2\x80\x93 Decentralized Future
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 10.57 seconds
[*] Done
╭─ ~/hacking/ctf/htb/insane/BlockSynergy/recon                                                               ✔ │ 29s ─╮
╰─                                                                                                                   ─╯

```


