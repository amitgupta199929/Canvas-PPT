Connect-AzAccount

$ResourceGroupName = "<RESOURCE_GROUP_NAME>"

# Get Restore Point Collections
$RPCs = Get-AzResource `
    -ResourceGroupName $ResourceGroupName `
    -ResourceType "Microsoft.Compute/restorePointCollections"

if (-not $RPCs) {
    Write-Host "No Restore Point Collections found." -ForegroundColor Yellow
    return
}

Write-Host "`nRestore Point Collections found:" -ForegroundColor Cyan
Write-Host "-----------------------------------"

# Print RPC details
$RPCs | Select-Object Name, ResourceGroupName, Location, ResourceId | Format-Table -AutoSize

# Confirmation
$Confirmation = Read-Host "`nDo you want to delete these Restore Point Collections? (Y/N)"

if ($Confirmation -eq "Y") {

    foreach ($RPC in $RPCs) {

        Write-Host "`nDeleting: $($RPC.Name)" -ForegroundColor Yellow

        try {
            Remove-AzResource `
                -ResourceId $RPC.ResourceId `
                -Force `
                -ErrorAction Stop

            Write-Host "Successfully deleted: $($RPC.Name)" -ForegroundColor Green
        }
        catch {
            Write-Host "Failed to delete: $($RPC.Name)" -ForegroundColor Red
            Write-Host $_.Exception.Message
        }
    }

}
else {
    Write-Host "`nDeletion cancelled." -ForegroundColor Cyan
}
