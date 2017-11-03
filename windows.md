# Various Windows stuff..

## Powershell

### Make ^D and other Emacs-like keybindings work in Powershell

This drove me nuts. Hitting ^D to exit, ^L to clear screen, etc did not work. Luckily, Powershell has a profile file similar to bash/zsh:
```
"Set-PSReadlineOption -EditMode Emacs" | Add-Content $Profile
.$Profile
```

### Disable bell

The bell is also annoying. Disable it:
```
"Set-PSReadlineOption -BellStyle None" | Add-Content $Profile
.$Profile
```

### Get Powershell version
```
$PSVersionTable.PSVersion
```

## Check NTP server

```
w32tm /query /peers
```

## nbtstat stuff

- Resolve IP address to NetBIOS name:
```
nbtstat -A 10.10.10.10
```

- Show local name table:
```
nbtstat -n
```

- Show local name cache:
```
nbtstat -c
```

- Show NetBIOS statistics:
```
nbtstat -S
```

## Install Linux Subsystem
```
Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Windows-Subsystem-Linux
```
