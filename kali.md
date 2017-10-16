# Kali Linux notes
Here are some of the things I typically do on a new installation of Kali Linux:

```
apt update && apt upgrade
apt autoremove

## If running as VMware guest:
apt install open-vm-tools-desktop

## Install some of the tools I use:
apt install emacs shutter sshuttle
```

## Reverse direction of scroll wheel
```
gsettings set org.gnome.desktop.peripherals.mouse natural-scroll false
gsettings set org.gnome.desktop.peripherals.touchpad natural-scroll false
```
