Resources
| where type =~ 'microsoft.compute/virtualmachines'
| extend osDisk = properties.storageProfile.osDisk
| extend dataDisks = properties.storageProfile.dataDisks
| project
    vmName = name,
    resourceGroup,
    location,
    osDiskName = osDisk.name,
    osDiskSizeGB = osDisk.diskSizeGB,
    osDiskType = osDisk.managedDisk.storageAccountType,
    dataDisks
| mv-expand dataDisks
| project
    vmName,
    resourceGroup,
    location,
    osDiskName,
    osDiskSizeGB,
    osDiskType,
    dataDiskName = dataDisks.name,
    dataDiskSizeGB = dataDisks.diskSizeGB,
    dataDiskType = dataDisks.managedDisk.storageAccountType,
    lun = dataDisks.lun
| order by vmName asc