La persistencia en un sistema se basa en obtener acceso de forma continua despues de conseguir un primer acceso inicial en el sistema.

En este caso voy a documentar como ganar un acceso persistente creando claves de registros en windows que ejecuten programas de codigo malicioso controlado por un atackante.

Que es una clave de registro en windows?
Una clave de registro en windows se basa en una pequeña base de datos del sistema la cual contiene registros, configuraciones, preferencias de usuario, etc. Estos registros se ejecutan cada vez que el sistema se enciende, por lo cual es una forma atractiva para conseguir acceso persistente.

Tengo un codigo basico tipico de shell reversa para que la victima se conecte a mi IP:




Por otro lado, el script de powershell lo añado a una clave de registro de windows:




<img width="1920" height="1080" alt="Screenshot_2026-02-21-01-25-08_1920x1080" src="https://github.com/user-attachments/assets/4aabb229-a364-417b-a073-f44c845dcfa9" />
