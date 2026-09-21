$inputdata= Import-CSV -Path "C:\Users\amgupta_2ND\Desktop\New Text Document.csv"
$count = 0
foreach($rec in $inputdata){
$subscriptionId=$rec.subscriptionId
$VMName=$rec.dataSourceId
$resourceGroup=$rec.ResourceGroup
$count++
$count
}

# Parameters
#$subscriptionId = "Enterprise Dev/Test"
#$vaultName = "AZ-USEA-DEV-ADHOC-BKP"
#$vaultRG = "AZEASTGENTRIENC-POC01"
#$vmName = "USTRNTDA369"

$subscriptionId =
$vaultName =
$vaultRG =
$vmName =

# Set context
Set-AzContext -Subscription $subscriptionId

# Get and set Recovery Services Vault context
$vault = Get-AzRecoveryServicesVault -Name $vaultName -ResourceGroupName $vaultRG
Set-AzRecoveryServicesVaultContext -Vault $vault

# Try to get active (protected) backup item
$container = Get-AzRecoveryServicesBackupContainer -ContainerType "AzureVM" | Where-Object { $_.FriendlyName -eq $vmName }
$activeItem = $null
if ($container) {
    $activeItem = Get-AzRecoveryServicesBackupItem -Container $container -WorkloadType "AzureVM" | Where-Object { $_.SourceResourceId -match $vmName }
}

# If backup is active ? disable and delete restore points
if ($activeItem) {
    Write-Host "Backup is active for $vmName. Disabling and removing restore points..."
    Disable-AzRecoveryServicesBackupProtection -Item $activeItem -RemoveRecoveryPoints -Force
    Write-Host "Backup protection disabled and restore points removed for $vmName."
}
