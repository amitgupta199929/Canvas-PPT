New-Item -ItemType Directory -Path C:\Temp\AzureCert -Force

$url = "https://www.microsoft.com/pkiops/certs/Microsoft%20TLS%20G2%20RSA%20CA%20OCSP%2004.crt"

Invoke-WebRequest -Uri $url -OutFile "C:\Temp\AzureCert\OCSP04.crt"
