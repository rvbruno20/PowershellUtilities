# Azure PowerShell Scripts

This repository contains a small set of PowerShell scripts for common Azure administration and automation tasks. The scripts are intended to help with VM management, operational validation, restore testing, and Azure Virtual Desktop session host registration.

## What is in this repository?

- `scripts/avd_hostpool_registration.ps1` - Connects to an AVD session host over WinRM, installs required AVD agent packages, and registers the host using a provided registration token.
- `scripts/azure_vm_resize.ps1` - Resizes an Azure virtual machine by switching to the correct subscription, deallocating the VM if needed, updating its size, and restarting it.
- `scripts/automation_account/azure_sp_certificate_audit.ps1` - Audits Azure service principal certificates for expiration and validity.
- `scripts/automation_account/azure_sp_secret_audit.ps1` - Audits Azure service principal secrets for expiration and validity.
- `scripts/automation_account/azure_vm_start_aa.ps1` - Starts an Azure VM using automation account credentials or context.
- `scripts/automation_account/azure_vm_stop_aa.ps1` - Stops an Azure VM using automation account credentials or context.
- `scripts/service_principal/azure_backup_restore_test.ps1` - Validates a backup-and-restore workflow by authenticating to Azure, restoring a VM from Recovery Services, and cleaning up temporary restore resources.
- `scripts/service_principal/azure_vm_decommission.ps1` - Decommissions a VM and related resources such as disks and network interfaces using service principal authentication.

## Prerequisites

Before running these scripts, ensure that:

- You have PowerShell installed.
- The Az PowerShell module is installed.
- The Microsoft Graph PowerShell module (`Microsoft.Graph`) is installed if any script relies on Graph data.
- You are authenticated to Azure and have the required permissions for the resources being managed.
- For AVD session host registration, the machine running the script must be able to communicate with the session host over WinRM and must have local administrator credentials for the remote host.

Install the Azure PowerShell module if needed:

```powershell
Install-Module Az -Scope CurrentUser -Repository PSGallery
```

Install the Microsoft Graph PowerShell module if needed:

```powershell
Install-Module Microsoft.Graph -Scope CurrentUser -Repository PSGallery
```

## Authentication

These scripts typically rely on either:

- Your currently signed-in Azure session via `Connect-AzAccount`, or
- A service principal configured with the necessary RBAC permissions.

The AVD host registration script does not authenticate to Azure directly; it connects to the remote session host using WinRM and local admin credentials.

If you are using a service principal for other scripts, update the placeholder values in the script before execution.

## Required permissions

- `Contributor` or `Virtual Machine Contributor` for VM lifecycle operations
- `Disk Contributor` for managed disk operations
- `Network Contributor` for network interface cleanup
- `Recovery Services Contributor` for backup and restore workflows
- A signed-in user or identity must have access to the selected subscription via `Set-AzContext`

## Usage

1. Review the script and update any placeholder values such as subscription names, VM names, resource group names, storage account names, or local admin credentials.
2. Open PowerShell and navigate to the repository folder.
3. Run the desired script:

```powershell
./azure_vm_resize.ps1
```

## Notes

- Always review the script carefully before running it in a production environment.
- Some operations may require elevated permissions or specific Azure roles.
- The AVD host registration script requires WinRM connectivity to the target session host and local administrator access on that host.
- The scripts are designed to be clear and reusable, but they should be adjusted to match your environment and naming conventions.

## Intended use cases

- Routine VM size changes
- Azure backup restore testing
- Azure Virtual Desktop session host registration
- Temporary environment setup and cleanup for validation workflows
