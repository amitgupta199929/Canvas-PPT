Get-ChildItem Cert:\LocalMachine\CA |
Where-Object {
    $_.Subject -match "Microsoft TLS"
} |
Format-Table Subject, Issuer, NotAfter -AutoSize


certutil -addstore CA C:\Temp\AzureCert\OCSP04.crt
