# Various Windows stuff..

## Powershell

### Make ^D and other Emacs-like keybindings work in Powershell

This drove me nuts. Hitting ^D to exit, ^L to clear screen, etc did not work. Luckily, Powershell has a profile file similar to bash/zsh:
```
"Set-PSReadlineOption -EditMode Emacs" | Add-Content $Profile
.$Profile
```

## Check NTP server

```
w32tm /query /peers
```
