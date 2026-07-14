# Azure PowerShell Scripts

This repository contains a set of PowerShell scripts for Azure administration, operational auditing, and general automation tasks. The scripts are intended to support day-to-day Azure operations such as VM lifecycle management, backup validation, storage security checks, private endpoint work, and Azure Virtual Desktop host onboarding.

## Repository layout

- [scripts/azure](scripts/azure) contains the Azure-focused automation scripts.
- [scripts/miscellaneous](scripts/miscellaneous) contains general-purpose helper scripts.
- [docs](docs) contains supporting notes and documentation.

## Script inventory

| Script | Purpose | Notes |
| --- | --- | --- |
| [scripts/azure/avd_hostpool_registration.ps1](scripts/azure/avd_hostpool_registration.ps1) | Registers an Azure Virtual Desktop session host over WinRM. | Requires remote PowerShell access and local administrator credentials on the target host. |
| [scripts/azure/azure_backup_restore_test.ps1](scripts/azure/azure_backup_restore_test.ps1) | Tests Azure VM backup and restore workflows. | Uses a service principal, creates temporary restore resources, and removes them afterward. |
| [scripts/azure/azure_nsg_isolation.ps1](scripts/azure/azure_nsg_isolation.ps1) | Applies an emergency deny-all NSG to VMs in accessible subscriptions. | Destructive by design; use with caution. |
| [scripts/azure/azure_public_ip_audit.ps1](scripts/azure/azure_public_ip_audit.ps1) | Audits public IP addresses across subscriptions. | Writes results to a configurable CSV path. |
| [scripts/azure/azure_remove_group_zero_users.ps1](scripts/azure/azure_remove_group_zero_users.ps1) | Reviews Microsoft 365 groups and logs or removes groups with no members. | Requires Microsoft Graph connectivity and appropriate permissions. |
| [scripts/azure/azure_sa_tls_audit.ps1](scripts/azure/azure_sa_tls_audit.ps1) | Ensures storage accounts are configured to use TLS 1.2 minimum. | Reads a list of storage accounts from a file and updates each subscription context. |
| [scripts/azure/azure_sp_certificate_audit.ps1](scripts/azure/azure_sp_certificate_audit.ps1) | Audits application registrations for expiring certificates. | Sends notifications through a Slack webhook when issues are detected. |
| [scripts/azure/azure_sp_secret_audit.ps1](scripts/azure/azure_sp_secret_audit.ps1) | Audits application registrations for expiring secrets. | Similar to the certificate audit but for client secrets. |
| [scripts/azure/azure_vm_decommission.ps1](scripts/azure/azure_vm_decommission.ps1) | Removes a VM and dependent resources such as disks and NICs. | Uses service principal authentication and is destructive. |
| [scripts/azure/azure_vm_resize.ps1](scripts/azure/azure_vm_resize.ps1) | Resizes one or more Azure VMs. | Supports a DryRun mode via the script parameter. |
| [scripts/azure/azure_vm_start_aa.ps1](scripts/azure/azure_vm_start_aa.ps1) | Starts VMs from an Automation Account managed identity context. | Useful for scheduled start operations. |
| [scripts/azure/azure_vm_stop_aa.ps1](scripts/azure/azure_vm_stop_aa.ps1) | Stops VMs from an Automation Account managed identity context. | Useful for scheduled stop operations. |
| [scripts/azure/create-private-endpoint.ps1](scripts/azure/create-private-endpoint.ps1) | Creates private endpoints for storage accounts from a CSV file. | Requires a target VNet, subnet, and resource group. |
| [scripts/azure/get_unused_private_endpoints.ps1](scripts/azure/get_unused_private_endpoints.ps1) | Identifies private endpoints that do not appear to be in use. | Exports results to CSV when an output path is provided. |
| [scripts/azure/storage-account-public-access.ps1](scripts/azure/storage-account-public-access.ps1) | Audits or remediates storage accounts that allow public network access. | Uses a local variables file and expects additional configuration values. |
| [scripts/miscellaneous/password_generator.ps1](scripts/miscellaneous/password_generator.ps1) | Generates a random password through a public API. | Useful for supplying temporary secret values to other automation workflows. |

## Prerequisites

Before running these scripts, ensure that:

- PowerShell 5.1 or later is available.
- The Az PowerShell module is installed.
- The Microsoft Graph PowerShell module is installed when a script uses Microsoft Graph.
- You have an authenticated Azure session or the required automation identity available.
- You have the Azure RBAC permissions needed for the specific operation.

Install the main dependencies with:

```powershell
Install-Module Az -Scope CurrentUser -Repository PSGallery
Install-Module Microsoft.Graph -Scope CurrentUser -Repository PSGallery
```

## Authentication

These scripts use one of the following approaches:

- Interactive sign-in with Connect-AzAccount for ad-hoc administration.
- Service principal authentication for scripted or automation-driven operations.
- Managed identity authentication for Azure Automation scenarios.
- WinRM and local administrator credentials for the AVD host registration workflow.

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
