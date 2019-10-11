# Raspberry Pi setup with wifi

Useful for a headless setup, when you don't have a screen, keyboard, or ethernet cable.

- Download [latest image of Raspbian](https://www.raspberrypi.org/downloads/raspbian/) 
- Burn image to SD-card using [Etcher](https://etcher.io)

- Adding [empty file `ssh`](files-for-boot-partition/ssh) to the boot image will *enable ssh on the first boot*.

  (Read [ssh docs](https://www.raspberrypi.org/documentation/remote-access/ssh/) for background)

- Adding [file `wpa_supplicant.conf`](files-for-boot-partition/wpa_supplicant.conf) to the boot image will
*configure wifi on the first boot*:

  Keep these three lines at the top (set your own country code):
   
  ```
  country=SE
  ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
  update_config=1
  ```
  
  (Consult [this forum thread](https://www.raspberrypi.org/forums/viewtopic.php?t=191252) for background)

- Add [relevant network config](https://www.raspberrypi.org/documentation/configuration/wireless/wireless-cli.md)
to bottom of `wpa_supplicant.conf` on the boot image.

  Example:
  
  ```
  network={
    ssid="yourSSID"
    psk="yourPassword"
  }
  ```

- Power up

- `ssh pi@raspberrypi.local`

  Initial password: raspberry

  If you have trouble finding the ip,
  [read the ip-guide](https://www.raspberrypi.org/documentation/remote-access/ip-address.md)
  …or try `ping -c 1 raspberrypi.local`
  …or try `arp -a`

- Complete [Security setup](https://www.raspberrypi.org/documentation/configuration/security.md)

## Installing other useful tools

### The `locate` command

- `sudo apt-get install mlocate`
- `sudo updatedb`

### DHCP Server

If you need the ethernet port of the Raspberry Pi to expose a DHCP server:

- Configure `eth0` to use (or fallback to) static IP `10.254.239.1/27` by editing `/etc/dhcpcd.conf`
- Connect an ethernet cable to another network device and run `ip -4 addr show` to check that the static IP has been set
- Install `dhcpd` using `sudo apt-get install isc-dhcp-server`
- Edit `/etc/dhcp/dhcpd.conf` and configure at least one `subnet` for `10.254.239.0/27` with a `range` *not* containing `10.254.239.1`
- Edit `/etc/default/isc-dhcp-server` to uncomment both `DHCPDv4_CONF` and `DHCPDv4_PID`, and add `eth0` to `INTERFACESv4`
- Test the config with `dhcpd -t`
- Start the service using `sudo service isc-dhcp-server start`
