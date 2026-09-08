
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

``` bash

❯ ffuf -u http://cobblestone.htb/ -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-110000.txt -H "Host: FUZZ.cobblestone.htb" -fw 18

        /'___\  /'___\           /'___\
       /\ \__/ /\ \__/  __  __  /\ \__/
       \ \ ,__\\ \ ,__\/\ \/\ \ \ \ ,__\
        \ \ \_/ \ \ \_/\ \ \_\ \ \ \ \_/
         \ \_\   \ \_\  \ \____/  \ \_\
          \/_/    \/_/   \/___/    \/_/

       v2.1.0-dev
________________________________________________

 :: Method           : GET
 :: URL              : http://cobblestone.htb/
 :: Wordlist         : FUZZ: /usr/share/seclists/Discovery/DNS/subdomains-top1million-110000.txt
 :: Header           : Host: FUZZ.cobblestone.htb
 :: Follow redirects : false
 :: Calibration      : false
 :: Timeout          : 10
 :: Threads          : 40
 :: Matcher          : Response status: 200-299,301,302,307,401,403,405,500
 :: Filter           : Response words: 18
________________________________________________

vote                    [Status: 302, Size: 81, Words: 10, Lines: 4, Duration: 42ms]
deploy                  [Status: 200, Size: 1745, Words: 121, Lines: 52, Duration: 37ms]
:: Progress: [114442/114442] :: Job [1/1] :: 826 req/sec :: Duration: [0:01:51] :: Errors: 0 ::
╭─ ~/hacking/ctf/htb/insane/Cobblestone/recon                                                             ✔ │ 1m 51s ─╮
╰─                                                                                                                   ─╯

```

``` bash

❯ echo "$ip cobblestone.htb vote.cobblestone.htb deploy.cobblestone.htb" | sudo tee -a /etc/hosts
10.129.232.170 cobblestone.htb vote.cobblestone.htb deploy.cobblestone.htb
╭─ ~/hacking/ctf/htb/insane/Cobblestone/recon                                                                      ✔ ─╮
╰─                                                                                                                   ─╯

```
