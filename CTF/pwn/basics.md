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
