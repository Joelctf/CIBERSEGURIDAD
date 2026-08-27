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


``` bash

❯ mkdir -p sysvol
❯ smbclient //10.129.83.15/SYSVOL -U 'alex.turner' -c 'recurse ON; prompt OFF; lcd sysvol; mget *'
Password for [WORKGROUP\alex.turner]:
NT_STATUS_ACCESS_DENIED listing \checkpoint.htb\DfsrPrivate\*
getting file \checkpoint.htb\Policies\{31B2F340-016D-11D2-945F-00C04FB984F9}\GPT.INI of size 22 as checkpoint.htb/Policies/{31B2F340-016D-11D2-945F-00C04FB984F9}/GPT.INI (0.1 KiloBytes/sec) (average 0.1 KiloBytes/sec)
getting file \checkpoint.htb\Policies\{6AC1786C-016F-11D2-945F-00C04fB984F9}\GPT.INI of size 22 as checkpoint.htb/Policies/{6AC1786C-016F-11D2-945F-00C04fB984F9}/GPT.INI (0.1 KiloBytes/sec) (average 0.1 KiloBytes/sec)
getting file \checkpoint.htb\Policies\{31B2F340-016D-11D2-945F-00C04FB984F9}\MACHINE\Registry.pol of size 2796 as checkpoint.htb/Policies/{31B2F340-016D-11D2-945F-00C04FB984F9}/MACHINE/Registry.pol (17.7 KiloBytes/sec) (average 6.1 KiloBytes/sec)
getting file \checkpoint.htb\Policies\{31B2F340-016D-11D2-945F-00C04FB984F9}\MACHINE\Microsoft\Windows NT\SecEdit\GptTmpl.inf of size 1098 as checkpoint.htb/Policies/{31B2F340-016D-11D2-945F-00C04FB984F9}/MACHINE/Microsoft/Windows NT/SecEdit/GptTmpl.inf (7.3 KiloBytes/sec) (average 6.4 KiloBytes/sec)
getting file \checkpoint.htb\Policies\{6AC1786C-016F-11D2-945F-00C04fB984F9}\MACHINE\Microsoft\Windows NT\SecEdit\GptTmpl.inf of size 4212 as checkpoint.htb/Policies/{6AC1786C-016F-11D2-945F-00C04fB984F9}/MACHINE/Microsoft/Windows NT/SecEdit/GptTmpl.inf (27.1 KiloBytes/sec) (average 10.6 KiloBytes/sec)
❯ cd sysvol
❯ ls
checkpoint.htb
╭─ ~/hacking/ctf/htb/medium/checkpoint/recon/sysvol                                                                ✔ ─╮
╰─                                                                                                                   ─╯

```
``` bash

❯ tree
.
├── {31B2F340-016D-11D2-945F-00C04FB984F9}
│   ├── GPT.INI
│   ├── MACHINE
│   │   ├── Microsoft
│   │   │   └── Windows NT
│   │   │       └── SecEdit
│   │   │           └── GptTmpl.inf
│   │   └── Registry.pol
│   └── USER
└── {6AC1786C-016F-11D2-945F-00C04fB984F9}
    ├── GPT.INI
    ├── MACHINE
    │   └── Microsoft
    │       └── Windows NT
    │           └── SecEdit
    │               └── GptTmpl.inf
    └── USER

13 directories, 5 files
╭─ ~/hacking/ctf/htb/medium/checkpoint/recon/sysvol/checkpoint.htb/Policies                                        ✔ ─╮
╰─                                                                                                                   ─╯

```


``` bash

❯ rpcclient -U 'checkpoint.htb/alex.turner%Checkpoint2024!' 10.129.83.15
rpcclient $> enumdomusers
user:[Administrator] rid:[0x1f4]
user:[Guest] rid:[0x1f5]
user:[krbtgt] rid:[0x1f6]
user:[alex.turner] rid:[0x44d]
user:[ryan.brooks] rid:[0x44f]
user:[svc_deploy] rid:[0x450]
user:[james.harper] rid:[0x457]
user:[sarah.mitchell] rid:[0x458]
user:[emily.carter] rid:[0x459]
user:[david.reynolds] rid:[0x45a]
user:[jessica.coleman] rid:[0x45b]
user:[lauren.flores] rid:[0x45c]
user:[michael.torres] rid:[0x45d]
user:[kevin.patterson] rid:[0x45e]
user:[brian.jenkins] rid:[0x45f]
user:[megan.perry] rid:[0x460]
user:[max.palmer] rid:[0x13ed]
rpcclient $> enumdomgroups
group:[Enterprise Read-only Domain Controllers] rid:[0x1f2]
group:[Domain Admins] rid:[0x200]
group:[Domain Users] rid:[0x201]
group:[Domain Guests] rid:[0x202]
group:[Domain Computers] rid:[0x203]
group:[Domain Controllers] rid:[0x204]
group:[Schema Admins] rid:[0x206]
group:[Enterprise Admins] rid:[0x207]
group:[Group Policy Creator Owners] rid:[0x208]
group:[Read-only Domain Controllers] rid:[0x209]
group:[Cloneable Domain Controllers] rid:[0x20a]
group:[Protected Users] rid:[0x20d]
group:[Key Admins] rid:[0x20e]
group:[Enterprise Key Admins] rid:[0x20f]
group:[Forest Trust Accounts] rid:[0x210]
group:[External Trust Accounts] rid:[0x211]
group:[IT-Staff] rid:[0x451]
group:[Finance-Staff] rid:[0x452]
group:[HR-Staff] rid:[0x453]
group:[Engineering-Staff] rid:[0x454]
group:[VPN-Users] rid:[0x455]
group:[DevTeam] rid:[0x456]
group:[DnsUpdateProxy] rid:[0x462]
group:[BackupAccess] rid:[0x463]
rpcclient $>

```

``` bash

❯ bloodyAD --host 10.129.83.15 --dns 10.129.83.15 -d checkpoint.htb -u alex.turner -p 'Checkpoint2024!' get writable

distinguishedName: CN=Deleted Objects,DC=checkpoint,DC=htb
DACL: WRITE

distinguishedName: CN=S-1-5-11,CN=ForeignSecurityPrincipals,DC=checkpoint,DC=htb
permission: WRITE

distinguishedName: OU=Employees,DC=checkpoint,DC=htb
permission: CREATE_CHILD

distinguishedName: CN=Alex Turner,OU=Employees,DC=checkpoint,DC=htb
permission: WRITE

distinguishedName: CN=Mark Davies\0ADEL:2217e877-e2a2-47d7-91d4-99ede36f367e,CN=Deleted Objects,DC=checkpoint,DC=htb
permission: WRITE

distinguishedName: DC=checkpoint.htb,CN=MicrosoftDNS,DC=DomainDnsZones,DC=checkpoint,DC=htb
permission: CREATE_CHILD

distinguishedName: DC=_msdcs.checkpoint.htb,CN=MicrosoftDNS,DC=ForestDnsZones,DC=checkpoint,DC=htb
permission: CREATE_CHILD
╭─ ~/hacking/ctf/htb/medium/checkpoint/recon/sysvol/checkpoint.htb/Policies                                   ✔ │ 3s ─╮
╰─                                                                                                                   ─╯

```
