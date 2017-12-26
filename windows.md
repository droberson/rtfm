# Various Windows stuff..

Stuff I've found useful while working with Windows systems.

## Command Line Equivalents from Linux to Windows

| Purpose      		          | Linux	      | Windows               |
| ------------------------- | ----------- | --------------------- |
| Directory list    	      | ls          | dir                   |
| Display file's contents   | cat         | type                  |
| Display file with a pager | less file   | type file &#124; more |
| Copy file    	      	    | cp 	        | copy                  |
| Delete file	              | rm          | del	                  |
| Rename file	              | mv          | ren	                  |
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
```
net users /domain
net groups /domain
```
