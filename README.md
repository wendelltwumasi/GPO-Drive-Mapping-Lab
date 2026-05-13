# GPO-Drive-Mapping-Lab
Implementation of a GPO to map a network drive for the Managers group, automating access to the \\WIN-JIFQTFFRT3V\Manager-Vault$ hidden as a Z: Drive
# Windows Server Administration: Automated Drive Mapping via GPO

## 1. Project Overview
This project demonstrates the implementation of a **Group Policy Object (GPO)** to automate the mapping of network resources for specific user groups within the `JUMBO.local` domain. By using Group Policy Preferences (GPP), we eliminate the need for manual drive mapping, ensuring a seamless and consistent user experience for the management team.

### The Strategy
The core objective is to instruct Active Directory: *"If a user belongs to the Managers group, automatically connect them to the Manager-Vault share as their Z: Drive"*.

## 2. Technical Specifications
*   **Domain:** JUMBO.local
*   **Server:** DC-Server-2-19
*   **Target Group:** Managers
*   **Network Path:** `\\WIN-JIFQTFFRT3V\Manager-Vault$`
*   **Drive Letter:** Z:
*   **Protocol:** SMB (Server Message Block)

## 3. Implementation Process

### Phase I: Secure File Share Configuration
Before automation, the destination must be secured. A hidden share was created to prevent casual browsing by unauthorized users.
1. **Creation**: Created folder `Manager-Vault` on the file server.
2. **Hidden Share**: Shared the folder as `Manager-Vault$` (the `$` suffix hides the share from network discovery).
3. **Permissions**: 
   - **Share Permissions**: Managers Group - Change/Read.
   - **NTFS Permissions**: Managers Group - Modify.

> **Screenshot:** [Insert image of Share/Security properties here]

### Phase II: GPO Development & Linking
Using the Group Policy Management Console (GPMC), a new policy was created to handle the logic.
1. **GPO Name**: `GPO_Map_Manager_Drive`.
2. **Scope**: Linked directly to the **Managers Organizational Unit (OU)** to ensure precise targeting.
3. **Policy Path**: `User Configuration` > `Preferences` > `Windows Settings` > `Drive Maps`.

> **Screenshot:** [Insert image of GPMC showing the GPO linked to the OU here]

### Phase III: Drive Map Preference Settings
The specific mapping instruction was configured with the following parameters:
- **Action**: `Update` (Ensures the drive is mapped or updated upon each login).
- **Location**: `\\WIN-JIFQTFFRT3V\Manager-Vault$`.
- **Reconnect**: Enabled (to persist across sessions).
- **Label**: `Manager Vault`.
- **Drive Letter**: `Z`.

> **Screenshot:** [Insert image of the 'New Drive Properties' window here]

## 4. Verification and Testing
To ensure the policy was applied successfully, the following commands and checks were performed on a client workstation:

1. **Policy Update**: Executed `gpupdate /force` in the command prompt.
2. **Result Confirmation**:
   - Logged in as a user within the **Managers** group.
   - Verified that the **Z: Drive** appeared automatically in File Explorer.
   - Attempted access from a non-manager account to verify security restrictions.

## 5. Troubleshooting & Observations
- **Hidden Shares**: Using the `$` suffix is a best practice for administrative or departmental vaults to reduce the attack surface for internal reconnaissance.
- **Item-Level Targeting**: For more complex environments, I recommend using Item-Level Targeting (ILT) within the GPO to filter by Security Group membership even if the GPO is linked at a higher domain level.
