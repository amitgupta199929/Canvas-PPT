Connect-AzAccount
$subscription = ""
$ResourceGroupName = "<RESOURCE_GROUP_NAME>"
$resourceName = ""
Get Restore Point Collections
$sub = Select-AzSubscription -Name $subscription
$RPCs = Get-AzResource -Name $resourceName -ResourceGroupName $ResourceGroupName -ResourceType "Microsoft.Compute/restorePointCollections"

if (-not $RPCs) { Write-Host "No Restore Point Collections found." -ForegroundColor Yellow return }

Write-Output "`nRestore Point Collections found:" -ForegroundColor Cyan Write-Output "-----------------------------------"

Print RPC details
$RPCs | Select-Object Name, ResourceGroupName, Location, ResourceId | Format-Table -AutoSize

Remove-AzResource -ResourceId $RPC.ResourceId -Force -ErrorAction Continue

Write-Output "Successfully deleted: $($RPC.Name)" -ForegroundColor Green
