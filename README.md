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
Edit `/etc/interfaces` with `nano /etc/network/interfaces` and add line `post-up /sbin/ethtool -s <interface name> wol g` below to the label of the physical interface like:
```                                                       
auto lo
iface lo inet loopback

post-up /sbin/ethtool -s enp8s0 wol g

iface enp8s0 inet manual

auto vmbr0
iface vmbr0 inet static
        address 192.168.1.249/24
        gateway 192.168.1.1
        bridge-ports enp8s0
        bridge-stp off
        bridge-fd 0


source /etc/network/interfaces.d/*
```

How to see WOL configuration of interface. If Wake-on: pumbg, “g” means WOL is supported
```
ethtool <interface-name> | grep Wake-on
```
