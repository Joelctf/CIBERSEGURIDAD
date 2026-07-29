
``` bash

#!/bin/bash

ip=$1
current_dir=$(pwd)

if [ $# -ne 1 ]; then
    echo "Usage: $0 <ip>"
    exit 1
fi

sleep 2

nmap -p- -sS -Pn --min-rate 5000 "$ip" -oN "$current_dir"/all_ports.txt 2>/dev/null

echo -e "\e[32m[*] First script done\e[0m"

sleep 2

ports=$(grep "^[0-9]" "$current_dir"/all_ports.txt | cut -d "/" -f1 | paste -sd "," -)

sleep 1

echo -e "\e[32m[*] Open ports = '$ports'\e[0m"

sleep 1

nmap -p "$ports" -sCV -Pn --min-rate 5000 "$ip" -oN "$current_dir"/version_ports.txt 2>/dev/null

echo -e "\e[32m[*] Done\e[0m"

```

``` bash

❯ recon 10.129.229.66
Starting Nmap 7.98 ( https://nmap.org ) at 2026-07-30 01:08 +0200
Nmap scan report for 10.129.229.66
Host is up (0.039s latency).
Not shown: 65533 closed tcp ports (reset)
PORT   STATE SERVICE
22/tcp open  ssh
80/tcp open  http

Nmap done: 1 IP address (1 host up) scanned in 13.48 seconds
[*] First script done
[*] Open ports = '22,80'
Starting Nmap 7.98 ( https://nmap.org ) at 2026-07-30 01:08 +0200
Nmap scan report for 10.129.229.66
Host is up (0.036s latency).

PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.9p1 Ubuntu 3ubuntu0.1 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   256 3e:ea:45:4b:c5:d1:6d:6f:e2:d4:d1:3b:0a:3d:a9:4f (ECDSA)
|_  256 64:cc:75:de:4a:e6:a5:b4:73:eb:3f:1b:cf:b4:e3:94 (ED25519)
80/tcp open  http    nginx
|_http-title: Did not follow redirect to http://2million.htb/
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 8.57 seconds
[*] Done
╭─ ~/hacking/ctf/htb/easy/twomillion/recon                                                                   ✔ │ 28s ─╮
╰─                                                                                                                   ─╯

```

``` bash

❯ echo '10.129.229.66 2million.htb' | sudo tee -a /etc/hosts
10.129.229.66 2million.htb
╭─ ~/hacking/ctf/htb/easy/twomillion/recon                                                                         ✔ ─╮
╰─                                                                                                                   ─╯

```

<img width="955" height="592" alt="image" src="https://github.com/user-attachments/assets/1a73ba85-b5a7-4e37-af1d-59fbf6d9fc23" />


<img width="593" height="535" alt="image" src="https://github.com/user-attachments/assets/2b9f3eb8-9939-47d2-981d-aef20f6264b9" />

