# 1591131570 powershell-auto-generate-csv

```powershell
 $Columns=@("username", "address", "safe", "platformID", "password", "disableAutoMgmt", "disableAutoMgmtReason")

$Values=@{
    username = @("admin", "administrator", "operator", "dev");
    safe = @("SAFE1", "SAFE2", "SAFE3", "SAFE4", "SAFE5", "SAFE6", "SAFE7", "SAFE8", "SAFE9", "SAFE10");
    platformID = @("UnixSSH")
}

Function Get-ColumnValue
{
  param(
    $Column,
    $Values
  )

  $randomValue=""
  
  if ($Values.ContainsKey($Column))
  {
    $randomValue = Get-Random -InputObject $Values["${Column}"]
  }
  else
  {
    $length=10
    $randomValue = ( -join ((0x30..0x39) + ( 0x41..0x5A) + ( 0x61..0x7A) | Get-Random -Count $length  | % {[char]$_}) ) 
  }

  Write-Output "${randomValue},"
}


$desiredLines=5000
$i=0

Write-Output "$($Columns -join ",")"
Do
{
    $line=""
    $Columns | ForEach-Object -Process {$line+=$(Get-ColumnValue -Column "$_" -Values $Values)}
    Write-Output $($line.Substring(0,$line.Length-1))
    $i = $i + 1
} While ($i -le $desiredLines) 

```



## Links
