# Sumber tutorial
https://www.youtube.com/watch?v=hC3DCcmfrag

# Install RDP Server di Linux Mint
gunakan list cli ini
```
    sudo apt upgrade
    sudo apt install xrdp xorgxrdp
    sudo apt install xserver-xorg-input-all
    sudo adduser xrdp ssl-cert
    sudo ufw allow 3389/tcp
    echo env -u SESSION_MANAGER -u DBUS_SESSION_BUS_ADDRESS cinnamon-session~/.xsession
```
