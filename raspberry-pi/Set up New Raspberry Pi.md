* Format disk
* Flash raspbian os onto disk.
* plug flash memory into raspberry and boot.
* username is `pi` and password is `raspberry`

## Update the keyboard Locale
Update the line `XKBLAYOUT` to read `XKBLAYOUT="us"`, Then reboot the pi for the new keyboard layout to take effect.

## Change the host name of the pi machine
change the name from `raspberrypi` to the name of the role for this pi, eg `node1` if it is going to be a part of a cluster.
in the following files

    /etc/hosts
    /etc/hostname

## Change Memory split
Since we will not be using any gui features, we do not need that much memory for the graphics, so we can reduce the gui memory.
* Go to `raspi-config` menu.
```sh
sudo raspi-config
```
* Select the `Advanced Options` menu.
* In the `Advanced Options` menu, select `A3 Memory Split` sub section. then change the value from the default 64 to 16.
* Exit and reboot for the changes to take effect.

## Configure Wifi ##
To configure wifi, we are going to use `wpa_supplicant` which already comes withe version of raspbian.
* First take a backup of the `wpa_supplicant.conf` file.
```sh
sudo -s
cp /etc/wpa_supplicant/wpa_supplicant.conf /etc/wpa_supplicant/wpa_supplicant.conf.bak

# note that we are going to recreate it from scratch and so we do not need to use the old framework
rm /etc/wpa_supplicant/wpa_supplicant.conf

wpa_passphrase "Name of wifi" > /etc/wpa_supplicant/wpa_supplicant.conf
# After this command, you will be required to type in the password for the wifi.
```
### Configure DHCP 
* Change `/etc/network/interfaces` to look like this.
```sh
auto lo
iface lo inet loopback

iface eth0 inet dhcp

auto wlan0
allow-hotplug wlan0
iface wlan0 inet dhcp
wpa-conf /etc/wpa_supplicant/wpa_supplicant.conf
```
* Restart the dhcp service for the wifi to connect and receive an ip address
```sh
# scan the wifi's in the area to find your wifi
sudo iwlist wlan0 scan | grep wifi name
iwconfig
sudo dhcpcd wlan0
wpa_supplicant -B w -D wext -i wlan0 -c /etc/wpa_supplicant/wpa_supplicant.conf
```
* if the pi does not connect, reboot for the connection to take place.

### Configure static Ip 
Change `/etc/network/interfaces` to look like this.
```sh
auto lo
iface lo inet loopback

iface eth0 inet dhcp

auto wlan0
allow-hotplug wlan0
iface wlan0 inet static
    address 192.168.1.166 # or any other ip address you want
    netmask 255.255.255.0
    gateway 192.168.1.1     # address of your router
    network 192.168.0.0
    broadcast 192.168.1.255
wpa-conf /etc/wpa_supplicant/wpa_supplicant.conf
```

* Reboot for the connection to take place or try any of the following for the machine to connect to its wifi.
```sh
# scan the wifi's in the area to find your wifi
sudo iwlist wlan0 scan | grep wifi name
iwconfig
wpa_supplicant -B w -D wext -i wlan0 -c /etc/wpa_supplicant/wpa_supplicant.conf
```
* if the pi does not connect, reboot for the connection to take place.

### Set Default gateway
```sh
sudo route add default gw 192.168.1.1
```

## Enable ssh to start on Boot
Execute the `raspi-config` command to bring the configuration menu
```sh
sudo raspi-config
```
* Select the `Interfacing Options` menu.
* In the `Interfacing Options` menu, select `P2 SSH` sub section. Then, answer yes for the ssh server to be enabled.
* Exit and reboot for the changes to take effect.

When the menu opens go to `Advanced Options` menu.

## Rename PI user for security
Assign password to the root account
```sh
 sudo passwd root
```
Then log out. once logged out, log back in as root. and change the user name as follows:
```sh
# usermod -l myuname pi
# usermod -m -d /home/myuname myuname
```
where `myuname` is the new name you wish to change the `pi` user to. finally log out then log back in as the new user `myuname`

Disable the root account or lock it with the following account for extra security
```sh
 sudo passwd -l root
```

## Disable password authentication for ssh.