``` bash


❯ recon 10.129.83.15
Starting Nmap 7.98 ( https://nmap.org ) at 2026-08-27 19:54 +0200
Nmap scan report for 10.129.83.15
Host is up (0.037s latency).
Not shown: 65514 filtered tcp ports (no-response)
PORT      STATE SERVICE
53/tcp    open  domain
88/tcp    open  kerberos-sec
135/tcp   open  msrpc
139/tcp   open  netbios-ssn
389/tcp   open  ldap
445/tcp   open  microsoft-ds
464/tcp   open  kpasswd5
593/tcp   open  http-rpc-epmap
636/tcp   open  ldapssl
3268/tcp  open  globalcatLDAP
3269/tcp  open  globalcatLDAPssl
5985/tcp  open  wsman
9389/tcp  open  adws
49664/tcp open  unknown
49668/tcp open  unknown
49669/tcp open  unknown
49679/tcp open  unknown
49680/tcp open  unknown
49688/tcp open  unknown
49709/tcp open  unknown
49717/tcp open  unknown

Nmap done: 1 IP address (1 host up) scanned in 26.95 seconds
[*] First script done
[*] Open ports = '53,88,135,139,389,445,464,593,636,3268,3269,5985,9389,49664,49668,49669,49679,49680,49688,49709,49717'
Starting Nmap 7.98 ( https://nmap.org ) at 2026-08-27 19:55 +0200
Nmap scan report for 10.129.83.15
Host is up (0.040s latency).

PORT      STATE SERVICE           VERSION
53/tcp    open  domain            Simple DNS Plus
88/tcp    open  kerberos-sec      Microsoft Windows Kerberos (server time: 2026-08-28 00:54:16Z)
135/tcp   open  msrpc             Microsoft Windows RPC
139/tcp   open  netbios-ssn       Microsoft Windows netbios-ssn
389/tcp   open  ldap              Microsoft Windows Active Directory LDAP (Domain: checkpoint.htb, Site: Default-First-Site-Name)
445/tcp   open  microsoft-ds?
464/tcp   open  kpasswd5?
593/tcp   open  ncacn_http        Microsoft Windows RPC over HTTP 1.0
636/tcp   open  ldapssl?
3268/tcp  open  ldap              Microsoft Windows Active Directory LDAP (Domain: checkpoint.htb, Site: Default-First-Site-Name)
3269/tcp  open  globalcatLDAPssl?
5985/tcp  open  http              Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
9389/tcp  open  mc-nmf            .NET Message Framing
49664/tcp open  msrpc             Microsoft Windows RPC
49668/tcp open  msrpc             Microsoft Windows RPC
49669/tcp open  msrpc             Microsoft Windows RPC
49679/tcp open  msrpc             Microsoft Windows RPC
49680/tcp open  ncacn_http        Microsoft Windows RPC over HTTP 1.0
49688/tcp open  msrpc             Microsoft Windows RPC
49709/tcp open  msrpc             Microsoft Windows RPC
49717/tcp open  msrpc             Microsoft Windows RPC
Service Info: Host: DC01; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| smb2-security-mode:
|   3.1.1:
|_    Message signing enabled and required
| smb2-time:
|   date: 2026-08-28T00:55:09
|_  start_date: N/A
|_clock-skew: 6h59m06s

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 96.99 seconds
[*] Done
╭─ ~/hacking/ctf/htb                                                                                      ✔ │ 2m 10s ─╮
╰─                                                                                                                   ─╯



```


![img](./img/Captura1.png)


```` bash

❯ netexec smb 10.129.83.15 -u 'alex.turner' -p 'Checkpoint2024!' --shares
SMB         10.129.83.15    445    DC01             [*] Windows 11 / Server 2025 Build 26100 x64 (name:DC01) (domain:checkpoint.htb) (signing:True) (SMBv1:None)
SMB         10.129.83.15    445    DC01             [+] checkpoint.htb\alex.turner:Checkpoint2024!
SMB         10.129.83.15    445    DC01             [*] Enumerated shares
SMB         10.129.83.15    445    DC01             Share           Permissions     Remark
SMB         10.129.83.15    445    DC01             -----           -----------     ------
SMB         10.129.83.15    445    DC01             ADMIN$                          Remote Admin
SMB         10.129.83.15    445    DC01             C$                              Default share
SMB         10.129.83.15    445    DC01             DevDrop         READ            VS Code extensions share for approved .vsix packages compatible with VS Code engine 1.118.0
SMB         10.129.83.15    445    DC01             IPC$            READ            Remote IPC
SMB         10.129.83.15    445    DC01             NETLOGON        READ            Logon server share
SMB         10.129.83.15    445    DC01             SYSVOL          READ            Logon server share
SMB         10.129.83.15    445    DC01             VMBackups
╭─ ~/hacking/ctf/htb                                                                                          ✔ │ 6s ─╮
╰─                                                                                                                   ─╯

```


``` bash

❯ smbclient '//10.129.83.15/DevDrop' -U 'alex.turner'
Password for [WORKGROUP\alex.turner]:
Try "help" to get a list of possible commands.
smb: \> dir
  .                                   D        0  Tue May 26 23:45:01 2026
  ..                                  D        0  Sat May  9 16:42:27 2026

                10459391 blocks of size 4096. 2465216 blocks available
smb: \> exit
❯ smbclient '//10.129.83.15/IPC$' -U 'alex.turner'
Password for [WORKGROUP\alex.turner]:
Try "help" to get a list of possible commands.
smb: \> dir
NT_STATUS_NO_SUCH_FILE listing \*
smb: \> exit
❯ smbclient '//10.129.83.15/NETLOGON' -U 'alex.turner'
Password for [WORKGROUP\alex.turner]:
Try "help" to get a list of possible commands.
smb: \> dir
  .                                   D        0  Sat May  9 10:39:17 2026
  ..                                  D        0  Sat May  9 10:42:23 2026

                10459391 blocks of size 4096. 2467797 blocks available
smb: \> exit
❯ smbclient '//10.129.83.15/SYSVOL' -U 'alex.turner'
Password for [WORKGROUP\alex.turner]:
Try "help" to get a list of possible commands.
smb: \> dir
  .                                   D        0  Sat May  9 10:39:17 2026
  ..                                  D        0  Sat May  9 10:39:17 2026
  checkpoint.htb                     Dr        0  Sat May  9 10:39:17 2026

                10459391 blocks of size 4096. 2467737 blocks available
smb: \> cd checkpoint.htb\
smb: \checkpoint.htb\> dir
  .                                   D        0  Sat May  9 10:42:23 2026
  ..                                  D        0  Sat May  9 10:39:17 2026
  DfsrPrivate                      DHSr        0  Sat May  9 10:42:23 2026
  Policies                            D        0  Sat May  9 10:39:34 2026
  scripts                             D        0  Sat May  9 10:39:17 2026

                10459391 blocks of size 4096. 2467737 blocks available
smb: \checkpoint.htb\>

```
