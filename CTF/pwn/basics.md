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

gcc vuln.c -o vuln -fno-stack-protector -no-pie -m32

```

conseguir direccion en memoria de la funcion wins():

``` bash

objdump -d vuln | grep win

```

``` bash

gcc exploit.c -o exploit -m32

```

