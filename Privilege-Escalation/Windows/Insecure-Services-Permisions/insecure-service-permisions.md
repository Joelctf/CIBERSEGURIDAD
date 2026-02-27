Hoy voy a explicar como escalar privilegios en windows mediante el abuso de un permiso de servicio inseguro.

Lo primero de todo deberemos de saber que es un servicio en windows:

Un servicio en Windows
es un tipo especial de aplicación que se ejecuta en segundo plano, sin interfaz gráfica ni interacción directa del usuario, gestionada por el Service Control Manager. Se inician automáticamente al arrancar el sistema o bajo demanda para proporcionar funciones esenciales como redes, sonido, antivirus o tareas programadas

Dicho esto, como se enumeran servicios en windows?

<img width="501" height="497" alt="image" src="https://github.com/user-attachments/assets/41d22c0e-64a7-4836-b18f-b21d7cc3b576" />

Para procesos:

<img width="527" height="538" alt="image" src="https://github.com/user-attachments/assets/94cb9747-2ae0-4ef4-b4c9-113bfbf7c63a" />

Una vez sabemos como enumerar los servicios y procesos , debemos de comprender que cada uno de ellos estan corriendo como un usuario y tienen diferentes tipos de permisos.

Para ver el usuario que corre un servicio en windows:

<img width="542" height="210" alt="image" src="https://github.com/user-attachments/assets/8af534e0-4f23-415b-b0ce-818062155bbb" />

Donde `daclsvc` es el nombre del servicio y en la respuesta `LocalSystem` nos dice que lo ejecuta el administrador.


Para ver los permisos del servicio:

<img width="370" height="344" alt="image" src="https://github.com/user-attachments/assets/3a4b23e0-5e30-4a7a-b440-682d4cc76dc3" />

En este caso el servicio `daclsvc` tiene el permiso `SERVICE_CHANGE_CONFIG` y corre como administrador. Esto nos da una via potencial para escalar privilegios , ya que se podria cambiar 

1- Cambiar la ruta del binario (binPath)

2- Cambiar la cuenta con la que corre el servicio

3- Modificar parámetros de inicio

En este caso haré el primer ejemplo de demostración

