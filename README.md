Get-ChildItem Cert:\LocalMachine\CA |
Where-Object {
    $_.Subject -match "Microsoft TLS"
} |
Format-Table Subject, Issuer, NotAfter -AutoSize


certutil -addstore CA C:\Temp\AzureCert\OCSP04.crt


$url = "http://caissuers.microsoft.com/pkiops/certs/Microsoft%20TLS%20RSA%20Root%20G2%20-%20xsign.crt"

Invoke-WebRequest -Uri $url -OutFile "$env:TEMP\TLSRootG2-xsign.crt"


certutil -dump "$env:TEMP\TLSRootG2-xsign.crt"

certutil -addstore CA "$env:TEMP\TLSRootG2-xsign.crt"


Get-ChildItem Cert:\LocalMachine\CA |
Where-Object {$_.Subject -match "Microsoft TLS"} |
Format-Table Subject,Issuer,Thumbprint,NotAfter -AutoSize
