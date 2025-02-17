# Lab: Entra ID Sync

## Required VMs

* VN1-SRV1
* CL1
* VN1-SRV8

## Tasks

### Task 1: Install tools

On CL1, install the RSAT-Tools for Active Directory and the GPMC

### Task 2: Prepare the Sync Server

Perform this task on VN1-SRV8.

1. Enable TLS 1.2 according to <https://learn.microsoft.com/en-us/entra/identity/hybrid/connect/reference-connect-tls-enforcement>.
2. Disable IE Enhanced Security Configuration
3. Download Microsoft Entra Connect <https://www.microsoft.com/en-ie/download/details.aspx?id=47594>
4. Install Entra Connect, but do not configure it yet.

### Task 3: Prepare Active Directory

Perform this task on CL1.

1. In AD Domains and Trusts: Add the default domain as Suffix ([figure 1]).
2. Change the UPN suffix for all users.

    ````powershell
    Get-ADUser -Filter { UserPrincipalname -like '*@ad.adatum.com' } |
    ForEach-Object {
        $PSItem | Set-ADUser -UserPrincipalName (
            $PSItem.UserPrincipalName -replace `
                '(.*)@(.*)', '$1@M365x63029661.onmicrosoft.com'
            )
    }
    ````
    
### Task 4: Configure Entra ID Connect

Configure Entra Connect with Express settings.
    
[figure 1]: /images/UPN-Suffixes.png
