# Raspberry Pi setup with wifi
Useful for a headless setup, when you don't have a screen, keyboard, or ethernet cable.

- Download [latest image of Raspbian](https://www.raspberrypi.org/downloads/raspbian/) 
- Burn image to SD-card using [Etcher](https://etcher.io)

- Adding [empty file `ssh`](files-for-boot-partion/ssh) to the boot image will *enable ssh on the first boot*.

  (Read [ssh docs](https://www.raspberrypi.org/documentation/remote-access/ssh/) for background)

- Adding [file `wpa_supplicant.conf`](files-for-boot-partion/wpa_supplicant.conf) to the boot image will
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
