# Interacting with services

Here are some notes about interacting with various services. Often, the correct tool or information
that can be gleaned isn't immediately apparent.

## TCP

- [7 / echo](#echo)
- [21 / FTP](#ftp)
- [22 / SSH](#ssh)
- [23 / Telnet](#telnet)
- [1433 / MS-SQL](#ms-sql)

## UDP

- [161 / SNMP](#snmp)

## Simple TCP/IP Services
Windows comes with a feature called "Simple TCP/IP Services" which includes an echo, discard,
character generator, quote of the day, and daytime service. This is mostly for testing network
connectivity and serve no real purpose beyond testing from what I can tell.

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
