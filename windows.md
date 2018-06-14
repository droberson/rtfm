# Various Windows stuff..

Stuff I've found useful while working with Windows systems.

## Get Windows version
- GUI window:
```
winver
```

- Within cmd:
```
ver
```

- Using systeminfo (Powershell and cmd):
```
systeminfo |findstr OS
```

## Command Line Equivalents from Linux to Windows

| Purpose      		        | Linux	      | Windows               |
| ------------------------- | ----------- | --------------------- |
| Directory list    	    | ls          | dir                   |
| Display file's contents   | cat         | type                  |
| Display file with a pager | less file   | type file &#124; more |
| Copy file    	      	    | cp 	      | copy                  |
| Delete file	            | rm          | del	                  |
| Rename file	            | mv          | ren	                  |
| Show network interfaces   | ifconfig -a | ipconfig /all         |
| Show running processes    | ps          | tasklist              |
| Search for strings        | grep        | findstr &#124; find   |
| Terminate a process       | kill        | taskkill              |
| Change file attributes    | chmod       | attrib                |
| Clear screen              | clear       | cls                   |
| Compare files             | diff        | fc                    |
| Show disk usage           | df          | freedisk              |
| Show memory usage         | free        | mem                   |
| System information        | uname -a    | systeminfo            |

Further reading on Windows command line nonsense:
- http://ss64.com/nt/

## Windows Platform Tokens

These strings are found in user agent strings in browsers amongst
other places and can be used to identify a machine's operating system:

| Platform Token  | Description                                   |
| --------------- | --------------------------------------------- |
| Windows NT 10.0 | Windows 10                                    |
| Windows NT 6.3  | Windows 8.1                                   |
| Windows NT 6.2  | Windows 8                                     |
| Windows NT 6.1  | Windows 7                                     |
| Windows NT 6.0  | Windows Vista                                 |
| Windows NT 5.2  | Windows Server 2003 or Windows XP x64 Edition |
| Windows NT 5.1  | Windows XP                                    |
| Windows NT 5.01 | Windows 2000 SP1                              |
| Windows NT 5.0  | Windows 2000                                  |
| Windows NT 4.0  | Microsoft Windows NT 4.0                      |
| Windows 98      | Windows ME (Millennium Edition)               |
| Win 9x 4.90     | Windows ME                                    |
| Windows 98      | Windows 98                                    |
| Windows 95      | Windows 95                                    |
| Windows CE      | Windows CE                                    |

Further reading:

- https://en.wikipedia.org/wiki/List_of_Microsoft_Windows_versions
- https://msdn.microsoft.com/en-us/library/ms537503%28v=vs.85%29.aspx

## Get password lockout threshold/duration.
```
net accounts
```

This will give you output similar to this:
```
Force user logoff how long after time expires?:       Never
Minimum password age (days):                          1
Maximum password age (days):                          90
Minimum password length:                              7
Length of password history maintained:                24
Lockout threshold:                                    9
Lockout duration (minutes):                           30
Lockout observation window (minutes):                 30
Computer role:                                        WORKSTATION
The command completed successfully.
```

This is very useful information to have when password spraying or launching brute
force attacks. Basically, configure your tool to send _Lockout threshold - 1_ attempts
every _Lockout observation window_ minutes. This will avoid accounts being locked out,
which would reveal your attack.

## Reset a Windows 10 machine 
1. Start your PC and select the Power icon in the bottom right-hand corner of the lock screen.
2. While holding down the Shift key, select Restart.
3. On the Boot options menu, select Troubleshoot > Reset this PC > Remove everything.
4. Your PC will restart and will reset itself.

https://support.microsoft.com/en-us/help/4028457/windows-reset-your-windows-10-local-account-password


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

## AD user/group enumeration
- Get all users or groups:
```
net users /domain
net groups /domain
```

- Get specific user or group:
```
net users USERNAME_HERE /domain
net groups GROUP_HERE /domain
```

## Check for Meltdown/Spectre
As Administrator:
```
Install-Module SpeculationControl
Get-SpeculationControlSettings
```

## Install chocolately
Like apt/brew/pkg/yum/..., but for Windows!
```
Set-ExecutionPolicy Bypass -Scope Process -Force; iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))
```

## Select copy/middle click paste
I frequently swap back and forth between X11-based window managers and Windows.
The functionality of select copies and middle click pastes is very handy for me;
Control-C doesnt kill programs in Linux and I always forget to Control-C to copy
in Windows. This has been one of the most annoying things about using different
types of systems.

Luckily, I was able to achieve some of this functionality in Windows using
_AutoHotKey_, but this is still not perfect because some applications such as
putty and powershell don't honor Control-C and Control-V:
```
choco install autohotkey
```

I used this script that I copied from https://autohotkey.com/board/topic/44064-copy-on-select-implementation/
but removed the annoying tooltip:

*select-copy-middle-paste.ahk*
```
mousedrag_treshold := 20 ; pixels
middleclick_available := 15 ; seconds

Hotkey mbutton, paste_selection
Hotkey mbutton, off
Hotkey rbutton, cancel_paste
Hotkey rbutton, off
    
    
#IfWinNotActive ahk_class ConsoleWindowClass
~lButton::
  MouseGetPos, mousedrag_x, mousedrag_y
  keywait lbutton
  mousegetpos, mousedrag_x2, mousedrag_y2
  if (abs(mousedrag_x2 - mousedrag_x) > mousedrag_treshold
    or abs(mousedrag_y2 - mousedrag_y) > mousedrag_treshold)
  {
    wingetclass class, A
    if (class == "Emacs")
      sendinput !w
    else
      sendinput ^c
    settimer cleanup, % middleclick_available * 1000
    hotkey mbutton, on
    hotkey rbutton, on
  }
  return
#IfWinNotActive
  
paste_selection:
  sendinput {lbutton}
  WinGetClass class, A
  if (class == "Emacs")
    SendInput ^y
  else
    SendInput ^v
  gosub cleanup
  return
  
cancel_paste:
  sendinput {rbutton}
  gosub cleanup
  return  
  
cleanup:
  Hotkey mbutton, off
  Hotkey rbutton, off
  SetTimer cleanup, off
  tooltip
  Return
  
  
;; clipx
^mbutton::
  sendinput ^+{insert}
  return
```

## Enable RDP remotely
- Using psexec
```
psexec \\host reg add "HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Terminal Server" /v fDenyTSConnections /t REG_DWORD /d 0 /f
```
- Using remote registry
```
reg add "\\host\HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server" /v fDenyTSConnections /t REG_DWORD /d 0 /f
```
