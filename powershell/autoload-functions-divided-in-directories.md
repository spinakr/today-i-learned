# Autoloading functions from files divided in directories
When creating a powershell function or module it is convinient to be able to divide functions into private and public functions. This can be achieved with a simple function that can be loaded in the "main" script or function.

Given a directory structur

- My-Powershell-Project
    - Public
        - My-Public-Function.ps1
    - Private
        - My-Private-Function.ps1
    - Functions.psm1
    - My-Powershell-Module.psm1

```powershell
#Functions.psm1
$Public = @( Get-ChildItem -Path "$PSScriptRoot\Public\*.ps1" )
$Private = @( Get-ChildItem -Path "$PSScriptRoot\Private\*.ps1" )

@($Public + $Private) | ForEach-Object {
    Try {
        . $_.FullName
    } Catch {
        Write-Error -Message "Failed to import function $($_.FullName): $_"
    }
}

Export-ModuleMember -Function $Public.BaseName
Export-ModuleMember -Variable 'LogPath'
```

In the main program script, all private og public functions can be loaded with one import statement.

```powershell
#My-Powershell-Module.psm1
param(
    [Parameter(Mandatory=1)][string]$MyParameter,
)

Import-Module .\ReleaseScripts.psm1


#script implementation ...
```

When importing the main module, only the public functions will be aviable from outside den scope of the module itself.