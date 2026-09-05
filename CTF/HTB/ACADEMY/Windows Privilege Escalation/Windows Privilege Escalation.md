
### Useful Tools

There are many tools available to us to assist with enumerating Windows systems for common and obscure privilege escalation vectors. Below is a list of useful binaries and scripts, many of which we will cover within the coming module sections

| Tool | Description |
| --- | --- |
| [Seatbelt](https://github.com/GhostPack/Seatbelt) | C# project for performing a wide variety of local privilege escalation checks |
| [winPEAS](https://github.com/carlospolop/privilege-escalation-awesome-scripts-suite/tree/master/winPEAS) | WinPEAS is a script that searches for possible paths to escalate privileges on Windows hosts. All of the checks are explained [here](https://book.hacktricks.wiki/en/windows-hardening/checklist-windows-privilege-escalation.html) |
| [PowerUp](https://raw.githubusercontent.com/PowerShellMafia/PowerSploit/master/Privesc/PowerUp.ps1) | PowerShell script for finding common Windows privilege escalation vectors that rely on misconfigurations. It can also be used to exploit some of the issues found |
| [SharpUp](https://github.com/GhostPack/SharpUp) | C# version of PowerUp |
| [JAWS](https://github.com/411Hall/JAWS) | PowerShell script for enumerating privilege escalation vectors written in PowerShell 2.0 |
| [SessionGopher](https://github.com/Arvanaghi/SessionGopher) | SessionGopher is a PowerShell tool that finds and decrypts saved session information for remote access tools. It extracts PuTTY, WinSCP, SuperPuTTY, FileZilla, and RDP saved session information |
| [Watson](https://github.com/rasta-mouse/Watson) | Watson is a .NET tool designed to enumerate missing KBs and suggest exploits for Privilege Escalation vulnerabilities |
| [LaZagne](https://github.com/AlessandroZ/LaZagne) | Tool used for retrieving passwords stored on a local machine from web browsers, chat tools, databases, Git, email, memory dumps, PHP, sysadmin tools, wireless network configurations, internal Windows password storage mechanisms, and more |
| [Windows Exploit Suggester - Next Generation](https://github.com/bitsadmin/wesng) | WES-NG is a tool based on the output of Windows' `systeminfo` utility which provides the list of vulnerabilities the OS is vulnerable to, including any exploits for these vulnerabilities. Every Windows OS between Windows XP and Windows 10, including their Windows Server counterparts, is supported |
| [Sysinternals Suite](https://docs.microsoft.com/en-us/sysinternals/downloads/sysinternals-suite) | We will use several tools from Sysinternals in our enumeration including [AccessChk](https://docs.microsoft.com/en-us/sysinternals/downloads/accesschk), [PipeList](https://docs.microsoft.com/en-us/sysinternals/downloads/pipelist), and [PsService](https://docs.microsoft.com/en-us/sysinternals/downloads/psservice) |



We can also find pre-compiled binaries of Seatbelt and SharpUp [here](https://github.com/r3motecontrol/Ghostpack-CompiledBinaries), and standalone binaries of LaZagne [here](https://github.com/AlessandroZ/LaZagne/releases/). It is recommended that we always compile our tools from the source if using them in a client environment

Cabe recalcar que, estas son algunas de las herramientas mas conocidas de automatización en la enumeracion de privesc, por lo cual ejecutarlo directo en un sistema alertaría al antivirus al momento. En este modulo no se toca los temas de AV. Por lo que obviaremos esta parte.


### Network Information

La enumeración es de red es de las partes mas importantes. Podemos descubrir que el host tiene doble conexión y que comprometerlo nos permitirá acceder lateralmente a otra parte de la red a la que antes no podíamos acceder.

##### Comandos utiles

Ver IP, interfazes de red, MAC, DNS, etc:

``` powershell

C:\htb> ipconfig /all

```

Ver información de la tabla ARP:

``` powershell

C:\htb> arp -a

```

Ver las rutas configuradas en el Host:

``` powershell

C:\htb> route print


```

### Enumerating Protections

La mayoría de los entornos modernos cuentan con algún tipo de antivirus o servicio de detección y respuesta de endpoints (EDR) en funcionamiento para monitorizar, alertar y bloquear proactivamente las amenazas. Estas herramientas pueden interferir con el proceso de enumeración. Es muy probable que presenten algún tipo de dificultad durante el proceso de escalada de privilegios, especialmente si utilizamos algún exploit o herramienta de prueba de concepto (PoC) pública. Enumerar las protecciones implementadas nos ayudará a garantizar que utilizamos métodos que no están siendo bloqueados ni detectados, y nos será útil si necesitamos crear payloads personalizados o modificar herramientas antes de compilarlas.


#### Comprobar el estado de Windows Defender

Campos importantes:
- AMServiceEnabled          → ¿Servicio de Defender activo?
- AntivirusEnabled          → ¿Antivirus habilitado?
- AntispywareEnabled        → ¿Antispyware habilitado?
- RealTimeProtectionEnabled → ¿Protección en tiempo real?
- BehaviorMonitorEnabled    → ¿Monitorización de comportamiento?
- OnAccessProtectionEnabled → ¿Protección al acceder a archivos?
- IoavProtectionEnabled     → ¿Protección de archivos descargados?
- NISEnabled                → ¿Network Inspection System?

Ejemplo:

- AntivirusEnabled              : True
- AntispywareEnabled            : True
- RealTimeProtectionEnabled    : False
- BehaviorMonitorEnabled       : False
- OnAccessProtectionEnabled    : False
- IoavProtectionEnabled        : False
- NISEnabled                   : False

En este ejemplo defender está instalado/activo, pero varias protecciones importantes
están deshabilitadas.

``` powershell

PS C:\htb> Get-MpComputerStatus


```

#### Enumerar las reglas de AppLocker

(Applocker es el software oficial de windows para bloquear aplicaciones segun como este configurado a distintos usuarios o grupos.)

Buscar:
- Qué rutas están permitidas
- Qué usuarios/grupos están afectados
- Reglas de Allow/Deny
- PathExceptions / PublisherExceptions / HashExceptions

Importante:
- S-1-1-0 = Everyone
- S-1-5-32-544 = Administrators

``` powershell

PS C:\htb> Get-AppLockerPolicy -Effective | select -ExpandProperty RuleCollections


```

#### Probar la política de AppLocker

###### Imagina que queremos comprobar si applocker bloquea cmd.exe

``` powershell

PS C:\htb> Get-AppLockerPolicy -Local | Test-AppLockerPolicy -path C:\Windows\System32\cmd.exe -User Everyone


```

Por ejemeplo , en este caso se está bloqueando cmd.exe y powershell.exe

``` powershell

PS C:\Users\htb-student> Get-AppLockerPolicy -Effective | select -ExpandProperty RuleCollections


PublisherConditions : {*\*\*,0.0.0.0-*}
PublisherExceptions : {}
PathExceptions      : {}
HashExceptions      : {}
Id                  : a9e18c21-ff8f-43cf-b9fc-db40eed693ba
Name                : (Default Rule) All signed packaged apps
Description         : Allows members of the Everyone group to run packaged apps that are signed.
UserOrGroupSid      : S-1-1-0
Action              : Allow

PathConditions      : {%SYSTEM32%\WindowsPowerShell\v1.0\powershell_ise.exe}
PathExceptions      : {}
PublisherExceptions : {}
HashExceptions      : {}
Id                  : 684d8b3e-7656-4451-8abe-2588d772db8f
Name                : Block PowerShell ISE
Description         :
UserOrGroupSid      : S-1-1-0
Action              : Deny

PathConditions      : {%PROGRAMFILES%\*}
PathExceptions      : {}
PublisherExceptions : {}
HashExceptions      : {}
Id                  : 921cc481-6e17-4653-8f75-050b80acca20
Name                : (Default Rule) All files located in the Program Files folder
Description         : Allows members of the Everyone group to run applications that are located in the Program Files
                      folder.
UserOrGroupSid      : S-1-1-0
Action              : Allow

PathConditions      : {c:\windows\system32\cmd.exe}
PathExceptions      : {}
PublisherExceptions : {}
HashExceptions      : {}
Id                  : 9b8293c1-49c7-4bbb-aa17-52c4232b1fe4
Name                : c:\windows\system32\cmd.exe
Description         :
UserOrGroupSid      : S-1-1-0
Action              : Deny

PathConditions      : {%WINDIR%\*}
PathExceptions      : {}
PublisherExceptions : {}
HashExceptions      : {}
Id                  : a61c8b2c-a319-4cd0-9690-d2177cad7b51
Name                : (Default Rule) All files located in the Windows folder
Description         : Allows members of the Everyone group to run applications that are located in the Windows folder.
UserOrGroupSid      : S-1-1-0
Action              : Allow

PathConditions      : {*}
PathExceptions      : {}
PublisherExceptions : {}
HashExceptions      : {}
Id                  : fd686d83-a829-4351-8ff4-27c7de5755d2
Name                : (Default Rule) All files
Description         : Allows members of the local Administrators group to run all applications.
UserOrGroupSid      : S-1-5-32-544
Action              : Allow

PublisherConditions : {*\*\*,0.0.0.0-*}
PublisherExceptions : {}
PathExceptions      : {}
HashExceptions      : {}
Id                  : b7af7102-efde-4369-8a89-7a6a392d1473
Name                : (Default Rule) All digitally signed Windows Installer files
Description         : Allows members of the Everyone group to run digitally signed Windows Installer files.
UserOrGroupSid      : S-1-1-0
Action              : Allow

PathConditions      : {%WINDIR%\Installer\*}
PathExceptions      : {}
PublisherExceptions : {}
HashExceptions      : {}
Id                  : 5b290184-345a-4453-b184-45305f6d9a54
Name                : (Default Rule) All Windows Installer files in %systemdrive%\Windows\Installer
Description         : Allows members of the Everyone group to run all Windows Installer files located in
                      %systemdrive%\Windows\Installer.
UserOrGroupSid      : S-1-1-0
Action              : Allow

PathConditions      : {*.*}
PathExceptions      : {}
PublisherExceptions : {}
HashExceptions      : {}
Id                  : 64ad46ff-0d71-4fa0-a30b-3f3d30c5433d
Name                : (Default Rule) All Windows Installer files
Description         : Allows members of the local Administrators group to run all Windows Installer files.
UserOrGroupSid      : S-1-5-32-544
Action              : Allow

PathConditions      : {%PROGRAMFILES%\*}
PathExceptions      : {}
PublisherExceptions : {}
HashExceptions      : {}
Id                  : 06dce67b-934c-454f-a263-2515c8796a5d
Name                : (Default Rule) All scripts located in the Program Files folder
Description         : Allows members of the Everyone group to run scripts that are located in the Program Files folder.
UserOrGroupSid      : S-1-1-0
Action              : Allow

PathConditions      : {%WINDIR%\*}
PathExceptions      : {}
PublisherExceptions : {}
HashExceptions      : {}
Id                  : 9428c672-5fc3-47f4-808a-a0011f36dd2c
Name                : (Default Rule) All scripts located in the Windows folder
Description         : Allows members of the Everyone group to run scripts that are located in the Windows folder.
UserOrGroupSid      : S-1-1-0
Action              : Allow

PathConditions      : {*}
PathExceptions      : {}
PublisherExceptions : {}
HashExceptions      : {}
Id                  : ed97d0cb-15ff-430f-b82c-8d7832957725
Name                : (Default Rule) All scripts
Description         : Allows members of the local Administrators group to run all scripts.
UserOrGroupSid      : S-1-5-32-544
Action              : Allow



PS C:\Users\htb-student>
```
</details>
