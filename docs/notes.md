# Operational notes

## Purpose

This repository is a collection of PowerShell scripts for Azure administration and operational automation. The scripts help with VM lifecycle actions, security and compliance checks, backup testing, private networking, and Microsoft 365 or Entra-related housekeeping.

## Repository structure

- [scripts/azure](scripts/azure) contains the Azure and Microsoft Graph automation scripts.
- [scripts/miscellaneous](scripts/miscellaneous) contains general helper utilities.
- [docs](docs) contains supporting documentation.

## Common prerequisites

- PowerShell 5.1 or later.
- The Az PowerShell module.
- The Microsoft Graph PowerShell module for scripts that query or update Entra resources.
- Access to the target Azure tenant and subscriptions.
- Appropriate RBAC roles for the operations being performed.

## Authentication models

The scripts in this repository are written around a few common authentication patterns:

- Interactive Azure login using Connect-AzAccount for ad-hoc administration.
- Service principal authentication for automation and unattended execution.
- Managed identity authentication for Azure Automation runbooks.
- WinRM-based remote authentication for the AVD host registration workflow.

When a script uses a service principal, update the tenant, client, and secret placeholders before running it.

## Required permissions

The exact role requirements depend on the script, but the most common permissions are:

- Contributor or Virtual Machine Contributor for VM lifecycle operations.
- Disk Contributor for disk cleanup and VM decommissioning tasks.
- Network Contributor for NSG, NIC, and private endpoint changes.
- Recovery Services Contributor or Owner for backup and restore validation scenarios.
- Microsoft Graph permissions for scripts that inspect or modify Entra applications, certificates, secrets, or groups.
- Access to the target subscription or resource group through the selected identity.

Use the least-privilege approach that still allows the script to perform its intended task.

## Typical configuration values to update

Most scripts expect you to replace placeholders for the following values:

- Subscription names or IDs.
- VM names and resource group names.
- Storage account names.
- VNet and subnet names.
- Tenant IDs, client IDs, and secret values.
- Output file paths and webhook URLs.
- Local administrator credentials for remote host registration.

## Script categories

### VM and infrastructure operations

These scripts manage Azure infrastructure directly:

- [scripts/azure/azure_vm_resize.ps1](scripts/azure/azure_vm_resize.ps1)
- [scripts/azure/azure_vm_start_aa.ps1](scripts/azure/azure_vm_start_aa.ps1)
- [scripts/azure/azure_vm_stop_aa.ps1](scripts/azure/azure_vm_stop_aa.ps1)
- [scripts/azure/azure_vm_decommission.ps1](scripts/azure/azure_vm_decommission.ps1)
- [scripts/azure/azure_nsg_isolation.ps1](scripts/azure/azure_nsg_isolation.ps1)

### Security and compliance checks

These scripts review Azure resources or Entra objects for configuration risk or expiry:

- [scripts/azure/azure_public_ip_audit.ps1](scripts/azure/azure_public_ip_audit.ps1)
- [scripts/azure/azure_sa_tls_audit.ps1](scripts/azure/azure_sa_tls_audit.ps1)
- [scripts/azure/azure_sp_certificate_audit.ps1](scripts/azure/azure_sp_certificate_audit.ps1)
- [scripts/azure/azure_sp_secret_audit.ps1](scripts/azure/azure_sp_secret_audit.ps1)
- [scripts/azure/storage-account-public-access.ps1](scripts/azure/storage-account-public-access.ps1)

### Backup and networking helpers

These scripts focus on restore validation and private networking:

- [scripts/azure/azure_backup_restore_test.ps1](scripts/azure/azure_backup_restore_test.ps1)
- [scripts/azure/create-private-endpoint.ps1](scripts/azure/create-private-endpoint.ps1)
- [scripts/azure/get_unused_private_endpoints.ps1](scripts/azure/get_unused_private_endpoints.ps1)

### Microsoft 365 and Entra helpers

These scripts interact with Microsoft Graph or related services:

- [scripts/azure/azure_remove_group_zero_users.ps1](scripts/azure/azure_remove_group_zero_users.ps1)

## Operational guidance

- Review each script before execution because several of them make changes to Azure resources.
- Use the DryRun behavior when a script supports it.
- Prefer least-privilege roles and avoid using overly broad permissions in production.
- Keep secrets out of source control and use secure stores where possible.
- Test scripts in a non-production subscription before applying them broadly.

## Troubleshooting tips

- If a script cannot find a VM or resource, verify the subscription context and names.
- If authentication fails, confirm that the identity has the required permissions and that the credentials are valid.
- If WinRM-based scripts fail, verify network connectivity, firewall rules, and remote PowerShell access on the target host.
- If a script writes to a file or exports CSV data, verify that the target path is writable and correctly formatted.
