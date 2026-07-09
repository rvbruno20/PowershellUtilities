# Azure PowerShell Scripts

This repository contains a small set of PowerShell scripts for common Azure administration and automation tasks. The scripts are intended to help with VM management, operational validation, and restore testing in Azure environments.

## What is in this repository?

- [azure_vm_resize.ps1](azure_vm_resize.ps1) - Resizes an Azure virtual machine by switching to the correct subscription, deallocating the VM if needed, updating its size, and restarting it.
- [../azure_backup_restore_test.ps1](../azure_backup_restore_test.ps1) - Validates a backup-and-restore workflow by authenticating to Azure, restoring a VM from Recovery Services, and cleaning up temporary restore resources.

## Prerequisites

Before running these scripts, ensure that:

- You have PowerShell installed.
- The Az PowerShell module is installed.
- You are authenticated to Azure and have the required permissions for the resources being managed.

Install the Azure PowerShell module if needed:

```powershell
Install-Module Az -Scope CurrentUser -Repository PSGallery
```

## Authentication

These scripts typically rely on either:

- Your currently signed-in Azure session, or
- A service principal configured with the necessary RBAC permissions.

If you are using a service principal, update the placeholder values in the script before execution.

## Usage

1. Review the script and update any placeholder values such as subscription names, VM names, resource group names, and storage account names.
2. Open PowerShell and navigate to the repository folder.
3. Run the desired script:

```powershell
./azure_vm_resize.ps1
```

## Notes

- Always review the script carefully before running it in a production environment.
- Some operations may require elevated permissions or specific Azure roles.
- The scripts are designed to be clear and reusable, but they should be adjusted to match your environment and naming conventions.

## Intended use cases

- Routine VM size changes
- Azure backup restore testing
- Temporary environment setup and cleanup for validation workflows
