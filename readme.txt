=== Domains in Windows erstellen

C:\Windows\System32\drivers\etc\hosts

192.168.2.22 whoami0.aci.lan
192.168.2.22 whoami1.aci.lan
192.168.2.22 whoami2.aci.lan
192.168.2.22 whoami3.aci.lan
192.168.2.22 whoami4.aci.lan
192.168.2.22 whoami5.aci.lan
192.168.2.22 whoami6.aci.lan


=== Zertifikat erstellen und in Windows installieren

openssl genpkey -algorithm RSA -out whoami6.key

openssl req -new -key whoami6.key -out whoami6.csr -subj "/CN=whoami6.aci.lan"

openssl x509 -req -in whoami6.csr -signkey whoami6.key -out whoami6.crt

Linux:	 cat whoami6.key whoami6.crt > whoami6.pem

Windows: Get-Content whoami6.key, whoami6.crt | Set-Content -Path whoami6.pem


certutil -addstore "Root" whoami6.crt

certutil -store "Root" | find "whoami6.aci.lan"

curl --cacert whoami6.crt -w "HTTP-Status: %{http_code}" https://whoami6.aci.lan




=== Curl ausfüren

curl.exe -s -o NUL -w "HTTP-Status: %{http_code}"   http://whoami0.aci.lan       -k							HTTP-Status: 200
curl.exe -s -o NUL -w "HTTP-Status: %{http_code}"   http://whoami0.aci.lan:80    -k							HTTP-Status: 200
curl.exe -s -o NUL -w "HTTP-Status: %{http_code}"  https://whoami0.aci.lan       -k							HTTP-Status: 404
curl.exe -s -o NUL -w "HTTP-Status: %{http_code}"  https://whoami0.aci.lan									HTTP-Status: 000

curl.exe -s -o NUL -w "HTTP-Status: %{http_code}"   http://whoami1.aci.lan       -k							HTTP-Status: 200
curl.exe -s -o NUL -w "HTTP-Status: %{http_code}"   http://whoami1.aci.lan:80    -k							HTTP-Status: 200
curl.exe -s -o NUL -w "HTTP-Status: %{http_code}"   http://whoami3.aci.lan:8880  -k							HTTP-Status: 200
curl.exe -s -o NUL -w "HTTP-Status: %{http_code}"  https://whoami1.aci.lan       -k							HTTP-Status: 404
curl.exe -s -o NUL -w "HTTP-Status: %{http_code}"  https://whoami1.aci.lan									HTTP-Status: 000

curl.exe -s -o NUL -w "HTTP-Status: %{http_code}"   http://whoami2.aci.lan       -k							HTTP-Status: 404
curl.exe -s -o NUL -w "HTTP-Status: %{http_code}"   http://whoami2.aci.lan:80    -k							HTTP-Status: 404
curl.exe -s -o NUL -w "HTTP-Status: %{http_code}"  https://whoami2.aci.lan       -k							HTTP-Status: 200
curl.exe -s -o NUL -w "HTTP-Status: %{http_code}"  https://whoami2.aci.lan									HTTP-Status: 000

curl.exe -s -o NUL -w "HTTP-Status: %{http_code}"   http://whoami3.aci.lan       -k							HTTP-Status: 200
curl.exe -s -o NUL -w "HTTP-Status: %{http_code}"   http://whoami3.aci.lan:80    -k							HTTP-Status: 200
curl.exe -s -o NUL -w "HTTP-Status: %{http_code}"   http://whoami3.aci.lan:8881  -k							HTTP-Status: 200
curl.exe -s -o NUL -w "HTTP-Status: %{http_code}"  https://whoami3.aci.lan       -k							HTTP-Status: 200
curl.exe -s -o NUL -w "HTTP-Status: %{http_code}"  https://whoami3.aci.lan									HTTP-Status: 000

curl.exe -s -o NUL -w "HTTP-Status: %{http_code}"   http://whoami4.aci.lan       -k							HTTP-Status: 302
curl.exe -s -o NUL -w "HTTP-Status: %{http_code}"   http://whoami4.aci.lan:80    -k							HTTP-Status: 302
curl.exe -s -o NUL -w "HTTP-Status: %{http_code}"  https://whoami4.aci.lan       -k							HTTP-Status: 200
curl.exe -s -o NUL -w "HTTP-Status: %{http_code}"  https://whoami4.aci.lan									HTTP-Status: 000

curl.exe -s -o NUL -w "HTTP-Status: %{http_code}"   http://whoami5.aci.lan       -k							HTTP-Status: 302
curl.exe -s -o NUL -w "HTTP-Status: %{http_code}"   http://whoami5.aci.lan:80    -k							HTTP-Status: 302
curl.exe -s -o NUL -w "HTTP-Status: %{http_code}"   http://whoami5.aci.lan:8882  -k							HTTP-Status: 200
curl.exe -s -o NUL -w "HTTP-Status: %{http_code}"  https://whoami5.aci.lan       -k							HTTP-Status: 200
curl.exe -s -o NUL -w "HTTP-Status: %{http_code}"  https://whoami5.aci.lan									HTTP-Status: 000

curl.exe -s -o NUL -w "HTTP-Status: %{http_code}"   http://whoami6.aci.lan       -k							HTTP-Status: 302
curl.exe -s -o NUL -w "HTTP-Status: %{http_code}"   http://whoami6.aci.lan:80    -k							HTTP-Status: 302
curl.exe -s -o NUL -w "HTTP-Status: %{http_code}"   http://whoami6.aci.lan:8883  -k							HTTP-Status: 200
curl.exe -s -o NUL -w "HTTP-Status: %{http_code}"  https://whoami6.aci.lan       -k							HTTP-Status: 200
curl.exe -s -o NUL -w "HTTP-Status: %{http_code}"  https://whoami6.aci.lan									HTTP-Status: 200
curl.exe -s  --cacert ./certs/whoami6.crt -o NUL -w "HTTP-Status: %{http_code}"  https://whoami6.aci.lan	HTTP-Status: 200

