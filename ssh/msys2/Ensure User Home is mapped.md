# Mapping user home in msys2

In `/etc/fstab` Add the following lines to ensure that ssh config can be found

```sh
C:/Users/$USER /home/$USER ntfs binary,posix=0,user 0 0
```

or

```sh
C:/Users/ /home/ ntfs binary,posix=0,user 0 0
```
