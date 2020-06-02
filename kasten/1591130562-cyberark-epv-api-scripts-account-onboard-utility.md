# 1591130562 cyberark-epv-api-scripts-account-onboard-utility
#cyberark #onboard #automation

The script can be found [here](https://github.com/cyberark/epv-api-scripts/tree/master/Account%20Onboard%20Utility).

It requires an `accounts.csv` file e.g:
```cvs
username,address,safe,platformID,password,disableAutoMgmt,disableAutoMgmtReason
Administrator,myDomain.com,IT-DOM-ADMIN,WinDomain,pz3Rsd!iwef8,,
Administrator,APP01.myDomain.com,WIN-LCL-SRV-ADMIN,WinServerLocal,P6]xlE7L,,
```


This is a powershell script, so you can download it with the following command:
```powershell
Invoke-RestMethod https://raw.githubusercontent.com/cyberark/epv-api-scripts/master/Account%20Onboard%20Utility/Accounts_Onboard_Utility.ps1 | Set-Content -Path ./Accounts_Onboard_Utility.ps1
```

You can also pull the sample csv file by executing the command:
```powershell
Invoke-RestMethod https://raw.githubusercontent.com/cyberark/epv-api-scripts/master/Account%20Onboard%20Utility/sample_accounts.csv | Set-Content -Path "./account.csv"
```


Then execute the utility:
```powershell
.\Accounts_Onboard_Utility.ps1 -PVWAURL "https://myPVWA.myDomain.com/PasswordVault"  -CsvPath .\accounts.csv -Create
```

This will just add the 4 dummy accounts specified in the `accounts.csv`.


## Links
- [1591130388-pcloud-conjur-load-testing.md](1591130388-pcloud-conjur-load-testing.md)
