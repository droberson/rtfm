# Kali Linux notes

## Running as a guest:
```
## If running as VMware guest:
apt install open-vm-tools-desktop

## If running as VirtualBox guest:
apt install virtualbox-guest-x11
```

## Reverse direction of scroll wheel
```
gsettings set org.gnome.desktop.peripherals.mouse natural-scroll false
gsettings set org.gnome.desktop.peripherals.touchpad natural-scroll false
```


## Fixing EXPKEYSIG error in apt
```
gpg --keyserver hkp://keys.gnupg.net --recv-key 7D8D0BF6
gpg --fingerprint 7D8D0BF6 ### verify output of this command before proceeding

gpg -a --export 7D8D0BF6 | apt-key add -
apt update
```
