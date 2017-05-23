Raspbian comes with a default user with username `pi` and password `raspberry`. This is not secure as anyone can get into the server quickly.

[Detailed instructions on changing user.](http://unixetc.co.uk/2016/01/07/how-to-rename-the-default-raspberry-pi-user/)
```sh
tempuser@pi /etc $ sudo tar -cvf authfiles.tar passwd group shadow gshadow sudoers lightdm/lightdm.conf systemd/system/autologin@.service sudoers.d/* polkit-1/localauthority.conf.d/60-desktop-policy.conf
```

```sh
sudo tar -cvf authfiles.tar passwd group shadow gshadow sudoers systemd/system/autologin@.service sudoers.d/* 

sudo sed -i.$(date +'%y%m%d_%H%M%S') 's/\bpi\b/piadmin/g' passwd group shadow gshadow sudoers systemd/system/autologin@.service sudoers.d/*

sudo mv /home/pi /home/piadmin

sudo ln -s /home/piadmin /home/pi

sudo userdel tempuser
```