La persistencia en un sistema se basa en obtener acceso de forma continua despues de conseguir un primer acceso inicial en el sistema.

En este caso voy a documentar como ganar un acceso persistente creando claves de registros en windows que ejecuten programas de codigo malicioso controlado por un atackante.

Que es una clave de registro en windows?
Una clave de registro en windows se basa en una pequeña base de datos del sistema la cual contiene registros, configuraciones, preferencias de usuario, etc. Estos registros se ejecutan cada vez que el sistema se enciende, por lo cual es una forma atractiva para conseguir acceso persistente.

Tengo un codigo basico tipico de shell reversa para que la victima se conecte a mi IP:




Por otro lado, el script de powershell lo añado a una clave de registro de windows:



<img width="1920" height="1080" alt="Screenshot_2026-02-07-17-43-50_1920x1080" src="https://github.com/user-attachments/assets/963a942d-20f3-438e-b87f-a6739b7a8e92" />
