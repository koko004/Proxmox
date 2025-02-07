# Proxmox
![image](https://github.com/user-attachments/assets/f89aaeb6-fdc7-4c0a-a25d-c915b26df264)

### - Set WOL (Wake On Lan)

Install `ethtool` and set wol on all interfaces
```
apt-get install ethtool -y
ethtool -s enp2s0 wol g && ethtool -s enp1s0 wol g
ethtool -s vmbr0 wol g && ethtool -s vmbr1 wol g
```

Create a file in systemd `nano /etc/systemd/network/90-wakeonlan.link` with the content:
```
[Match]
MACAddress=<mac-address-here>

[Link]
NamePolicy=kernel database onboard slot path
MACAddressPolicy=persistent
WakeOnLan= magic

```

How to see configuration of interface
```
ethtool <interface-name> | grep Wake-on
```
