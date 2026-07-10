
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

``` bash

❯ echo "10.129.42.21 nimbus.htb" | sudo tee -a /etc/hosts

10.129.42.21 nimbus.htb

```

``` bash

❯ ffuf -u "http://nimbus.htb/" -w /usr/share/wordlists/seclists/Discovery/DNS/subdomains-top1million-110000.txt -H "Host: FUZZ.nimbus.htb" -fs 178

        /'___\  /'___\           /'___\
       /\ \__/ /\ \__/  __  __  /\ \__/
       \ \ ,__\\ \ ,__\/\ \/\ \ \ \ ,__\
        \ \ \_/ \ \ \_/\ \ \_\ \ \ \ \_/
         \ \_\   \ \_\  \ \____/  \ \_\
          \/_/    \/_/   \/___/    \/_/

       v2.1.0-dev
________________________________________________

 :: Method           : GET
 :: URL              : http://nimbus.htb/
 :: Wordlist         : FUZZ: /usr/share/wordlists/seclists/Discovery/DNS/subdomains-top1million-110000.txt
 :: Header           : Host: FUZZ.nimbus.htb
 :: Follow redirects : false
 :: Calibration      : false
 :: Timeout          : 10
 :: Threads          : 40
 :: Matcher          : Response status: 200-299,301,302,307,401,403,405,500
 :: Filter           : Response size: 178
________________________________________________

aws                     [Status: 403, Size: 305, Words: 28, Lines: 8, Duration: 43ms]
:: Progress: [13760/114442] :: Job [1/1] :: 1047 req/sec :: Duration: [0:00:14] :: Errors: 0 ::

```

``` bash

❯ echo "10.129.42.21 aws.nimbus.htb" | sudo tee -a /etc/hosts

10.129.42.21 aws.nimbus.htb

```

``` bash

❯ curl -i aws.nimbus.htb
HTTP/1.1 403 FORBIDDEN
Server: nginx/1.24.0 (Ubuntu)
Date: Fri, 10 Jul 2026 18:01:22 GMT
Content-Type: text/xml; charset=utf-8
Content-Length: 305
Connection: keep-alive

<ErrorResponse xmlns="https://sts.amazonaws.com/doc/2011-06-15/">
  <Error>
    <Type>Sender</Type>
    <Code>InvalidClientTokenId</Code>
    <Message>The security token included in the request is invalid.</Message>
  </Error>
  <RequestId>p7518495-adc4-1191-49n2-4m70d8sdf922</RequestId>
</ErrorResponse>%      
<img width="715" height="573" alt="imagen" src="https://github.com/user-attachments/assets/c0516e1e-abd9-42dc-ad43-cd06c7a3abe9" />

```


<img width="686" height="240" alt="imagen" src="https://github.com/user-attachments/assets/acbe770d-1f89-476f-8591-0fac25f49a5f" />

``` bash
❯ nc -lvnp 9090
listening on [any] 9090 ...
connect to [10.10.14.54] from (UNKNOWN) [10.129.42.21] 38714
GET /test.yaml HTTP/1.1
Host: 10.10.14.54:9090
User-Agent: python-requests/2.34.2
Accept-Encoding: gzip, deflate
Accept: */*
Connection: keep-alive
```

<img width="672" height="235" alt="imagen" src="https://github.com/user-attachments/assets/d7f05726-3a13-466d-910a-48fef0c9c931" />


<img width="685" height="200" alt="imagen" src="https://github.com/user-attachments/assets/91626051-00de-4ca9-9230-ba39edf7f962" />



