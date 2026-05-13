# GPO-Drive-Mapping-Lab
Implementation of a GPO to map a network drive for the Managers group, automating access to the \\WIN-JIFQTFFRT3V\Manager-Vault$ hidden as a Z: Drive
# Lab: Automated Drive Mapping via Group Policy

## Objective
The goal of this lab was to automate the user experience for the "Managers" group in the `JUMBO.local` domain. Instead of manually mapping paths, Active Directory automatically connects them to the `Manager-Vault$` share as their **Z: Drive**.

## The Strategy
- **Target**: Managers Security Group.
- **Network Path**: `\\DC-Server-2-19\Manager-Vault$`.
- **Drive Letter**: Z: Drive.

## Implementation Steps

### 1. Server-Side Configuration
I created a hidden share on the domain controller and set NTFS/Share permissions to allow the Managers group access.
![Share Setup](./images/01-Server-Share.png)

### 2. Group Policy Management
I created a new GPO linked to the Managers OU and configured the **Drive Maps** preference.
![GPO Configuration](./images/02-GPO-Config.png)

### 3. Verification
Logged in as a user in the Managers group and verified the drive appears automatically in File Explorer.
![Success](./images/03-Client-Verification.png)
