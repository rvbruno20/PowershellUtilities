# Notes

## Purpose
This repository contains Azure PowerShell scripts used for VM start/stop, resizing, backup restore validation, Azure Virtual Desktop session host registration, and NSG isolation.

## Prerequisites
- PowerShell 7+
- Az PowerShell module
- Appropriate Azure RBAC permissions for Azure management scripts
- Local administrator credentials and WinRM access for AVD session host registration
- Network Contributor permissions to create and attach NSGs for the isolation script

## Authentication
- Azure management scripts should use an interactive Azure login via `Connect-AzAccount` or an appropriately configured service principal / managed identity.
- The AVD host registration script uses WinRM to connect to the remote session host and requires local admin credentials on the target VM.
- If using an Azure service principal for other scripts, ensure `applicationID`, `tenantID`, and `secretValue` are configured correctly.

## Required permissions and roles
- For VM management scripts: at minimum, use an identity with rights to the target subscription or resource group.
- Recommended roles for Azure resource operations:
  - `Contributor` on the subscription or resource group containing the VMs and dependent resources
  - `Virtual Machine Contributor` for VM lifecycle actions
  - `Disk Contributor` for managed disk deletion and management
  - `Network Contributor` for network interface cleanup
- For backup/restore scripts:
  - `Recovery Services Contributor` or `Owner` on the Recovery Services vault and target resource group
- For interactive login: the signed-in user or managed identity must have access to the subscription selected by `Set-AzContext`.

## Configuration
Update placeholder values in each script before execution:
- subscription names
- VM names
- resource group names
- storage account names
- local admin credentials for remote host registration

## Common issues
- VM not found: verify subscription mapping and VM name
- Permission denied: confirm RBAC roles
- Context errors: ensure the correct subscription is selected
- WinRM connectivity issues: verify the script host can reach the session host and that WinRM is enabled on the target machine