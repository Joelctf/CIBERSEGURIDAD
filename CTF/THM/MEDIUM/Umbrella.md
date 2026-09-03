``` bash

❯ recon 10.113.161.192
Starting Nmap 7.98 ( https://nmap.org ) at 2026-09-03 10:13 +0200
Nmap scan report for 10.113.161.192
Host is up (0.039s latency).
Not shown: 65531 closed tcp ports (reset)
PORT     STATE SERVICE
22/tcp   open  ssh
3306/tcp open  mysql
5000/tcp open  upnp
8080/tcp open  http-proxy

Nmap done: 1 IP address (1 host up) scanned in 11.82 seconds
[*] First script done
[*] Open ports = '22,3306,5000,8080'
Starting Nmap 7.98 ( https://nmap.org ) at 2026-09-03 10:13 +0200
Nmap scan report for 10.113.161.192
Host is up (0.038s latency).

PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.13 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   3072 55:ad:c1:15:65:9e:7e:a4:d8:6c:75:a5:d6:72:c4:11 (RSA)
|   256 11:b9:87:8b:72:3a:bf:47:dc:1d:33:d7:d0:53:f5:fb (ECDSA)
|_  256 2c:e0:34:0f:1e:1a:06:c5:53:45:2a:5d:de:96:4a:60 (ED25519)
3306/tcp open  mysql   MySQL 5.7.40
| mysql-info:
|   Protocol: 10
|   Version: 5.7.40
|   Thread ID: 4
|   Capabilities flags: 65535
|   Some Capabilities: FoundRows, LongColumnFlag, Support41Auth, InteractiveClient, ConnectWithDatabase, Speaks41ProtocolOld, SupportsLoadDataLocal, DontAllowDatabaseTableColumn, IgnoreSigpipes, LongPassword, SwitchToSSLAfterHandshake, Speaks41ProtocolNew, IgnoreSpaceBeforeParenthesis, SupportsTransactions, ODBCClient, SupportsCompression, SupportsMultipleStatments, SupportsMultipleResults, SupportsAuthPlugins
|   Status: Autocommit
|   Salt: r\x0C\x17\x1B]}\x10"hwmzw\x03bHG\x01aM
|_  Auth Plugin Name: mysql_native_password
|_ssl-date: TLS randomness does not represent time
| ssl-cert: Subject: commonName=MySQL_Server_5.7.40_Auto_Generated_Server_Certificate
| Not valid before: 2022-12-22T10:04:49
|_Not valid after:  2032-12-19T10:04:49
5000/tcp open  http    Docker Registry (API: 2.0)
|_http-title: Site doesn't have a title.
8080/tcp open  http    Node.js (Express middleware)
|_http-title: Login
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 24.76 seconds
[*] Done
╭─ ~/hacking/ctf/thm/medium/umbrella/recon                                                                   ✔ │ 43s ─╮
╰─                                                                                                                   ─╯

```


``` bash

❯ gobuster dir -u "http://10.113.161.192:5000/" -w /usr/share/wordlists/seclists/Discovery/Web-Content/DirBuster-2007_directory-list-2.3-medium.txt -t 100
===============================================================
Gobuster v3.8.2
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://10.113.161.192:5000/
[+] Method:                  GET
[+] Threads:                 100
[+] Wordlist:                /usr/share/wordlists/seclists/Discovery/Web-Content/DirBuster-2007_directory-list-2.3-medium.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.8.2
[+] Timeout:                 10s
===============================================================
Starting gobuster in directory enumeration mode
===============================================================
# license, visit http://creativecommons.org/licenses/by-sa/3.0/ (Status: 301) [Size: 0] [--> /%23%20license,%20visit%20http:/creativecommons.org/licenses/by-sa/3.0/]
v2                   (Status: 301) [Size: 39] [--> /v2/]
Progress: 31900 / 220558 (14.46%)

```
