# Virtualization Notes!

## RedHat 5.1 on VMWare Workstation
I had a need for an old version of RedHat. Installing this on VMWare Workstation was a bit of a challenge:

- Used redhat_5.1_i386.iso
- Removed SCSI hard drive
- Added IDE hard drive
- Installed like normal
- Could not find an option to change the network adapter chipset via the GUI. Ended up manually adding a line to the VM's .vmx:
```
ethernet0.virtualDev = "vlance"
```
- Booted machine. Had to insert kernel module for this card to work:
```
modprobe pcnet32
```
- Got an address via DHCP:
```
dhcpcd eth0
```
- Success! I can now use Linux like its 1997.
