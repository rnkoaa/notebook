# Add remote wifi

After cloning image to microsd card, mount the disk  then create file which `ssh`

```
touch /Volumes/boot/ssh
```

```
country=US
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
update_config=1

network={
    ssid="NETWORK-NAME"
    psk="NETWORK-PASSWORD"
}
```
