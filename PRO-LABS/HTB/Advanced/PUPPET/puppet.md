

Contexto: En esta mini pro lab se simula un atacke donde tendremos que comprometer 3 maquinas en la red empresarial llamada puppet inc. 

La red ha sido previamente comprometida por phising a la maquina de acceso inicial donde tenia una conexion a un command and control , en este caso para simular la maquina de acceso inicial tiene un Servidor C2 ya comprometido al que nosotros deberemos acceder para comenzar a comprometer la red 

Imagen representativa de la red :

<img width="1408" height="768" alt="Gemini_Generated_Image_niqobuniqobuniqo" src="https://github.com/user-attachments/assets/b106213f-d394-491d-8df0-7a0663033704" />


Reconocimiento , Acceso inicial y conexion al Command and Control:

``` bash

   client   git:(master)   nmap -p 21,22,8140,8443,8888,31337 -T4 -sV 10.13.38.33
Starting Nmap 7.98 ( https://nmap.org ) at 2026-03-06 23:40 +0100
Stats: 0:00:06 elapsed; 0 hosts completed (1 up), 1 undergoing Service Scan
Service scan Timing: About 40.00% done; ETC: 23:40 (0:00:09 remaining)
Stats: 0:00:11 elapsed; 0 hosts completed (1 up), 1 undergoing Service Scan
Service scan Timing: About 60.00% done; ETC: 23:40 (0:00:07 remaining)
Nmap scan report for 10.13.38.33
Host is up (0.035s latency).

PORT      STATE  SERVICE        VERSION
21/tcp    open   ftp            vsftpd 3.0.5
22/tcp    open   ssh            OpenSSH 8.9p1 Ubuntu 3ubuntu0.11 (Ubuntu Linux; protocol 2.0)
8140/tcp  open   ssl/http       WEBrick httpd 1.7.0 (Ruby 3.0.2 (2021-07-07); OpenSSL 3.0.2)
8443/tcp  open   ssl/https-alt?
8888/tcp  closed sun-answerbook
31337/tcp open   ssl/Elite?
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel
```

El servidor FTP accepta conexion como anonymous sin contraseña y dentro tiene los archivos para poder conectarse al Command and control:

``` bash

   client   git:(master)  curl ftp://10.13.38.33/ --user anonymous:
-rw----r--    1 0        0            2119 Oct 11  2024 red_127.0.0.1.cfg
-rwxr-xr-x    1 0        0        36515304 Oct 12  2024 sliver-client_linux

```

Lo descargamos:

``` bash

   programas curl -O ftp://10.13.38.33/red_127.0.0.1.cfg --user anonymous:
curl -O ftp://10.13.38.33/sliver-client_linux --user anonymous:
  % Total    % Received % Xferd  Average Speed  Time    Time    Time   Current
                                 Dload  Upload  Total   Spent   Left   Speed
100   2119 100   2119   0      0   5343      0                              0
  % Total    % Received % Xferd  Average Speed  Time    Time    Time   Current
                                 Dload  Upload  Total   Spent   Left   Speed
  0      0   0      0   0      0      0      0                              0
curl: (56) Recv failure: Connection reset by peer
   programas ls
red_127.0.0.1.cfg  sliver  sliver-client_linux
   programas

``` bash

❯ curl -O ftp://10.13.38.33/sliver-client_linux --user anonymous
Enter host password for user 'anonymous':
  % Total    % Received % Xferd  Average Speed  Time    Time    Time   Current
                                 Dload  Upload  Total   Spent   Left   Speed
100 34.82M 100 34.82M   0      0  8.95M      0   00:03   00:03          9.26M

```


``` bash

❯ cat red_127.0.0.1.cfg
{"operator":"red","token":"REDACTED","lhost":"10.13.38.33","lport":31337,"ca_certificate":"-----BEGIN CERTIFICATE-----\nMIICJjCCAYegAwIBAgIRALAbBDS3DFS4TF3CSDVSdsdgfsggLmSMwCgYIKoZIzj0EAwQwFDES\nMBAGA1UEAxMJb3BlcmF0b3JzMB4XDTIzMTIyMjEyMjQ1OVoXDTI2MTIyMTEyMjQ1\nOVowFDESMBAGA1UEAxMJb3BlcmF0b3JzMIGbMBAGByqGSM49AgEGBSuBBAAjA4GG\nAAQAvedDJyjbi1l9OzQvw2IOAx8RVwsjUr+YVDuJ1cG3Hcpt//uSXlCp6/BnsArr\n4V8a59m6MRLg5M6+CEoJWnYTAQ4BmQn6/izlEWpcSUv6VGhNlZRG8P3MpbN2M0cV\nprZ5SFL3SAcXmQWENES/DhkNMT8sf4IwgTM+RA95YXXXwvY9Z/CjdzB1MA4GA1Ud\nDwEB/wQEAwICpDAdBgNVHSUEFjAUBggrBgEFBQcDAQYIKwYBBQUHAwIwDwYDVR0T\nAQH/BAUwAwEB/zAdBgNVHQ4EFgQUd7jHTZN0eWYzJ8Nt/va/fHFc1zgwFAYDVR0R\nBA0wC4IJb3BlcmF0b3JzMAoGCCqGSM49BAMEA4GMADCBiAJCARgKKjMFUmd8+tkR\nAJUH30ZpBuSHcDMPYsDaSstgVva1jzn9sI9Dlg5dpRU+8LaK2FXsXUCdlLaYrzIv\np7anvR5CAkIBmk//V/6OV0e1YQcAtg6vL1dBTWPPk6YpLJEicwm6q5DGWMNNHTd8\nhtyfLIKpSsaVXHJjH7kqIbmbuY86TpQo6X0=\n-----END CERTIFICATE-----\n","private_key":"-----BEGIN EC PRIVATE KEY-----\nMIHcAgEBBEIB/vSoY+G1wyjB1xfYo+LpZ9ov7hkQOePJrmq0rznSa/HPRraYjwLZ\nVfdssdf3rvcvñlgdk8QBYAgOgBwYFK4EEACOhgYkDgYYABACp3pUH\nvLKFjb3z/0/IhcHjgfoSKsXCoLuzprckfJfBmI03DP+2uKNqi6V5bpZkzfWWfYDh\nmjXjfY/nPR3lGVL4fwE5ftQMmGffEUaSlZ/MyEQQwZo/oUs6OiTdw0S4aa141bDG\n54CXsdaceGN98H9V1Yrv27S4jFH1D3VEUrCJbkrU5Q==\n-----END EC PRIVATE KEY-----\n","certificate":"-----BEGIN CERTIFICATE-----\nMIIB7zCCAVGgAwIBAgIQL7uHbxTos3ke9pRfj7CXwDAKBggqhkjOPQQDBDAUMRIw\nEAYDVQQDEwlvcGVyYXRvcnMwHhcNMjQwMTMwMTIzMjIyWhcNMjcwMTI5MTIzMjIy\nWjAOMQwwCgYDVQQDEwNyZWQwgZswEAYHKoZIzj0CAQYFK4EEACMDgYYABACp3pUH\nvLKFjb3z/0/IhcHjgfoSKsXCoLuzprckfJfBmI03DP+2uKNqi6V5bpZkzfWWfYDh\nmjXjfY/nPR3lGVL4fwE5ftQMmGffEUaSlZ/MyEQQwZo/oUs6OiTdw0S4aa141bDG\n54CXsdaceGN98H9V1Yrv27S4jFH1D3VEUrCJbkrU5aNIMEYwDgYDVR0PAQH/BAQD\nAgWgMBMGA1UdJQQMMAoGCCsGAQUFBwMCMB8GA1UdIwQYMBaAFHe4x02TdHlmMyfD\nbf72v3xxXNc4MAoGCCqGSM49BAMEA4GLADCBhwJCAQrEErqmcDVO22Ze6caAd5+F\n4nrwq/o1NC1nNODRspipprdjB4/vQMt98PiA2cO9Ayql33rHBNky4IweHdieD4Ws\nAkFQEbWoqRsVhxGAcqmdLI76PyazW1pMi5Rge0UMLQ4mxB4lQ+yKS9qu5pWx3WKz\nsXraOydUfKNpOYdscD/i2TX7fg==\n-----END CERTIFICATE-----\n"}%                                      ╭─ ~/hacking/ctf/htb/pro-lab/puppet                                                                                ✔ ─╮
╰─                                                                                                                   ─╯

```




