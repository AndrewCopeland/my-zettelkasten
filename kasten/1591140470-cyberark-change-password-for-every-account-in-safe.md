# 1591140470 cyberark-change-password-for-every-account-in-safe
#cyberark #cpm

You can use the following powershell script to mark every account within a safe for a change:


```powershell
param(
    [Parameter(Mandatory=$true)]
    [String]
    $SafeName,
    [Parameter(Mandatory=$true)]
    [String]
    $PVWAUrl,
    $AuthnType = "Cyberark",
    [PSCredential]$Credential
)

New-PASSession -Credential $cred -BaseURI "$PVWAUrl" -type "CYBERARK"
Get-PASLoggedOnUser


$accounts = Get-PASAccount -filter "safeName eq $SafeName" -limit 1000
foreach($a in $accounts)
{
    Invoke-PASCPMOperation -AccountID $a.id -ChangeTask
    Write-Host "Marked account $($a.id) $($a.name) for change"
}
```



## Links
- [1591130388-pcloud-conjur-load-testing.md](1591130388-pcloud-conjur-load-testing.md)
