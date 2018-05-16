  # Linux Privilege Escalation/Post exploitation
  
  In no particular order, try these things:
  
  ## sudo
  Check which commands, if any, the current user can execute with sudo:
  ```
  sudo -l
  ```
  
  This will give you some output similar to this:
  ```
  [sudo] password for daniel:
  Matching Defaults entries for daniel on localhost:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

  User daniel may run the following commands on localhost:
    (ALL : ALL) ALL
   ```
  
  If there is an ALL line in the output similar to above, you can get a root shell:
  ```
  sudo -s
  ```
  
  If the administrators have done only allowed a user to use a specific set of commands, you may be able to
  leverage one of them to gain a shell. Examples of this are pagers such as _more_. You may be able to _!/bin/bash_
  and have it execute a shell as root.
  
  Notice the settings _env reset_ and _mail badpass_. The administrator may have set up additional restrictions.
  Read the manual page for _sudoers_ for more information. In this particular case, if you input a bad password,
  the systems administrator will receive an email saying that you failed the password attempt. This is likely to get
  you discovered.
  
  The _env reset_ setting, if not present, will allow you to abuse the PATH environment variable, which may yeild
  an easy win when executing shell scripts with sudo.
  
  ## Weak/reused passwords.
  
  Try to find hashes/passwords already present on the system or elsewhere on the network and try them against
  other services. This may be very noisy and get you discovered, however.
  
  ## SSH keys
  
  Check each users ~/.ssh for weak/passwordless keys and try them elsewhere.
  
  ## Check for running ssh agents.
  
  Might be able to hijack one and login to other machines, or login as root w/ key locally.
  
  ## uname -a
  
  Search for CVEs or use linux-exploit-suggester for kernel exploits.
  
  ## Check /etc/issue*, /etc/*release*, ...
  
  Figure out which distro and version is running. May be easy as Googling "Distro Version exploit."
  
  ## env/set
  
  Show environment variables. You might get lucky and have a password, key, or something nice in there.
  
  ## ps aux
  
  Review everything that is running, especially as root. For example, mysql may be running as root, which may lead to
  being able to read root-owned files.
  
  ## netstat -tulpn
  
  Review all of this. There may be something listening localhost only that is vulnerable.
  
  ## find / -perm -04000 -exec ls -l {} \; 2>/dev/null
  
  Find suid files. These may have issues that can be leveraged to get root or elevate your privileges to some
  other service. Figure out versions (--help, --version, dpkg -l |grep package, ...) Cross-reference exploitdb or
  CVE searches.
  
  ## find / -perm -o+w
  
  Find world-writable files. You may be able to edit these in some way to elevate privileges. Good examples are
  configuration directories. Some lesser experienced sysadmin will chmod -R 777 /directory if they are having
  issues with permissions. You can then go in and add/modify configuration settings.
  
  ## Check /var/www (or wherever wwwroot is)
  
  Might have plaintext credentials in config files.
  
  ## Check /var/spool directory for stuff.
  
  Might have emails containing passwords, keys, etc. May have useful info in crontabs.
  
  ## Loot people's home directories.
  
  You might get lucky and score a password in their .bash_history or even a spreadsheet full of credentials. There
  may be emails, pcaps, notes, software, keys, etc that will be interesting as well.
  
  ## Check software logs.
  
  You'd be surprised what you find in here, too. Highly situational and depends on what they are running and how it
  is configured.
  
  ## If possible, sniff traffic.
  
  Other accounts may be authenticating using plain-text protocols. 
  
  ## If feasible, install a keylogger, clipboard sniffer, or some other malware to run as the user.
  
  Use your imagination here. Might be able to catch them logging in and giving you something nice.
  
