
Voy a explicar de forma muy basica como se logra inyeccion de comandos en un programa hecho en python usando la libreria OS que este mal sanitizado y protegido.

Supongamos que tenemos un script de python para descomprimir carpetas comprimidas:

``` python

import os

print("Unzip program")

file = input("Type the path of the file: ")

os.system(f"unzip {file}")

```

Problemas:

El problema principal es que no valida lo que el usuario escribe en el input.

El programa ejecuta en el sistema: unzip + "archivo a descomprimir"

En linux y windows existen formas para separar comandas y que se hagan independientes aunque esten en la misma linea. Por ejemplo en linux: `";"` `"|"` `"&&"` 

Si probamos a crear un archivo llamado test.txt que contenga la palabra test de primeras no va a dejar porque esta en el contexto del comando "unzip" y en teoria es el argumento de la ruta del archivo:

<img width="455" height="105" alt="image" src="https://github.com/user-attachments/assets/e778de80-5ae5-4f98-9eae-276c0b646ced" />

Si probamos directamente a separar el comando con un `;` o `&&` o sinonimos, el programa simplemente no le hace caso al comando anterior "unzip" y se ejecuta lo que viene delante: 

<img width="657" height="248" alt="image" src="https://github.com/user-attachments/assets/9375c533-8d58-41d1-b800-b47b40f6ae07" />

<img width="658" height="170" alt="image" src="https://github.com/user-attachments/assets/cbcd2e8a-f42e-49bf-9dbf-dcbb7be0e711" />


Esto se puede llevar a comandos mas criticos, como ejecutar una shell que abra una terminal con el usuario del contexto del programa:

<img width="689" height="575" alt="image" src="https://github.com/user-attachments/assets/299e548a-39cf-4687-942c-b98cabfda6ae" />

Se me abre una terminal. 

En este caso es con mi mismo usuario , pero esta vulnerabilidad suele obtener peso cuando el programa esta corriendo con permisos del usuario administrador y otro usuario de alguna forma tiene permitido ejecutarla.







