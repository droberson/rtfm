# Setting up the database
```
# systemctl start postgresql
# msfdb init
# msfconsole
msf> db_status
... should say connected to msf ...
```

# Quick example of Metasploit Resource Script
Save this as search_cve.rc:
```
<ruby>

cvefile="CVEs.txt"
cves=[]

File.open(cvefile, "r") do |f|
	f.each_line do |line|
		cves.push line.strip
	end
end

cves.each do |cve|
	self.print("Searching for #{cve}")
	self.run_single("search #{cve}")
end
</ruby>
```
Execute the script:
```
msfconsole -r search_cve.rc
```
