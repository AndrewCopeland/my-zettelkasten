# 1591131570 powershell-auto-generate-csv

- `$Columns` is the columns required by the csv.
- `$Values` is the value of the columns random selected from the list. if empty than nothing is placed in that rows column, If column is not seen then a random 10 alphanumeric string will be printed
- `$Rows` is the amount of rows you want generated for the CSV.

```powershell
$Columns=@("username", "address", "safe", "platformID", "password", "disableAutoMgmt", "disableAutoMgmtReason")

$Values=@{
    username = @("admin", "administrator", "operator", "dev");
    safe = @("SAFE1", "SAFE2", "SAFE3", "SAFE4", "SAFE5", "SAFE6", "SAFE7", "SAFE8", "SAFE9", "SAFE10");
    platformID = @("UnixSSH");
    disableAutoMgmt=@("");
    disableAutoMgmtReason=@("");
}

$Rows=10

Function Get-ColumnValue
{
  param(
    $Column,
    $Values
  )

  $randomValue=""
  
  if ($Values.ContainsKey($Column))
  {
    $columnValues = $Values["${Column}"]
    $randomValue = ""
    if ($columnValues)
    {
      $randomValue = Get-Random -InputObject $Values["${Column}"]
    }
  }
  else
  {
    $length=10
    $randomValue = ( -join ((0x30..0x39) + ( 0x41..0x5A) + ( 0x61..0x7A) | Get-Random -Count $length  | % {[char]$_}) ) 
  }

  Write-Output "${randomValue},"
}


$i=0

Write-Output "$($Columns -join ",")"
Do
{
    $line=""
    $Columns | ForEach-Object -Process {$line+=$(Get-ColumnValue -Column "$_" -Values $Values)}
    Write-Output $($line.Substring(0,$line.Length-1))
    $i = $i + 1
} While ($i -le $Rows) 
```

Below was the generated output:
```csv
username,address,safe,platformID,password,disableAutoMgmt,disableAutoMgmtReason
operator,6sNEQCThfi,SAFE7,UnixSSH,Tb6S4hvcI2,,
administrator,26H3Ct4LZw,SAFE1,UnixSSH,LAnOSvFUxW,,
administrator,bY0Zd4wJIr,SAFE7,UnixSSH,5Sd8Jz0Er4,,
administrator,HYFLIWUn1b,SAFE7,UnixSSH,GzsO8DXd1x,,
dev,74p9c6maZF,SAFE8,UnixSSH,uMsL1rkpYR,,
dev,Km6dkhVwT3,SAFE4,UnixSSH,EAOGtSV69x,,
operator,4HXmibUZLg,SAFE2,UnixSSH,cXgDYRQhFJ,,
administrator,tkKNCgd21y,SAFE3,UnixSSH,oeHGkjINS4,,
dev,lDAf5QMe79,SAFE10,UnixSSH,aUfGkSHDoh,,
admin,TDwHyKu4Qb,SAFE6,UnixSSH,fLQ9uP0A7W,,
administrator,uatXdDEhby,SAFE3,UnixSSH,J450QXnzyk,,
```


## Links
