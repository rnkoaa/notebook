# How to rename a default pi user

Raspbian comes with a default user with username `pi` and password `raspberry`. This is not secure as anyone can get into the server quickly.

[Detailed instructions on changing user.](http://unixetc.co.uk/2016/01/07/how-to-rename-the-default-raspberry-pi-user/)

* For Desktop based Raspbian OS use the following command to update the username

```sh
tempuser@pi /etc $ sudo tar -cvf authfiles.tar passwd group shadow gshadow sudoers lightdm/lightdm.conf systemd/system/autologin@.service sudoers.d/* polkit-1/localauthority.conf.d/60-desktop-policy.conf
```

* For non Desktop based raspbian os use the following commands

```sh
$ cd /etc

# Become root and run the following commands.
$ sudo su

tar -cvf authfiles.tar passwd group shadow gshadow sudoers systemd/system/autologin@.service sudoers.d/*

sed -i.$(date +'%y%m%d_%H%M%S') 's/\bpi\b/piadmin/g' passwd group shadow gshadow sudoers systemd/system/autologin@.service sudoers.d/*

mv /home/pi /home/piadmin

ln -s /home/piadmin /home/pi
```

* Verify that the user exists in the group by using the following commands

```sh
grep piadmin /etc/group
```

Now, logout of the `tempuser` and log back in as the new `piadmin` user. then delete the `tempuser`.

```sh
sudo userdel tempuser
```
