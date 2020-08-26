# 1598411807 powershell-params
#powershell #params #cheatsheet

Mandatory parameter looks like this:
```powershell
Param(
    [Parameter(Mandatory=$true)]
    [String[]]
    $ComputerName
)
```

For a parameter to represent a specific argument position:
```powershell
Param(
    [Parameter(Position=0)]
    [String[]]
    $ComputerName
)
```

For a parameter to have a default value
```powershell
Param(
    [Parameter(Mandatory=$false)]
    [String]
    $ComputerName = "defaultValue"
)



## Links
