
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

# ENUMERATION

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

<details>
<summary>PowerShell</summary>
  
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


### Initial Enumeration


Durante una evaluación, podemos obtener acceso a una consola con privilegios limitados en un host Windows (perteneciente o no a un dominio) y necesitar realizar una escalada de privilegios para ampliar nuestro acceso. Comprometer completamente el host puede darnos acceso a archivos/recursos compartidos confidenciales, permitirnos capturar tráfico para obtener más credenciales u obtener credenciales que nos ayuden a ampliar nuestro acceso o incluso a escalar directamente a Administrador de dominio en un entorno de Active Directory. Podemos escalar privilegios a uno de los siguientes niveles, dependiendo de la configuración del sistema y del tipo de datos que encontremos:

| Cuenta / Rol | Descripción |
| :--- | :--- |
| **NT AUTHORITY\SYSTEM** | The highly privileged `NT AUTHORITY\SYSTEM` account, or `LocalSystem` account which is a highly privileged account with more privileges than a local administrator account and is used to run most Windows services. |
| **Built-in Local Administrator** | The built-in local `administrator` account. Some organizations disable this account, but many do not. It is not uncommon to see this account reused across multiple systems in a client environment. |
| **Local Administrators Member** | Another local account that is a member of the local `Administrators` group. Any account in this group will have the same privileges as the built-in `administrator` account. |
| **Domain User in Administrators Group** | A standard (non-privileged) domain user who is part of the local `Administrators` group. |
| **Domain Admin in Administrators Group** | A domain admin (highly privileged in the Active Directory environment) that is part of the local `Administrators` group. |


Aqui dejo un enlace de una gran parte documentada de comandos de windows (cmd.exe): [windows-commands](https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/windows-commands)
Esta referencia de comandos de Windows resulta muy útil para realizar tareas de enumeración manual.


### System Information


Podemos ver que servicios y procesos estan corriendo actualmente en el host con este comando:

``` powershell

C:\htb> tasklist /svc


```

Es fundamental familiarizarse con los procesos estándar de Windows. Como por ejemplo:

[Session Manager Subsystem (smss.exe)](https://en.wikipedia.org/wiki/Session_Manager_Subsystem), [Client Server Runtime Subsystem (csrss.exe)](https://en.wikipedia.org/wiki/Client/Server_Runtime_Subsystem), [WinLogon (winlogon.exe)](https://en.wikipedia.org/wiki/Winlogon), [Local Security Authority Subsystem Service (LSASS)](https://en.wikipedia.org/wiki/Local_Security_Authority_Subsystem_Service), [and Service Host (svchost.exe)](https://en.wikipedia.org/wiki/Svchost.exe)


#### Mostrar todas las variables de entorno

Las variables de entorno explican mucho sobre la configuración del host. Para obtener una impresión de ellas, Windows proporciona el comando `set`

``` powershell

C:\htb> set

```


### Ver información detallada de configuración

El comando `systeminfo` mostrará si el equipo se ha actualizado recientemente y si se trata de una máquina virtual. Si no se ha actualizado recientemente, obtener acceso de administrador puede ser tan sencillo como ejecutar una vulnerabilidad conocida. Busque en Google las actualizaciones instaladas en [HotFixes](https://www.catalog.update.microsoft.com/Search.aspx?q=hotfix) para tener una idea de cuándo se actualizó el equipo. Esta información no siempre está disponible, ya que es posible ocultar el software de actualizaciones a los usuarios que no son administradores. `System Boot Time` También `OS Version` se puede consultar para tener una idea del nivel de parche. Si el equipo no se ha reiniciado en más de seis meses, es probable que tampoco se esté actualizando.

``` powershell

C:\htb> systeminfo

```


### Parches y actualizaciones

Si `systeminfo` no muestra las correcciones urgentes, puede consultarlas con [WMI](https://learn.microsoft.com/en-us/windows/win32/wmisdk/wmi-start-page) utilizando el binario `WMI-Command` con [QFE (Quick Fix Engineering)](https://learn.microsoft.com/en-us/windows/win32/cimwin32prov/win32-quickfixengineering) para mostrar los parches.


``` powershell

C:\htb> wmic qfe


```

##### También podemos hacerlo con PowerShell utilizando el cmdlet [Get-Hotfix](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.management/get-hotfix?view=powershell-7.6&viewFallbackFrom=powershell-7.1) .

``` powershell

PS C:\htb> Get-HotFix | ft -AutoSize


```

### Programas instalados

`WMI` también puede utilizarse para mostrar el software instalado. Esta información suele orientarnos hacia vulnerabilidades difíciles de detectar, versiones vulnerables en programas, etc.

Ejecute la herramienta `LaZagne` para comprobar si se encuentran credenciales almacenadas para dichas aplicaciones. Además, algunos programas pueden estar instalados y ejecutándose como un servicio vulnerable.

``` powershell

C:\htb> wmic product get name

```

##### Por supuesto, también podemos hacerlo con PowerShell utilizando el cmdlet [Get-WmiObject](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.management/get-wmiobject?view=powershell-5.1) .

``` powershell

PS C:\htb> Get-WmiObject -Class Win32_Product |  select Name, Version


```


### Mostrar procesos en ejecución (En red)

El comando [netstat](https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/netstat) mostrará las conexiones TCP y UDP activas, lo que nos permitirá comprender mejor qué servicios están escuchando en qué puertos, tanto a nivel local como en puertos accesibles desde el exterior. Es posible que encontremos un servicio vulnerable, accesible únicamente desde el host local (cuando se inicia sesión en él), que podamos explotar para escalar privilegios.

``` powershell

PS C:\htb> netstat -ano


```

- a → muestra todas las conexiones y también los puertos que están escuchando (LISTENING).
- n → muestra las direcciones y puertos numéricamente, sin intentar resolver nombres DNS.
- o → muestra el PID del proceso que está usando cada conexión/puerto


### Información del usuario y del grupo

Los usuarios suelen ser el eslabón más débil de una organización, especialmente cuando los sistemas están correctamente configurados y actualizados. Por ello, es fundamental analizar los usuarios y grupos del sistema, identificar a los miembros de grupos que puedan proporcionarnos privilegios administrativos, revisar los permisos del usuario actual, conocer la política de contraseñas y detectar posibles usuarios conectados que puedan ser objetivos de ataque.

Incluso cuando un sistema está bien actualizado, pueden existir otras vías para obtener información sensible. Por ejemplo, si tenemos acceso al directorio personal de un usuario que pertenece al grupo de administradores locales, podemos encontrar archivos que contengan credenciales, como un archivo de contraseñas llamado `logins.xlsx`. Esto puede facilitar considerablemente la obtención de información confidencial.


#### Usuarios registrados

Para ver cuantas sessiones hay en un mismo equipo (Por ejemplo: RDP, Consola local, etc.)

``` powershell

C:\htb> query user


```

Ojo: No enumera conexiones de red como SSH, SMB, HTTP, etc. Muestra las sesiones de inicio de sesión de Windows existentes en el host donde ejecutamos el comando, incluyendo sesiones locales y RDP


### Usuario actual

Cuando accedemos a un host, siempre debemos verificar primero con qué contexto de usuario se ejecuta nuestra cuenta. ¡A veces, ya somos SYSTEM o equivalente! Supongamos que accedemos como una cuenta de servicio. En ese caso, podemos tener privilegios como `SeImpersonatePrivilege`, que a menudo se pueden aprovechar fácilmente para escalar privilegios usando una herramienta como [Juicy Potato](https://github.com/ohpe/juicy-potato)

Ver usuario actual

``` powershell


C:\htb> echo %USERNAME%
C:\htb> whoami


```


### Privilegios del usuario actual


Como ya se mencionó, conocer los privilegios de nuestro usuario puede ser de gran ayuda para escalarlos

``` powershell

C:\htb> whoami /priv

```


#### Información del grupo de usuarios actual

¿Nuestro usuario ha heredado algún derecho a través de su pertenencia a un grupo? ¿Tiene privilegios en el entorno del dominio de Active Directory que podrían aprovecharse para obtener acceso a más sistemas?

``` powershell


C:\htb> whoami /groups


```


### Obtener todos los usuarios


También es importante saber qué otros usuarios hay en el sistema. Si obtuvimos acceso RDP a un host usando las credenciales que capturamos para un usuario 'boby' vemos a ese usuario bob_adm en el grupo de administradores locales, conviene comprobar si se están reutilizando las credenciales

Ver todos los usuarios en el HOST

``` powershell

C:\htb> net user


```


### Obtener todos los grupos


Saber qué grupos no estándar están presentes en el host puede ayudarnos a determinar para qué se utiliza el host, con qué frecuencia se accede a él, o incluso puede llevarnos a descubrir una configuración incorrecta.

Ver todos los grupos locales

``` powershell

C:\htb> net localgroup


```


#### Detalles sobre un grupo

Para ver detalles de un grupo concreto

``` powershell

C:\htb> net localgroup administrators

```

Donde `administrators` podria ser cualquier grupo que queramos ver los detalles


### Obtenga la política de contraseñas y otra información de la cuenta.

``` powershell


C:\htb> net accounts


```

Ademas de todos estos comandos, hay muchas otras guias que nos pueden ayudar en el proceso de enumeración en un sistema windows: [winprivesc-guia](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Windows%20-%20Privilege%20Escalation.md) o [winprivesc](https://swisskyrepo.github.io/InternalAllTheThings/redteam/escalation/windows-privilege-escalation/)


### Comunicación con Procesos


Uno de los mejores lugares para buscar una escalada de privilegios (privilege escalation) son los procesos que se están ejecutando en el sistema. Incluso si un proceso no se está ejecutando como administrador, puede conducir a privilegios adicionales. El ejemplo más común es descubrir un servidor web como `IIS` o `XAMPP` ejecutándose en la máquina, colocar una shell aspx/php en la máquina y obtener una shell como el usuario que ejecuta el servidor web. Generalmente, este no es un administrador, pero a menudo tendrá el token `SeImpersonate`, lo que permite que `Rogue/Juicy/Lonely Potato` proporcionen permisos de `SYSTEM`

### Tokens de Acceso

En Windows, los [tokens de acceso](https://learn.microsoft.com/en-us/windows/win32/secauthz/access-tokens) se utilizan para describir el contexto de seguridad (atributos o reglas de seguridad) de un proceso o hilo (thread). El token incluye información sobre la identidad y los privilegios de la cuenta de usuario relacionados con un proceso o hilo específico. Cuando un usuario se autentica en un sistema, su contraseña se verifica contra una base de datos de seguridad y, si se autentica correctamente, se le asignará un token de acceso. Cada vez que un usuario interactúa con un proceso, se presentará una copia de este token para determinar su nivel de privilegios

con el comando comentado anteriormente `netstat -ano` vemos los procesos que tienen conexiones en la red activas.

Lo principal a buscar con las Conexiones de Red Activas son las entradas que escuchan en direcciones de loopback (127.0.0.1 y ::1) que no están escuchando en la dirección IP (10.129.43.8) o en broadcast (0.0.0.0, ::/0). La razón de esto es que los sockets de red en localhost suelen ser inseguros debido a la idea de que "no son accesibles desde la red".

### Canalizaciones con Nombre (Named Pipes)

La otra forma en que los procesos se comunican entre sí es a través de Canalizaciones con Nombre (Named Pipes). Las canalizaciones son esencialmente archivos almacenados en memoria que se borran después de ser leídos. Cobalt Strike utiliza Canalizaciones con Nombre para cada comando (excluyendo BOF). Esencialmente, el flujo de trabajo se ve así:

- El beacon inicia una canalización con nombre de \.\pipe\msagent_12
- El beacon inicia un nuevo proceso e inyecta el comando en ese proceso, dirigiendo la salida a \.\pipe\msagent_12
- El servidor muestra lo que se escribió en \.\pipe\msagent_12

Cobalt Strike hizo esto porque si el comando que se ejecutaba era marcado por el antivirus o se bloqueaba, no afectaría al beacon (el proceso que ejecuta el comando). A menudo, los usuarios de Cobalt Strike cambiarán sus canalizaciones con nombre para enmascararse como otro programa

#### Más sobre las Canalizaciones con Nombre

Las canalizaciones se utilizan para la comunicación entre dos aplicaciones o procesos utilizando memoria compartida. Hay dos tipos de canalizaciones, [canalizaciones con nombre](https://learn.microsoft.com/en-us/windows/win32/ipc/named-pipes) y canalizaciones anónimas (anonymous pipes). Un ejemplo de una canalización con nombre es `\\.\PipeName\\ExampleNamedPipeServer`. Los sistemas Windows utilizan una implementación cliente-servidor para la comunicación por canalización. En este tipo de implementación, el proceso que crea una canalización con nombre es el servidor, y el proceso que se comunica con la canalización con nombre es el cliente. Las canalizaciones con nombre pueden comunicarse usando half-duplex, o un canal unidireccional donde el cliente solo puede escribir datos en el servidor, o duplex, que es un canal de comunicación bidireccional que permite al cliente escribir datos a través de la canalización y al servidor responder con datos a través de esa canalización. Cada conexión activa a un servidor de canalización con nombre da como resultado la creación de una nueva canalización con nombre. Todas estas comparten el mismo nombre de canalización pero se comunican utilizando un búfer de datos diferente.

Podemos usar la herramienta [PipeList](https://learn.microsoft.com/en-us/sysinternals/downloads/pipelist) de la Suite Sysinternals para enumerar instancias de canalizaciones con nombre

### Listar `named pipes` con `Pipelist`




