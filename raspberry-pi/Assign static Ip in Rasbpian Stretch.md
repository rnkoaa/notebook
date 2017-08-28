# To assign static Ip to A raspberry pi in Raspbian Stretch version

* Do an `ifconfig` to find out the interface to use since `stretch` has changed the naming of interfaces.
* Write down the name of the interface.
* Open `/etc/dhcpcd.conf` and update it as follows  
  * `sudo vim /etc/dhcpcd.conf`
  * Update the static Ip section with the following example
    ```sh
    interface enxb827ebxxxxx
    static ip_address=192.168.1.120/24
    static routers=192.168.1.1
    static domain_name_servers=192.168.1.1 8.8.8.8
    ```
  * Reboot for the configuration to take effect or restart the dhcp service