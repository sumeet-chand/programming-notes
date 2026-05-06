
# PROGRAMMING WITH POWERSHELL

By: Sumeet Chand

Date: May 2026

# TABLE OF CONTENTS
- [1. Terminologies](#terminologies)
- [2. Requirements](#requirements)
- [3. Installing](#installing)
- [4. Profile script](#profile-script)
- [5. Common Commands](#common-commands)
- [6. Scripts](#scripts)
- [7. Winget](#winget)
- [8. microsoft 365 azure ad](#microsoft-365-azure-ad)
- [9. microsoft 365 exchange](#microsoft-365-exchange)
- [10. SHAREPOINT ONLINE](#sharepoint-online)

# TERMINOLOGIES

# REQUIREMENTS

MAKE PROFILE
```powershell
```

# INSTALLING

MAKE PROFILE
```powershell
```

# PROFILE SCRIPT

MAKE PROFILE
```bash
PS C:\Users\Sumeet\Documents\sandbox> $PROFILE
C:\Users\Sumeet\Documents\WindowsPowerShell\Microsoft.PowerShell_profile.ps1
mkdir C:\Users\Sumeet\Documents\WindowsPowerShell # make profile directory
new-item -path C:\Users\Sumeet\Documents\WindowsPowerShell\Microsoft.PowerShell_profile.ps1 # make profile
notepad $PROFILE # open profile
Write-Host "Welcome, $env:USERNAME@$env:COMPUTERNAME Today is $(Get-Date -Format 'dddd, MMMM dd yyyy')." # add line in profile
. $PROFILE # reset profile

# RESULT
Welcome, Sumeet@SUMEETS-PC Today is Sunday, July 07 2024.
PS C:\Users\Sumeet\Documents\sandbox>
```

# COMMON COMMANDS

RUN SCRIPT BLOCK IN MULTIPLE LINES IN NON ISE POWERSHELL
```powershell
# encase your commands in a .{} e.g. the below can be copy and pasted directly into PowerShell non ISE to keep formatting. Good for large scripts
. {Send-MailMessage `
  -From "reception@drshakenovsky.com.au" `
  -To "sumeet@sapphirecs.com.au" `
  -Subject "Test Email" `
  -Body "This is a test email via Office 365 SMTP relay." `
  -SmtpServer "drshakenovsky-com-au.mail.protection.outlook.com" `
  -Port 25}

# or
```

SMTP TEST
```powershell
Send-MailMessage `
  -From "reception@drshakenovsky.com.au" `
  -To "sumeet@sapphirecs.com.au" `
  -Subject "Test Email" `
  -Body "This is a test email via Office 365 SMTP relay." `
  -SmtpServer "drshakenovsky-com-au.mail.protection.outlook.com" `
  -Port 25
```

FIND A CMDLETS PROPERTIES & METHODS
```powershell
get-mailbox | get-member

# or

Get-Help -Name Set-Mailbox -Parameter * | Sort-Object -Property Name -Descending | Format-Table -Property Name,parameterValue,type 
```

FIND A MODULES CMDLETS e.g. EXCHANGE ONLINE MODULE CMDLETS
```powershell
Get-Command -Module ExchangeOnlineMangement
```

COPY SUBDIRECTORIES CONTENTS TO PARENT THEN DELETE
```powershell
cd "C:\Users\Sumeet\Downloads"

$source = "C:\Users\Sumeet\Downloads"
$destination = "C:\Users\Sumeet\Downloads"

Get-ChildItem -Directory | ForEach-Object {
    $subDirPath = $_.FullName
    Write-Host "Copying contents from: $subDirPath to  $destination"

    # Use robocopy to move files
    robocopy "$subDirPath" "$destination" /MOV /E /Z /NP /NFL /NDL /NJH /NJS /XD "$subDirPath" /R:1 /W:1
}

# Clean up empty subdirectories
Get-ChildItem -Directory -Recurse | Where-Object { $_.GetFileSystemInfos().Count -eq 0 } | ForEach-Object {
    $directoryPath = $_.FullName
    Write-Host "Deleting directory: $directoryPath"
    $_ | Remove-Item -Force
}
```



# SCRIPTS

SCRIPTS
```powershell

```

# WINGET

SEARCH INSTALLED PACKAGES
```powershell
winget list
```

SEARCH PACKAGE
```powershell
winget search g++
```

INSTALL
```powershell
winget install LLVM
```

UNINSTALL
```powershell
winget remove LLVM
```


UPLOADING A WINGET
```powershell

```


# MICROSOFT 365 AZURE AD

CONNECT TO TENANT
```powershell
# run PowerShell (non ISE) as Admin.
Install-Module -Name AzureAD -Force
Set-ExecutionPolicy RemoteSigned
Import-Module AzureAD
Connect-AzureAD -Credential (Get-Credential)
```

GET SECURITY GROUP MEMBERS
```powershell
Get-UnifiedGroupLinks -Identity "Compliance Team" -LinkType Members | Select-Object PrimarySmtpAddress 
```

FIND ALL LICENSED USERS
```powershell
.{$users = Get-MgUser -All
$licensedUsers = @()
foreach ($user in $users) {
    try {
        $licenseDetail = Get-MgUserLicenseDetail -UserId $user.Id
        if ($licenseDetail) {
            $licensedUsers += [PSCustomObject]@{
                DisplayName = $user.DisplayName
                Email       = $user.Mail
                MobilePhone = $user.MobilePhone
                OfficePhone = $user.BusinessPhones[0]
                JobTitle    = $user.JobTitle
                Licenses    = ($licenseDetail | ForEach-Object { $_.SkuPartNumber }) -join ", "
            }
        }
    } catch {
        Write-Warning "Could not retrieve license for $($user.DisplayName)"
    }
}
# Export to CSV
$licensedUsers | Export-Csv -Path "LicensedUsers.csv" -NoTypeInformation}
```

COPY SECURITY GROUPS FROM ONE USER TO ANOTHER
```powershell
. { $src="janed@test.com"; $tgt="johns@test.com"; $tid=(Get-MgUser -UserId $tgt).Id; Get-MgUserTransitiveMemberOf -UserId $src -All | Where-Object { $_.AdditionalProperties['@odata.type'] -eq '#microsoft.graph.group' } | ForEach-Object { try { New-MgGroupMemberByRef -GroupId $_.Id -BodyParameter @{ "@odata.id"="https://graph.microsoft.com/v1.0/directoryObjects/$tid" } -ErrorAction Stop } catch { } } }
```

# MICROSOFT 365 EXCHANGE

NOTE: The module Install-Module -Name AzureAD (with related Connect-AzureAD) and MSOnline module has been replaced by the new module Install-Module Microsoft.Graph

CONNECT TO TENANT
```powershell
# run PowerShell (non ISE) as Admin.
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned # run this command, click [A] Yes to All
Install-Module Microsoft.Graph
Connect-MgGraph -Scopes "Directory.ReadWrite.All","User.ReadWrite.All","Group.ReadWrite.All","RoleManagement.ReadWrite.Directory"

# test with Get-MgUser -UserId "mail"
```

FIND MAILBOX SIZE
```powershell
Get-MailboxStatistics -Identity "user@domain.com"
```

MODIFY MAILBOX PERMISSIONS
```powershell
Find Send As Permissions:
Get-RecipientPermission -Identity cv@test.com | Where-Object { $_.Trustee -notlike "NT AUTHORITY*" }
Find Send on Behalf Permissions:
Get-Mailbox -Identity cv@test.com -Properties GrantSendOnBehalfTo | Select-Object GrantSendOnBehalfTo
Find Full Access Permissions:
Get-MailboxPermission -Identity cv@test.com | Where-Object { $_.User -notlike "NT AUTHORITY*" -and $_.IsInherited -eq $false }
Grant Full Access Permission:
Add-MailboxPermission -Identity "cv@test.com" -User "michelle.msimanga@test.com" -AccessRights FullAccess -InheritanceType All
Grant Send As Permission:
Add-RecipientPermission -Identity "cv@test.com" -Trustee "michelle.msimanga@test.com" -AccessRights SendAs
Grant Send on Behalf Permission:
Set-Mailbox -Identity "cv@test.com" -GrantSendOnBehalfTo "michelle.msimanga@test.com"
Remove Full Access Permission:
Remove-MailboxPermission -Identity "cv@test.com" -User "michelle.msimanga@test.com" -AccessRights FullAccess -InheritanceType All
Remove Send As Permission:
Remove-RecipientPermission -Identity "cv@test.com" -Trustee "michelle.msimanga@test.com" -AccessRights SendAs
Remove Send on Behalf Permission:
Set-Mailbox -Identity "cv@test.com" -GrantSendOnBehalfTo @{remove="michelle.msimanga@test.com"}
```

MODIFY CALENDAR PERMISSIONS
```powershell
To give a 365 user access to someone's calendar you first have to "add" them basic permissions called "reviewer". Example command below. Abra is given access to John's calendar.

Add-MailboxFolderPermission -Identity "John@test.com.com:\Calendar" -User "Abra@test.com.com" -AccessRights Reviewer

FolderName           User                 AccessRights                                    SharingPermissionFlags
----------           ----                 ------------                                    ----------------------
Calendar             Abra Kadabra |... {Reviewer}

Once the user is added as a reviewer (which can view calendar meetings) you can then set them to editor with "edit" access.

Set-MailboxFolderPermission -Identity "John@test.com.com:\Calendar" -User "Abra@test.com.com" -AccessRights Editor
```

FIND ALL MAILBOXES USER HAS DELEGATED ACCESS TOO
```powershell
.{
    $User = "michelle.catoltol@veruspeople.com"
    Write-Output "Checking mailbox permissions for $User..."
    Get-Mailbox -ResultSize Unlimited | ForEach-Object {
        $mailbox = $_
        $permissions = Get-MailboxPermission -Identity $mailbox.Identity | Where-Object {
            $_.User.ToString() -eq $User -and $_.AccessRights -ne "None"
        }
        if ($permissions) {
            Write-Output "User has access to mailbox: $($mailbox.Identity)"
        }
    }
    Write-Output "Done checking."
}
```

COPY MAILBOX ACCESS TO ANOTHER USER
```powershell
. { $source="janed@test.com"; $target="johns@test.com"; $targetId=(Get-Recipient $target).DistinguishedName; Get-Mailbox -ResultSize Unlimited | ForEach-Object { $mbx=$_.Identity; try { if (Get-MailboxPermission $mbx | Where-Object { $_.User -like $source -and $_.AccessRights -contains "FullAccess" -and -not $_.IsInherited }) { Add-MailboxPermission -Identity $mbx -User $target -AccessRights FullAccess -InheritanceType All -ErrorAction SilentlyContinue } } catch {}; try { if (Get-RecipientPermission $mbx | Where-Object { $_.Trustee -like $source -and $_.AccessRights -contains "SendAs" }) { Add-RecipientPermission -Identity $mbx -Trustee $target -AccessRights SendAs -Confirm:$false -ErrorAction SilentlyContinue } } catch {}; try { Set-Mailbox -Identity $mbx -GrantSendOnBehalfTo @{Add=$target} -ErrorAction SilentlyContinue } catch {} } }
```

COPY DISTRIBUTION LISTS
```powershell
. { $source="janed@test.com"; $target="johns@test.com"; Get-DistributionGroup | ForEach-Object { try { $members = Get-DistributionGroupMember -Identity $_.Guid -ResultSize Unlimited -ErrorAction Stop; if ($members.PrimarySmtpAddress -contains $source) { Add-DistributionGroupMember -Identity $_.Guid -Member $target -ErrorAction SilentlyContinue } } catch {} } }
```


# SHAREPOINT ONLINE

IMPORT SHAREPOINT ONLINE MODULE
```powershell
# run PowerShell (non ISE) as Admin.
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned # run this command, click [A] Yes to All
Install-Module PnP.PowerShell # press Y to select all
Import-Module -Name Pnp.PowerShell
```
