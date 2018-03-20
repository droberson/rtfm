# Interacting with services

Here are some notes about interacting with various services. Often, the correct tool or information
that can be gleaned isn't immediately apparent.

## TCP
- [7 / echo](#echo)
- [21 / FTP](#ftp)
- [22 / SSH](#ssh)
- [23 / Telnet](#telnet)
- [25 / SMTP](#smtp)
- [79 / Finger](#finger)
- [80 / HTTP](#http)
- [443 / HTTPS](#http)
- [1433 / MS-SQL](#ms-sql)
- [2049 / NFS](#nfs)
- [4786 / Cisco Smart Install](#cisco-smart-install)
- [44818 / EtherNet-IP](#ethernet-ip)

## UDP
- [161 / SNMP](#snmp)

## ICMP
- [ICMP](#icmp-notes)

## Simple TCP/IP Services
Windows comes with a feature called "Simple TCP/IP Services" which includes an echo, discard,
character generator, quote of the day, and daytime service. This is mostly for testing network
connectivity and serves no real purpose beyond testing from what I can tell.

The quotes can be changed to Mike "Kid Dynamite" Tyson quotes by editing %systemroot%\System32\Drivers\Etc\Quotes

## Echo
- _nc host_here 7_
- Pretty useless, but present for network troubleshooting purposes on older hosts.
- Often an indicator of a poorly-managed or default installation.
- Should respond with whatever data you send to it.

## FTP
- _ftp_ command.
- Check for anonymous/user@host.com
- Might have writable directories.
- Might have interesting files available via anonymous access.
- Might have weak accounts that have full filesystem access.
- Can often brute force accounts.
- May be able to enumerate usernames depending on FTP server.
- FTP bounce port scanning _nmap -b ftp_server_here allports.alpha-draconis.com_
- Plaintext protocol. May be able to sniff passwords.
- OS/distro can often be determined based on the banner.
- Filezilla.xml files may contain saved credentials.

## SSH
- _ssh_ command.
- Adding -vvvv to _ssh_ command will show authentication methods, banner string, and encryption algorithms available.
- Can often be brute forced.
- Try known/default keys.
- Search for keys on other systems. You may be able to find an rsync or nagios key that lets you into more systems (see sshady tool).
- Banner often correlates with distro/release.
- May be able to run sshd with LD_PRELOAD hook for crypt() or with a custom PAM module to obtain more user's passwords after you've rooted the machine.

## Telnet
- _telnet_ command
- _nmap -p 23 -sV telnet_server_here_ is very accurate.
- OS and other identifying info can be determined with session negotiation data (read RFC 854).
- Often states exactly what a device is in the banner.
- Can often brute force accounts.
- Often have default credentials.
- Plaintext protocol. Can often sniff passwords.
- Open sessions may be able to be hijacked in certain circumstances.
- Device type can be determined by username/password prompts in some cases. For example, the following is a typical Cisco prompt:
```
User Access Verification

Username: dmfr
Password:
```

## SMTP
- Used for sending email.
- Banner may reveal details about the underlying OS.
- May be able to send mail to another internal mail server.
- May be able to enumerate usernames.
- Example sending email using netcat:
```
 % nc terrible.host 25
220 terrible.host ESMTP Postfix
helo blah.com
250 terrible.host
mail from:<daniel@localhost>
250 2.1.0 Ok
rcpt to:<daniel@terrible.host>
250 2.1.5 Ok
data
354 End data with <CR><LF>.<CR><LF>
Subject: lol
asdf
.
250 2.0.0 Ok: queued as 6B8F243186
quit
221 2.0.0 Bye
```

## Finger
- Not used very often anymore, but still can be found on legacy and newer embedded systems.
- Can gather useful information such as names, addresses, phone numbers, email addresses, login shell, last time they logged in, ...
- Users of a system with fingerd can create a .plan file in their home directory to display after their GECOS information. People used to put information such as their schedules and if they were on vacation in here. This practice is not very common anymore.

- Display informational page or all logged in users (depends on fingerd implementation):
```
finger @IP_ADDRESS_HERE
```

- Display information about a specific user:
```
finger user@IP_ADDRESS_HERE
```

- Some fingerd implementations support ambiguous searching. You can use this to gather a user list on a system:
```
finger a@IP_ADDRESS_HERE
finger b@IP_ADDRESS_HERE
...
finger z@IP_ADDRESS_HERE
```

- You can also just use netcat:
```
nc IP_ADDRESS_HERE 79
<ENTER>
...
all logged in users
...

nc IP_ADDRESS_HERE 79
admin<ENTER>
...
admin's information
...
```

- You can use finger locally:
```
% finger daniel
Login: daniel                           Name: Daniel Roberson
Directory: /home/daniel                 Shell: /bin/zsh
On since Tue Mar 20 07:01 (PDT) on pts/0 from 192.168.1.101
   2 seconds idle
No mail.
No Plan.
```

## HTTP
- Use a browser or curl.
- Get server information with curl:
```
curl -I http://host_here
```
- If the site is running SSL/TLS (https), use -k flag with curl for server banner:
```
curl -kI https://host_here
```
- Can use netcat if its not using SSL:
```
nc host 80
GET / HTTP/1.0 ### PRESS ENTER TWICE
```
- Can use HTTP/1.1 for a persistent connection:
```
nc host 80
GET / HTTP/1.1 ### PRESS ENTER TWICE
GET /robots.txt HTTP/1.1 ### PRESS ENTER TWICE
```
- Determine which options are available (doesn't always work!):
```
curl -iX OPTIONS allports.alpha-draconis.com
```
- Usually on port 80, but commonly found on alternative ports.
- Multiple instances of HTTP servers can be found on a single host, all running different software
- Huge field of study; dozens of vulnerability classes
- nikto is a fast and noisy web vulnerability scanner
- robots.txt may reveal useful information.
- archive.org may have a historical archive of a public site that may be useful during reconniassance.
- dirb attempts to find directories such as /backup, /dev, /secret, etc
- Check for WebDAV:
    - OPTIONS method
    - PROPFIND method
    - If found, interact with WebDAV using cadaver
- Check for editor scratch files: /index.html~, /#index.html#, ...
- Check for .old, .bak, .backup, etc: /index.html.old, /index.html.bak, ... This may reveal source code for the web page.
- Check for a .git directory
- Google dorking
- View cached copies to avoid being logged visiting a site.

## SNMP
- _snmpget_ and _snmpwalk_ commands
- _onesixtyone_ scans for hosts responding to a list of communities.
- Check for public/private default communities.
- Often community names are default.
- SNMPv1 can be sniffed, revealing the community names.
- Hosts with open public communities can divulge a lot of information: OS, processes running, netstat data, logs, ... 
- Hosts with open private commmunities can have their configurations changed.
- UDP can be spoofed, which may be useful while attacking SNMP.

## MS-SQL
- https://www.npmjs.com/package/sql-cli
- Sometimes, applications have default database credentials or can be discovered. Poke around.
- If you manage to access the database, try to find login/user tables. 
- May be able to use xp_cmdshell stored procedure to execute shell commands as the database user.

## NFS
- showmount -e IP_ADDRESS_HERE
- If the export is available to you, mount -t nfs IP_ADDRESS_HERE:/export /mnt

## Cisco Smart Install
- https://github.com/Sab0tag3d/SIET
- Can dump the device's configuration, modify existing configuration, and possibly firmware updates remotely with no authentication.

## EtherNet IP
- https://en.wikipedia.org/wiki/EtherNet/IP
- Used in ICS/SCADA stuff
- https://github.com/EIPStackGroup/OpENer has an open source EtherNet IP stack.
- https://github.com/digitalbond/Redpoint has ICS/SCADA enumeration scripts
- TODO: explore this further. 

## ICMP Notes
- Commonly used to test connectivity via _ping_ (ICMP ECHO)
- Many administrators erroneously think that ICMP is solely for the ping and traceroute utilities.
- Windows tracert uses ICMP, Linux and BSD traceroute uses UDP.
- Can close existing TCP sessions with specially crafted ICMP unreachable packets (blind connection reset attack): https://www.owasp.org/images/2/2b/ICMP_Attacks.pdf
- Multiple DoS attacks exist that use ICMP: Smurf, Black Nurse, WinNuke, ...
- Can potentially identify which software is pinging you. Linux and Windows use different patterns in their payloads. Some monitoring software advertises itself within the payload. Check this with a sniffer.
- Can be used for transferring files and shell services if an organization isn't watching ICMP traffic:
    - https://github.com/droberson/icmp-exfil
    - https://github.com/JeremyNGalloway/LOKI2
- Had a tool similar to this used against my team successfully in an attack/defend CTF:
    - https://github.com/droberson/icmp-backdoor
