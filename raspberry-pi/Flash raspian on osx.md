https://coolaj86.com/articles/sd-card-for-raspberry-pi-on-os-x/

Insert disk into osx,
Find the disk with diskutil
```sh
    diskutil list
```
In this case the sd card is located at `/dev/disk2`

Unmount the disk 
```sh
    diskutil unmount /dev/disk2
```

Format the disk to remove any data
```sh
sudo diskutil partitionDisk /dev/disk2 1 MBR "Free Space" "%noformat%" 100%
```

Write the jesse image to the disk
```sh
sudo dd if=2017-03-02-raspbian-jessie-lite.img of=/dev/rdisk1 bs=1m
```

To find out the status of the writing press `ctrl+t`