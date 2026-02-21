La persistencia en un sistema se basa en obtener acceso de forma continua despues de conseguir un primer acceso inicial en el sistema.

En este caso voy a documentar como ganar un acceso persistente creando claves de registros en windows que ejecuten programas de codigo malicioso controlado por un atackante.

Que es una clave de registro en windows?
Una clave de registro en windows se basa en una pequeña base de datos del sistema la cual contiene registros, configuraciones, preferencias de usuario, etc. Estos registros se ejecutan cada vez que el sistema se enciende, por lo cual es una forma atractiva para conseguir acceso persistente.

Tengo un codigo basico tipico de shell reversa para que la victima se conecte a mi IP:




Por otro lado, el script de powershell lo añado a una clave de registro de windows:




<img width="842" height="93" alt="2026-02-21_02-15" src="https://github.com/user-attachments/assets/2d577e5e-d023-43c1-93bb-d07ae687593d" />
