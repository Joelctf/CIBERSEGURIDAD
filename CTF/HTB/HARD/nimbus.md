
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
❯ cat version_ports.txt
# Nmap 7.98 scan initiated Tue Jun 23 17:15:43 2026 as: /usr/lib/nmap/nmap --privileged -p 22,80 -sCV -Pn --min-rate 5000 -oN /home/joel/hacking/ctf/htb/hard/nimbus/recon/version_ports.txt 10.129.28.44
Nmap scan report for 10.129.28.44
Host is up (0.036s latency).

PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 9.6p1 Ubuntu 3ubuntu13.16 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   256 eb:ab:8f:be:99:02:0b:3e:c4:1c:83:b2:66:2f:17:13 (ECDSA)
|_  256 c1:69:ab:84:f3:88:8b:b3:8a:ae:e2:28:35:54:35:0b (ED25519)
80/tcp open  http    nginx 1.24.0 (Ubuntu)
|_http-title: Did not follow redirect to http://nimbus.htb/
|_http-server-header: nginx/1.24.0 (Ubuntu)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Tue Jun 23 17:15:51 2026 -- 1 IP address (1 host up) scanned in 8.43 seconds
╭─ ~/hacking/ctf/htb/hard/nimbus/recon                                                                             ✔ ─╮
╰─                                                                                                                   ─╯
```
