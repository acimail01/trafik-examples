# Traefik-Examples
es sollen verschiedene Konfigurationen für Traefik für http, https, redirects, Port-Änderungen sowie Zertifikate ausgetestet werden.

## Domains in Windows erstellen
### Datei: C:\Windows\System32\drivers\etc\hosts
#### Inhalt

```BASH
127.0.0.1   whoami0.aci.lan
127.0.0.1   whoami1.aci.lan
127.0.0.1   whoami2.aci.lan
127.0.0.1   whoami3.aci.lan
127.0.0.1   whoami4.aci.lan
127.0.0.1   whoami5.aci.lan
127.0.0.1   whoami6.aci.lan
```


## Zertifikat erstellen und in Windows installieren

```BASH
openssl genpkey -algorithm RSA -out whoami6.key
openssl req -new -key whoami6.key -out whoami6.csr -subj "/CN=whoami6.aci.lan"
openssl x509 -req -in whoami6.csr -signkey whoami6.key -out whoami6.crt
```

.key/.crt to PEM
```BASH
Linux:	 cat whoami6.key whoami6.crt > whoami6.pem
Windows: Get-Content whoami6.key, whoami6.crt | Set-Content -Path whoami6.pem
```

installieren:
```BASH
certutil -addstore "Root" whoami6.crt
```

prüfen:
```BASH
certutil -store "Root" | find "whoami6.aci.lan"
```

ausführen:
```BASH
curl --cacert whoami6.crt -w "HTTP-Status: %{http_code}" https://whoami6.aci.lan
```



### Curl ausfüren

Example 0: HTTP
```BASH
curl.exe -s -o NUL -w "HTTP-Status: %{http_code}"   http://whoami0.aci.lan       -k	HTTP-Status: 200
curl.exe -s -o NUL -w "HTTP-Status: %{http_code}"   http://whoami0.aci.lan:80    -k	HTTP-Status: 200
curl.exe -s -o NUL -w "HTTP-Status: %{http_code}"  https://whoami0.aci.lan       -k	HTTP-Status: 404
curl.exe -s -o NUL -w "HTTP-Status: %{http_code}"  https://whoami0.aci.lan		HTTP-Status: 000
```

Example 1: HTTP + Port-Änderung
```BASH
curl.exe -s -o NUL -w "HTTP-Status: %{http_code}"   http://whoami1.aci.lan       -k	HTTP-Status: 200
curl.exe -s -o NUL -w "HTTP-Status: %{http_code}"   http://whoami1.aci.lan:80    -k	HTTP-Status: 200
curl.exe -s -o NUL -w "HTTP-Status: %{http_code}"   http://whoami1.aci.lan:8880  -k	HTTP-Status: 200
curl.exe -s -o NUL -w "HTTP-Status: %{http_code}"  https://whoami1.aci.lan       -k	HTTP-Status: 404
curl.exe -s -o NUL -w "HTTP-Status: %{http_code}"  https://whoami1.aci.lan		HTTP-Status: 000
```

Example 2: HTTPS
```BASH
curl.exe -s -o NUL -w "HTTP-Status: %{http_code}"   http://whoami2.aci.lan       -k	HTTP-Status: 404
curl.exe -s -o NUL -w "HTTP-Status: %{http_code}"   http://whoami2.aci.lan:80    -k	HTTP-Status: 404
curl.exe -s -o NUL -w "HTTP-Status: %{http_code}"  https://whoami2.aci.lan       -k	HTTP-Status: 200
curl.exe -s -o NUL -w "HTTP-Status: %{http_code}"  https://whoami2.aci.lan		HTTP-Status: 000
```

Example 3: HTTP + HTTPS + Port-Änderung
```BASH
curl.exe -s -o NUL -w "HTTP-Status: %{http_code}"   http://whoami3.aci.lan       -k	HTTP-Status: 200
curl.exe -s -o NUL -w "HTTP-Status: %{http_code}"   http://whoami3.aci.lan:80    -k	HTTP-Status: 200
curl.exe -s -o NUL -w "HTTP-Status: %{http_code}"   http://whoami3.aci.lan:8881  -k	HTTP-Status: 200
curl.exe -s -o NUL -w "HTTP-Status: %{http_code}"  https://whoami3.aci.lan       -k	HTTP-Status: 200
curl.exe -s -o NUL -w "HTTP-Status: %{http_code}"  https://whoami3.aci.lan		HTTP-Status: 000
```

Example 4: HTTP + HTTPS + Redirect
```BASH
curl.exe -s -o NUL -w "HTTP-Status: %{http_code}"   http://whoami4.aci.lan       -k	HTTP-Status: 302
curl.exe -s -o NUL -w "HTTP-Status: %{http_code}"   http://whoami4.aci.lan:80    -k	HTTP-Status: 302
curl.exe -s -o NUL -w "HTTP-Status: %{http_code}"  https://whoami4.aci.lan       -k	HTTP-Status: 200
curl.exe -s -o NUL -w "HTTP-Status: %{http_code}"  https://whoami4.aci.lan		HTTP-Status: 000
```

Example 5: HTTP + HTTPS + Redirect + Port-Änderung - kein Zertifikat
```BASH
curl.exe -s -o NUL -w "HTTP-Status: %{http_code}"   http://whoami5.aci.lan       -k	HTTP-Status: 302
curl.exe -s -o NUL -w "HTTP-Status: %{http_code}"   http://whoami5.aci.lan:80    -k	HTTP-Status: 302
curl.exe -s -o NUL -w "HTTP-Status: %{http_code}"   http://whoami5.aci.lan:8882  -k	HTTP-Status: 200
curl.exe -s -o NUL -w "HTTP-Status: %{http_code}"  https://whoami5.aci.lan       -k	HTTP-Status: 200
curl.exe -s -o NUL -w "HTTP-Status: %{http_code}"  https://whoami5.aci.lan		HTTP-Status: 000
```

Example 6: HTTP + HTTPS + Redirect + Port-Änderung + mit Zertifikat
```BASH
curl.exe -s -o NUL -w "HTTP-Status: %{http_code}"   http://whoami6.aci.lan       -k	HTTP-Status: 302
curl.exe -s -o NUL -w "HTTP-Status: %{http_code}"   http://whoami6.aci.lan:80    -k	HTTP-Status: 302
curl.exe -s -o NUL -w "HTTP-Status: %{http_code}"   http://whoami6.aci.lan:8883  -k	HTTP-Status: 200
curl.exe -s -o NUL -w "HTTP-Status: %{http_code}"  https://whoami6.aci.lan       -k	HTTP-Status: 200
curl.exe -s -o NUL -w "HTTP-Status: %{http_code}"  https://whoami6.aci.lan		HTTP-Status: 200
curl.exe -s  --cacert ./certs/whoami6.crt -o NUL -w "HTTP-Status: %{http_code}"  https://whoami6.aci.lan	HTTP-Status: 200
```
