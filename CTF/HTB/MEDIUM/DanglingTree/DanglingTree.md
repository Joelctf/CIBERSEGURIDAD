``` bash

❯ recon 10.129.80.127
Starting Nmap 7.98 ( https://nmap.org ) at 2026-08-25 01:25 +0200
Nmap scan report for 10.129.80.127
Host is up (0.036s latency).
Not shown: 65510 filtered tcp ports (no-response)
PORT      STATE SERVICE
53/tcp    open  domain
80/tcp    open  http
88/tcp    open  kerberos-sec
135/tcp   open  msrpc
139/tcp   open  netbios-ssn
389/tcp   open  ldap
443/tcp   open  https
445/tcp   open  microsoft-ds
464/tcp   open  kpasswd5
593/tcp   open  http-rpc-epmap
636/tcp   open  ldapssl
3268/tcp  open  globalcatLDAP
3269/tcp  open  globalcatLDAPssl
3389/tcp  open  ms-wbt-server
6600/tcp  open  mshvlm
9389/tcp  open  adws
49664/tcp open  unknown
49677/tcp open  unknown
49679/tcp open  unknown
49681/tcp open  unknown
49682/tcp open  unknown
49693/tcp open  unknown
49708/tcp open  unknown
49722/tcp open  unknown
49771/tcp open  unknown

Nmap done: 1 IP address (1 host up) scanned in 26.92 seconds
[*] First script done
[*] Open ports = '53,80,88,135,139,389,443,445,464,593,636,3268,3269,3389,6600,9389,49664,49677,49679,49681,49682,49693,49708,49722,49771'
Starting Nmap 7.98 ( https://nmap.org ) at 2026-08-25 01:26 +0200
Nmap scan report for 10.129.80.127
Host is up (0.038s latency).

PORT      STATE SERVICE       VERSION
53/tcp    open  domain        Simple DNS Plus
80/tcp    open  http          Microsoft IIS httpd 10.0
|_http-server-header: Microsoft-IIS/10.0
|_http-title: IIS Windows Server
| http-methods:
|_  Potentially risky methods: TRACE
88/tcp    open  kerberos-sec  Microsoft Windows Kerberos (server time: 2026-08-25 06:25:31Z)
135/tcp   open  msrpc         Microsoft Windows RPC
139/tcp   open  netbios-ssn   Microsoft Windows netbios-ssn
389/tcp   open  ldap          Microsoft Windows Active Directory LDAP (Domain: danglingtree.htb, Site: Default-First-Site-Name)
|_ssl-date: TLS randomness does not represent time
| ssl-cert: Subject:
| Subject Alternative Name: DNS:dc.danglingtree.htb, DNS:danglingtree.htb, DNS:DANGLINGTREE
| Not valid before: 2026-08-03T16:32:53
|_Not valid after:  2106-08-03T16:32:53
443/tcp   open  ssl/https?
| tls-alpn:
|   h2
|_  http/1.1
|_ssl-date: TLS randomness does not represent time
| ssl-cert: Subject: commonName=danglingtree-DC-CA
| Not valid before: 2026-03-26T05:34:19
|_Not valid after:  2114-03-26T05:44:18
445/tcp   open  microsoft-ds?
464/tcp   open  kpasswd5?
593/tcp   open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
636/tcp   open  ssl/ldap
| ssl-cert: Subject:
| Subject Alternative Name: DNS:dc.danglingtree.htb, DNS:danglingtree.htb, DNS:DANGLINGTREE
| Not valid before: 2026-08-03T16:32:53
|_Not valid after:  2106-08-03T16:32:53
|_ssl-date: TLS randomness does not represent time
3268/tcp  open  ldap          Microsoft Windows Active Directory LDAP (Domain: danglingtree.htb, Site: Default-First-Site-Name)
|_ssl-date: TLS randomness does not represent time
| ssl-cert: Subject:
| Subject Alternative Name: DNS:dc.danglingtree.htb, DNS:danglingtree.htb, DNS:DANGLINGTREE
| Not valid before: 2026-08-03T16:32:53
|_Not valid after:  2106-08-03T16:32:53
3269/tcp  open  ssl/ldap      Microsoft Windows Active Directory LDAP (Domain: danglingtree.htb, Site: Default-First-Site-Name)
|_ssl-date: TLS randomness does not represent time
| ssl-cert: Subject:
| Subject Alternative Name: DNS:dc.danglingtree.htb, DNS:danglingtree.htb, DNS:DANGLINGTREE
| Not valid before: 2026-08-03T16:32:53
|_Not valid after:  2106-08-03T16:32:53
3389/tcp  open  ms-wbt-server
|_ssl-date: TLS randomness does not represent time
| rdp-ntlm-info:
|   Target_Name: DANGLINGTREE
|   NetBIOS_Domain_Name: DANGLINGTREE
|   NetBIOS_Computer_Name: DC
|   DNS_Domain_Name: danglingtree.htb
|   DNS_Computer_Name: dc.danglingtree.htb
|   DNS_Tree_Name: danglingtree.htb
|   Product_Version: 10.0.26100
|_  System_Time: 2026-08-25T06:27:11+00:00
| ssl-cert: Subject: commonName=dc.danglingtree.htb
| Not valid before: 2026-08-24T06:18:49
|_Not valid after:  2027-02-23T06:18:49
6600/tcp  open  ssl/mshvlm?
| tls-alpn:
|   h2
|_  http/1.1
|_ssl-date: TLS randomness does not represent time
| ssl-cert: Subject: commonName=dc.danglingtree.htb
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1:<unsupported>, DNS:dc.danglingtree.htb
| Not valid before: 2026-03-26T05:41:20
|_Not valid after:  2027-03-26T05:41:20
| fingerprint-strings:
|   GetRequest:
|     HTTP/1.1 403 Forbidden
|     Connection: close
|     Date: Tue, 25 Aug 2026 06:25:48 GMT
|     Cache-Control: no-store
|     Cache-Control: max-age=0
|     Pragma: no-cache
|     Set-Cookie: .AspNetCore.Antiforgery.7Eyhia2WOxE=CfDJ8HsozULo80ZBsxvkNAKguomH23UVdad5h8GKNqVw5xY4mztJZwpMkqspxQYBAdl1CrTGsvEHVFW0rLdG7467wFytC0Mz61LDlepGp-QlAGPs3ayA_FZTgfOSB-QwpR1A_SP0P8vuIPg-o7MXbqQ37O8; path=/; secure; samesite=none; Partitioned
|     Set-Cookie: WAC-SESSION=1b9ed195453c4c5f81b6640ba0d1e96d; expires=Wed, 26 Aug 2026 06:25:48 GMT; path=/; secure; samesite=lax; httponly
|     Set-Cookie: WAC-TOKEN=; expires=Thu, 01 Jan 1970 00:00:00 GMT; path=/
|     Set-Cookie: WAC-AAD=; expires=Thu, 01 Jan 1970 00:00:00 GMT; path=/
|     Set-Cookie: XSRF-TOKEN=; expires=Thu, 01 Jan 1970 00:00:00 GMT; path=/
|     Strict-Transport-Security: max-age=5184000; includeSubDomains; preload
|     <!DOCTYPE html>
|     <html lang="en" xmlns="http://www.w3.org/1999/xhtml">
|     <head
|   HTTPOptions:
|     HTTP/1.1 403 Forbidden
|     Connection: close
|     Date: Tue, 25 Aug 2026 06:25:48 GMT
|     Cache-Control: no-store
|     Cache-Control: max-age=0
|     Pragma: no-cache
|     Set-Cookie: .AspNetCore.Antiforgery.7Eyhia2WOxE=CfDJ8HsozULo80ZBsxvkNAKguonsq_NqZnGV0-vJLxmH0J5BzIF_5CiMqccl9Dhp8pb8VJZ1XRvGf9mRguiyjgJswULUKMTSkUOWW0ouCv95aGu7VkLdcjl0lgaAGm2gZzn5QF1YYcWEZzp5JOClTlj5i30; path=/; secure; samesite=none; Partitioned
|     Set-Cookie: WAC-SESSION=95f7c0c0f6e34cbc870b2f17a0d9cfa0; expires=Wed, 26 Aug 2026 06:25:48 GMT; path=/; secure; samesite=lax; httponly
|     Set-Cookie: WAC-TOKEN=; expires=Thu, 01 Jan 1970 00:00:00 GMT; path=/
|     Set-Cookie: WAC-AAD=; expires=Thu, 01 Jan 1970 00:00:00 GMT; path=/
|     Set-Cookie: XSRF-TOKEN=; expires=Thu, 01 Jan 1970 00:00:00 GMT; path=/
|     Strict-Transport-Security: max-age=5184000; includeSubDomains; preload
|     <!DOCTYPE html>
|     <html lang="en" xmlns="http://www.w3.org/1999/xhtml">
|_    <head
9389/tcp  open  mc-nmf        .NET Message Framing
49664/tcp open  msrpc         Microsoft Windows RPC
49677/tcp open  msrpc         Microsoft Windows RPC
49679/tcp open  msrpc         Microsoft Windows RPC
49681/tcp open  msrpc         Microsoft Windows RPC
49682/tcp open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
49693/tcp open  msrpc         Microsoft Windows RPC
49708/tcp open  msrpc         Microsoft Windows RPC
49722/tcp open  msrpc         Microsoft Windows RPC
49771/tcp open  msrpc         Microsoft Windows RPC
2 services unrecognized despite returning data. If you know the service/version, please submit the following fingerprints at https://nmap.org/cgi-bin/submit.cgi?new-service :
==============NEXT SERVICE FINGERPRINT (SUBMIT INDIVIDUALLY)==============
SF-Port3389-TCP:V=7.98%I=7%D=8/25%Time=6A8CD322%P=x86_64-pc-linux-gnu%r(Te
SF:rminalServerCookie,13,"\x03\0\0\x13\x0e\xd0\0\0\x124\0\x02\?\x08\0\x02\
SF:0\0\0");
==============NEXT SERVICE FINGERPRINT (SUBMIT INDIVIDUALLY)==============
SF-Port6600-TCP:V=7.98%T=SSL%I=7%D=8/25%Time=6A8CD32E%P=x86_64-pc-linux-gn
SF:u%r(GetRequest,1000,"HTTP/1\.1\x20403\x20Forbidden\r\nConnection:\x20cl
SF:ose\r\nDate:\x20Tue,\x2025\x20Aug\x202026\x2006:25:48\x20GMT\r\nCache-C
SF:ontrol:\x20no-store\r\nCache-Control:\x20max-age=0\r\nPragma:\x20no-cac
SF:he\r\nSet-Cookie:\x20\.AspNetCore\.Antiforgery\.7Eyhia2WOxE=CfDJ8HsozUL
SF:o80ZBsxvkNAKguomH23UVdad5h8GKNqVw5xY4mztJZwpMkqspxQYBAdl1CrTGsvEHVFW0rL
SF:dG7467wFytC0Mz61LDlepGp-QlAGPs3ayA_FZTgfOSB-QwpR1A_SP0P8vuIPg-o7MXbqQ37
SF:O8;\x20path=/;\x20secure;\x20samesite=none;\x20Partitioned\r\nSet-Cooki
SF:e:\x20WAC-SESSION=1b9ed195453c4c5f81b6640ba0d1e96d;\x20expires=Wed,\x20
SF:26\x20Aug\x202026\x2006:25:48\x20GMT;\x20path=/;\x20secure;\x20samesite
SF:=lax;\x20httponly\r\nSet-Cookie:\x20WAC-TOKEN=;\x20expires=Thu,\x2001\x
SF:20Jan\x201970\x2000:00:00\x20GMT;\x20path=/\r\nSet-Cookie:\x20WAC-AAD=;
SF:\x20expires=Thu,\x2001\x20Jan\x201970\x2000:00:00\x20GMT;\x20path=/\r\n
SF:Set-Cookie:\x20XSRF-TOKEN=;\x20expires=Thu,\x2001\x20Jan\x201970\x2000:
SF:00:00\x20GMT;\x20path=/\r\nStrict-Transport-Security:\x20max-age=518400
SF:0;\x20includeSubDomains;\x20preload\r\n\r\n<!DOCTYPE\x20html>\r\n<html\
SF:x20lang=\"en\"\x20xmlns=\"http://www\.w3\.org/1999/xhtml\">\r\n\r\n<hea
SF:d")%r(HTTPOptions,2000,"HTTP/1\.1\x20403\x20Forbidden\r\nConnection:\x2
SF:0close\r\nDate:\x20Tue,\x2025\x20Aug\x202026\x2006:25:48\x20GMT\r\nCach
SF:e-Control:\x20no-store\r\nCache-Control:\x20max-age=0\r\nPragma:\x20no-
SF:cache\r\nSet-Cookie:\x20\.AspNetCore\.Antiforgery\.7Eyhia2WOxE=CfDJ8Hso
SF:zULo80ZBsxvkNAKguonsq_NqZnGV0-vJLxmH0J5BzIF_5CiMqccl9Dhp8pb8VJZ1XRvGf9m
SF:RguiyjgJswULUKMTSkUOWW0ouCv95aGu7VkLdcjl0lgaAGm2gZzn5QF1YYcWEZzp5JOClTl
SF:j5i30;\x20path=/;\x20secure;\x20samesite=none;\x20Partitioned\r\nSet-Co
SF:okie:\x20WAC-SESSION=95f7c0c0f6e34cbc870b2f17a0d9cfa0;\x20expires=Wed,\
SF:x2026\x20Aug\x202026\x2006:25:48\x20GMT;\x20path=/;\x20secure;\x20sames
SF:ite=lax;\x20httponly\r\nSet-Cookie:\x20WAC-TOKEN=;\x20expires=Thu,\x200
SF:1\x20Jan\x201970\x2000:00:00\x20GMT;\x20path=/\r\nSet-Cookie:\x20WAC-AA
SF:D=;\x20expires=Thu,\x2001\x20Jan\x201970\x2000:00:00\x20GMT;\x20path=/\
SF:r\nSet-Cookie:\x20XSRF-TOKEN=;\x20expires=Thu,\x2001\x20Jan\x201970\x20
SF:00:00:00\x20GMT;\x20path=/\r\nStrict-Transport-Security:\x20max-age=518
SF:4000;\x20includeSubDomains;\x20preload\r\n\r\n<!DOCTYPE\x20html>\r\n<ht
SF:ml\x20lang=\"en\"\x20xmlns=\"http://www\.w3\.org/1999/xhtml\">\r\n\r\n<
SF:head");
Service Info: Host: DC; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
|_clock-skew: mean: 6h59m08s, deviation: 0s, median: 6h59m08s
| smb2-security-mode:
|   3.1.1:
|_    Message signing enabled and required
| smb2-time:
|   date: 2026-08-25T06:27:15
|_  start_date: N/A

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 149.09 seconds
[*] Done
╭─ ~/hacking/ctf/htb/medium/danglintree/recon                                                              ✔ │ 3m 2s ─╮
╰─                                                                                                                   ─╯

```
``` bash

❯ echo "10.129.80.127 danglingtree.htb" | sudo tee -a /etc/hosts
[sudo] password for joel:
10.129.80.127 danglingtree.htb
╭─ ~/hacking/ctf/htb/medium/danglintree/recon                                                                 ✔ │ 3s ─╮
╰─                                                                                                                   ─╯

```

``` bash

❯ curl -I 10.129.80.127
HTTP/1.1 200 OK
Content-Length: 703
Content-Type: text/html
Last-Modified: Thu, 26 Mar 2026 05:40:48 GMT
Accept-Ranges: bytes
ETag: "b3b97519e3bcdc1:0"
Server: Microsoft-IIS/10.0
X-Powered-By: ASP.NET
Date: Tue, 25 Aug 2026 06:38:04 GMT

❯ curl -I https://10.129.80.127 -k
HTTP/2 200
content-length: 703
content-type: text/html
last-modified: Thu, 26 Mar 2026 05:40:48 GMT
accept-ranges: bytes
etag: "b3b97519e3bcdc1:0"
server: Microsoft-IIS/10.0
x-powered-by: ASP.NET
date: Tue, 25 Aug 2026 06:38:07 GMT

❯ curl https://10.129.80.127 -k
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Strict//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd">
<html xmlns="http://www.w3.org/1999/xhtml">
<head>
<meta http-equiv="Content-Type" content="text/html; charset=iso-8859-1" />
<title>IIS Windows Server</title>
<style type="text/css">
<!--
body {
        color:#000000;
        background-color:#0072C6;
        margin:0;
}

#container {
        margin-left:auto;
        margin-right:auto;
        text-align:center;
        }

a img {
        border:none;
}

-->
</style>
</head>
<body>
<div id="container">
<a href="http://go.microsoft.com/fwlink/?linkid=66138&amp;clcid=0x409"><img src="iisstart.png" alt="IIS" width="960" height="600" /></a>
</div>
</body>
</html>%                                                                                                                ╭─ ~/hacking/ctf/htb/medium/danglintree/recon                                                                      ✔ ─╮
╰─                                                                                                                   ─╯

```

``` bash

❯ smbclient -L //danglingtree.htb -N

        Sharename       Type      Comment
        ---------       ----      -------
        ADMIN$          Disk      Remote Admin
        C$              Disk      Default share
        IPC$            IPC       Remote IPC
        IT              Disk
        NETLOGON        Disk      Logon server share
        SYSVOL          Disk      Logon server share
Reconnecting with SMB1 for workgroup listing.
do_connect: Connection to danglingtree.htb failed (Error NT_STATUS_RESOURCE_NAME_NOT_FOUND)
Unable to connect with SMB1 -- no workgroup available
╭─ ~/hacking/ctf/htb/medium/danglintree/recon                                                                      ✔ ─╮
╰─                                                                                                                   ─╯

```

``` bash

❯ smbclient //danglingtree.htb/NETLOGON -N
do_connect: Connection to dc.danglingtree.htb failed (Error NT_STATUS_UNSUCCESSFUL)
❯ smbclient //danglingtree.htb/SYSVOL -N
do_connect: Connection to dc.danglingtree.htb failed (Error NT_STATUS_UNSUCCESSFUL)
❯ smbclient //danglingtree.htb/IPC$ -N
Try "help" to get a list of possible commands.
smb: \> dir
NT_STATUS_NO_SUCH_FILE listing \*
smb: \> exit
❯ smbclient //danglingtree.htb/C$ -N
tree connect failed: NT_STATUS_ACCESS_DENIED
❯ smbclient //danglingtree.htb/ADMIN$ -N
tree connect failed: NT_STATUS_ACCESS_DENIED
❯ smbclient //danglingtree.htb/IT -N
Try "help" to get a list of possible commands.
smb: \> dir
  .                                   D        0  Sun Apr  5 03:05:09 2026
  ..                                  D        0  Sun Apr  5 02:57:30 2026
  Security                            D        0  Sun Apr  5 03:05:20 2026

                7062015 blocks of size 4096. 2261723 blocks available

smb: \> cd Security\
smb: \Security\> dir
  .                                   D        0  Sun Apr  5 03:05:20 2026
  ..                                  D        0  Sun Apr  5 03:05:09 2026
  DanglingTree_RoE_Assessment.pdf      A    28905  Sat Apr  4 17:50:23 2026

                7062015 blocks of size 4096. 2261707 blocks available
smb: \Security\> get DanglingTree_RoE_Assessment.pdf
getting file \Security\DanglingTree_RoE_Assessment.pdf of size 28905 as DanglingTree_RoE_Assessment.pdf (146.3 KiloBytes/sec) (average 146.3 KiloBytes/sec)
smb: \Security\> exit
❯ ls
all_ports.txt  DanglingTree_RoE_Assessment.pdf  version_ports.txt
╭─ ~/hacking/ctf/htb/medium/danglintree/recon                                                                      ✔ ─╮
╰─                                                                                                                   ─╯

```

[PDF](./DanglingTree_RoE_Assessment.pdf)

``` bash

❯ sudo ntpdate 10.129.80.127
2026-08-25 09:16:29.378752 (+0200) +25149.596887 +/- 0.019536 10.129.80.127 s1 no-leap
CLOCK: time stepped by 25149.596887
╭─ ~/hacking/ctf/htb/medium/danglintree/recon                                                         ✔ │ 6h 59m 10s ─╮
╰─                                                                                                                   ─╯

```

``` bash

❯ rpcclient -U 'danglingtree.htb/anderson.w%R3dT3am@Acc3ss#01' 10.129.80.127
rpcclient $> enumdomusers
user:[anderson.w] rid:[0xa29]
rpcclient $> ^C
❯ nxc ldap 10.129.80.127 -d danglingtree.htb -u 'anderson.w' -p 'R3dT3am@Acc3ss#01' --users
LDAP        10.129.80.127   389    DC               [*] Windows 11 / Server 2025 Build 26100 (name:DC) (domain:danglingtree.htb) (signing:Enforced) (channel binding:Never)
LDAP        10.129.80.127   389    DC               [+] danglingtree.htb\anderson.w:R3dT3am@Acc3ss#01
LDAP        10.129.80.127   389    DC               [*] Enumerated 1 domain users: danglingtree.htb
LDAP        10.129.80.127   389    DC               -Username-                    -Last PW Set-       -BadPW-  -Description-
LDAP        10.129.80.127   389    DC               anderson.w                    2026-04-05 02:00:40 0
❯ bloodhound-python -u 'anderson.w' -p 'R3dT3am@Acc3ss#01' -d danglingtree.htb -ns 10.129.80.127 -c All
INFO: BloodHound.py for BloodHound LEGACY (BloodHound 4.2 and 4.3)
INFO: Found AD domain: danglingtree.htb
INFO: Getting TGT for user
INFO: Connecting to LDAP server: dc.danglingtree.htb
INFO: Testing resolved hostname connectivity dead:beef::dba:45e7:cd70:5a73
INFO: Trying LDAP connection to dead:beef::dba:45e7:cd70:5a73
WARNING: LDAP Authentication is refused because LDAP signing is enabled. Trying to connect over LDAPS instead...
INFO: Found 1 domains
INFO: Found 1 domains in the forest
INFO: Found 1 computers
INFO: Connecting to LDAP server: dc.danglingtree.htb
INFO: Testing resolved hostname connectivity dead:beef::dba:45e7:cd70:5a73
INFO: Trying LDAP connection to dead:beef::dba:45e7:cd70:5a73
WARNING: LDAP Authentication is refused because LDAP signing is enabled. Trying to connect over LDAPS instead...
INFO: Found 2 users
INFO: Found 33 groups
INFO: Found 2 gpos
INFO: Found 3 ous
INFO: Found 18 containers
INFO: Found 0 trusts
INFO: Starting computer enumeration with 10 workers
INFO: Querying computer: dc.danglingtree.htb
INFO: Done in 00M 10S
╭─ ~/hacking/ctf/htb/medium/danglintree/recon                                                                ✔ │ 11s ─╮
╰─

```

``` bash

❯ ls
20260825020930_computers.json   20260825021042_computers.json   20260825021242_computers.json
20260825020930_containers.json  20260825021042_containers.json  20260825021242_containers.json
20260825020930_domains.json     20260825021042_domains.json     20260825021242_domains.json
20260825020930_gpos.json        20260825021042_gpos.json        20260825021242_gpos.json
20260825020930_groups.json      20260825021042_groups.json      20260825021242_groups.json
20260825020930_ous.json         20260825021042_ous.json         20260825021242_ous.json
20260825020930_users.json       20260825021042_users.json       20260825021242_users.json
❯ jq -r '.data[].Properties.name' *_users.json
NT AUTHORITY@DANGLINGTREE.HTB
ANDERSON.W@DANGLINGTREE.HTB
NT AUTHORITY@DANGLINGTREE.HTB
ANDERSON.W@DANGLINGTREE.HTB
NT AUTHORITY@DANGLINGTREE.HTB
ANDERSON.W@DANGLINGTREE.HTB
╭─ ~/hacking/ctf/htb/medium/danglintree/recon/bloodhound_enum                                                      ✔ ─╮
╰─                                                                                                                   ─╯

```

``` bash

❯ curl -I https://danglingtree.htb:6600 -k
HTTP/2 403
date: Tue, 25 Aug 2026 07:29:29 GMT
cache-control: no-store
cache-control: max-age=0
pragma: no-cache
set-cookie: .AspNetCore.Antiforgery.7Eyhia2WOxE=CfDJ8HsozULo80ZBsxvkNAKguonuTI1Gb3oAWCu34qldFXf70BdrsDE_q6YFdRAWtQMOK-Ntb3vrHqUZD7_vpmEmfOMt0GErF4fU3nsK1ms5-BMzQp9hav9OqYnQojlirRtK-FgxwiZrOweeW_cyhfEJV_U; path=/; secure; samesite=none; Partitioned
set-cookie: WAC-SESSION=1c5821565b364ae884b3b2190c3b639d; expires=Wed, 26 Aug 2026 07:29:30 GMT; path=/; secure; samesite=lax; httponly
set-cookie: WAC-TOKEN=; expires=Thu, 01 Jan 1970 00:00:00 GMT; path=/
set-cookie: WAC-AAD=; expires=Thu, 01 Jan 1970 00:00:00 GMT; path=/
set-cookie: XSRF-TOKEN=; expires=Thu, 01 Jan 1970 00:00:00 GMT; path=/
strict-transport-security: max-age=5184000; includeSubDomains; preload

╭─ ~/hacking/ctf/htb/medium/danglintree/recon/bloodhound_enum                                                      ✔ ─╮
╰─                                                                                                                   ─╯

```


