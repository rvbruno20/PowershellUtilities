# Notes

## Purpose
This repository contains Azure PowerShell scripts used for VM start/stop, resizing, and backup restore validation.

## Prerequisites
- PowerShell 7+
- Az PowerShell module
- Appropriate Azure RBAC permissions

## Authentication
- Automation scripts use managed identity
- Restore script may require a service principal

## Configuration
Update placeholder values in each script before execution:
- subscription names
- VM names
- resource group names
- storage account names

## Common issues
- VM not found: verify subscription mapping and VM name
- Permission denied: confirm RBAC roles
- Context errors: ensure the correct subscription is selected