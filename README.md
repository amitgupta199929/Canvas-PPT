High Level Steps

You will perform these phases:
	1.	Prepare Azure prerequisites
	2.	Prepare VMware prerequisites
	3.	Deploy Azure Arc Resource Bridge
	4.	Connect vCenter to Azure Arc
	5.	Enable Azure Arc on VMware VMs
	6.	Verify Arc connection
	7.	Enable Update Manager
	8.	Patch VMs using Update Manager

⸻

Phase 1: Azure Prerequisites

Step 1: Register required Resource Providers

Go to:

Azure Portal → Subscriptions → Your Subscription → Resource Providers

Register:
	•	Microsoft.HybridCompute
	•	Microsoft.GuestConfiguration
	•	Microsoft.HybridConnectivity
	•	Microsoft.AzureArcData
	•	Microsoft.Compute
	•	Microsoft.ConnectedVMwarevSphere

⸻

Step 2: Create Resource Group

Azure Portal → Resource Groups → Create

Example:

Name: rg-arc-vmware-prod
Region: East US / Central India (recommended same region)

⸻

Step 3: Create Azure Arc VMware vCenter resource

Go to:

Azure Portal → Search:

Azure Arc → VMware vCenters

Click:

Create

Provide:
	•	Subscription
	•	Resource Group
	•	Name: arc-vcenter-prod
	•	Region

Click Next (Resource Bridge will be deployed later)

⸻

Phase 2: VMware Prerequisites

Ensure:

vCenter version supported:
	•	vCenter 6.7 or higher
	•	ESXi 6.7 or higher

Required access:

Create vCenter account:

Example:

azurearcuser

Permissions required:
	•	Read all
	•	VM guest operations
	•	Snapshot
	•	Inventory read

⸻

Phase 3: Deploy Azure Arc Resource Bridge

This is the most important component.

Azure Arc Resource Bridge = connector between Azure and VMware

⸻

Step 1: Download Resource Bridge OVA

Azure Portal →

Azure Arc → VMware vCenters → Add

Download OVA file

Example:

ArcResourceBridge.ova

⸻

Step 2: Deploy OVA in VMware vCenter

Login to vCenter

Right click cluster → Deploy OVF Template

Select:

ArcResourceBridge.ova

Provide:

Name: arc-resource-bridge

Configure:

CPU: 4
RAM: 16 GB
Disk: 100 GB

Network: Same network as vCenter

Power ON VM

⸻

Step 3: Configure Resource Bridge

Login to VM console

Set:

IP address
DNS
Gateway

Then login via browser:

https://ResourceBridgeIP

Login credentials provided during deployment

⸻

Step 4: Connect Resource Bridge to Azure

Run command from Azure Portal:

Azure Arc → Resource Bridge → Add

Copy command and run inside Resource Bridge VM

Example command:

az arcappliance run vmware

Follow wizard

This connects VMware to Azure.

⸻

Phase 4: Connect vCenter to Azure Arc

Azure Portal →

Azure Arc → VMware vCenters → Add vCenter

Provide:
	•	vCenter FQDN/IP
	•	Username
	•	Password
	•	Resource Bridge

Click Add

Now Azure discovers all VMware VMs

⸻

Phase 5: Enable Azure Arc on VMware VMs

Azure Portal →

Azure Arc → VMware vCenters → Select vCenter → Inventory

You will see all VMs

Select VM → Enable Azure Arc

This installs:

Azure Connected Machine Agent

Now VM becomes Azure Arc server

⸻

Phase 6: Verify Azure Arc connection

Go to:

Azure Portal → Azure Arc → Machines

You should see:

VM name
Status: Connected

⸻

Phase 7: Enable Update Manager

Azure Portal →

Search:

Update Manager

Click:

Machines

Select Azure Arc machines

Click:

Enable Update Manager

⸻

Phase 8: Configure Patch Schedule

Azure Portal →

Update Manager → Create Maintenance Configuration

Provide:

Name: vmware-patch-schedule

Select:

Scope → Azure Arc machines

Configure:

Patch classification:
	•	Critical
	•	Security
	•	Update Rollups

Schedule:

Example:

Every Sunday 2 AM

Save

⸻

Phase 9: Run Patch Update

Manual update:

Update Manager → Machines → Select VM

Click:

Check for updates

Then:

Install updates

⸻

Phase 10: Monitor Patch Status

Azure Portal →

Update Manager → History

You can see:
	•	Success
	•	Failed
	•	Pending

⸻

What happens in background

Azure Update Manager communicates via Azure Arc agent installed on VMware VM.

No direct VMware integration required for patching.

⸻

Required Ports

VM → Azure

Outbound:

443 HTTPS

Required URLs:

*.azure.com
*.microsoft.com

⸻

Required Agent Installed Automatically

Azure Connected Machine Agent

Path:

C:\Program Files\AzureConnectedMachineAgent

⸻

Real Production Architecture

VMware vCenter
│
Resource Bridge VM
│
Azure Arc
│
Update Manager
│
Patch VMware VMs

⸻

Microsoft Official Documentation

Azure Arc VMware:

https://learn.microsoft.com/en-us/azure/azure-arc/vmware-vsphere/overview

Resource Bridge:

https://learn.microsoft.com/en-us/azure/azure-arc/vmware-vsphere/deploy-resource-bridge

Update Manager:

https://learn.microsoft.com/en-us/azure/update-manager/overview

⸻

Best Practice Recommendation

Deploy Resource Bridge inside VMware environment

Do NOT install agents manually

Use Azure Arc automatic onboarding

⸻

If you want, I can create:

• Full production guide Word document
• Step-by-step with screenshots
• Exact commands to run
• Troubleshooting guide

Just tell me.