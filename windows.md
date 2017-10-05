# Various Windows stuff..

## Powershell

### Make ^D and other Emacs-like keybindings work in Powershell

This drove me nuts. Hitting ^D to exit, ^L to clear screen, etc did not work. Luckily, Powershell has a profile file similar to bash/zsh:
```
notepad $Profile
...
Set-PSReadlineOption -EditMode Emacs
...
```

## Check NTP server

```
w32tm /query /peers
```
