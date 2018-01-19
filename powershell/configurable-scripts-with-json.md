# Configurable script with Json config files

When creating scripts, it might be usefull to have a config file for urls, passwords or other configurable variables.
We can load such a setting from Json and make it aviable as a global parameter in powershell using a simple function.

The function can be loaded in the same fashion as described in [Autoloading functions from files divided in directories](/autoload-functions-divided-in-directories). Defining a single file containing the config function:

```powershell
function SetConfig()
{    
	if (Test-Path "$PSScriptRoot\..\Config.json") {
		Write-Host "Setting global config parameter"
		$global:Config = Get-Content "$PSScriptRoot\..\Config.json" | ConvertFrom-Json
	} else {
		Write-Error "Missing config file, run create 'Set-Config -New' to create an empty config file"
	}
}
```

This will load the json-file specified and make it aviable on the `$global:Config`-variable.